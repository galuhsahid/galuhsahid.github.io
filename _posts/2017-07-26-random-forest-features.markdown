---
layout: post
title:  "Positive and Negative Feature Contributions of Random Forest with TreeInterpreter"
date:   2017-07-26
tag: machine learning
blog: true
---

A few weeks ago I was building a model to estimate the amount of donation reached by social project campaigns[^1]. The caveat was aside of estimating, we also have to know how the features contribute to the prediction. This is because we want the estimator to also inform people what to do to increase the amount of funding for their social projects campaign. Saying “the number of images contribute to the funding you receive” isn’t really helpful, isn’t it? Do more images drive people to donate more, or is it in fact the other way around? I learned that this makes a huge difference as not all methods will give you the answer to this question--and oftentimes, these methods are the ones that yield the best result. 

It’s quite simple if we’re thinking linear regression, since from one look at the coefficients we can already tell if a certain feature actually increases the value of the target variable or otherwise. But not so much with models using random forest and gradient booster--the methods that, as I’ve said before, often yield the best results. In my case, when evaluated using root mean squared error (RMSE), random forest did give the best result in my case, although just a shy away from gradient booster. Both outperformed linear regression, ridge regression, and lasso regression by a significant margin. 

I have to admit that prior to this, I never quite understood that we still have more work to do in terms of the interpretation of machine learning results. While we may have reached significant progress in predicting, in some cases the process itself is still quite a mystery[^2]. What is surprising is that although random forests have been proven to be accurate and have seen extensive use in various applied researches, there is not much work on the theoretical explanation of random forest yet. 

## Feature Importances in Scikit-Learn
If you have read the scikit-learn documentation on random forests, you may have noticed that there is, in fact, [a way to get the feature importances of a random forest model](http://scikit-learn.org/stable/auto_examples/ensemble/plot_forest_importances.html). Feature importances implemented in the scikit-learn library will tell you which features are most informative, so that’s a step forward towards answering the question “how do these features influence the model”. Knowing the importance of these features decreases the uncertainty and helps in making more accurate prediction, but that’s it. Depending on the purpose of your model though, it might not be a complete answer yet, which happens in my case.

Scikit-learn’s implementation followed one of the methods proposed by Breiman, which is called mean decrease impurity (MDI). The mean decrease impurity calculated the sum of of decrease in node impurity, which can be defined for any kind of impurity measure. Another alternative is by measuring the mean decrease accuracy (MDA) of the forest, which uses out-of-bag (OOB) samples to measure accuracy. It is sometimes referred as the permutation importance because the MDA refers to the accuracy when the values of the variable are randomly permuted in the OOB samples[^3]. So it is based on the hypothesis that if a variable is not important, then it will not degrade the accuracy of a prediction. Through experiments it had been shown that both have their own biases--MDI has shown biases towards predictors with a large number of values while MDA tends to overestimate correlated variables.

I calculated the scikit-learn’s feature importances and visualized them using matplotlib with the code below:

```python
coef = pd.Series(rfModel.feature_importances_, index = X_train.columns)
imp_coef = coef.sort_values()
matplotlib.rcParams['figure.figsize'] = (8.0, 10.0)
imp_coef.plot(kind = "barh")
plt.title("Feature Importance in Random Forest")
```

![feature importances]({{ site.url }}/assets/images/random_forest_feature_importance.png)
	    
From scikit-learn’s feature importances, we know that the feature img_cnt (number of images in a campaign page) contributes the most. But it doesn’t say anything about whether fundraisers should add more images or in fact do the opposite. 

Another alternative is to create partial dependence plots to see how our targeted value changes as we change our variables. However, depending on plots mean we are most likely restricted to one or two variables at most due to the limitation of human perception. Since plotting and analyzing all plots for each variable was going to be quite time-consuming, I decided to try find another alternative that I can use to at least gain some analysis from.

## An Alternative: TreeInterpreter
A few searches and readings later, I stumbled upon a post by [DataDive](http://datadive.net) titled [“Interpreting Random Forests”](http://blog.datadive.net/interpreting-random-forests/). Long story short, they interpreted random forest using what they call as feature contributions. 

The full derivation is not described in the post, but the basic idea is to present every prediction as a sum of feature contributions. If it sounds like linear regression to you, then I guess it is quite similar in a way, since this idea aims to serve interpretability that is on a similar level to linear model.

This idea can be generalized to other methods related to decision trees, although perhaps a few adjustments needed to be made. This is possible because the idea itself is derived from decision trees before it is expanded to random forest. For random forest, the sum of feature contributions for each prediction can be described as: 

$$F(x) = \frac{1}{J}{\sum\limits_{j=1}^J {c_{j}}_{full}}  + \sum\limits_{k=1}^K (\frac{1}{J}\sum\limits_{j=1}^J contrib_j(x, k))$$


In the equation above, \\(J\\) is the number of trees, \\(c_{j_{full}}\\) is the value at the root of the node for each \\(J\\)-th tree, \\(K\\) is the number of features involved, and \\(contrib(x, K)\\) is the contribution from the \\(K\\)-th feature in the feature vector \\(x\\).

The great news is they already have [a Python package](https://github.com/andosa/treeinterpreter) available to install, which you can get by simply running `pip install treeinterpreter` in your terminal.

Once installed, I imported the package to my Jupyter notebook and defined my instances:

```python
from treeinterpreter import treeinterpreter as ti

instances = X_train
```

Instances are basically the rows of data that you would like to use. I have mine in the Pandas dataframe X_train, therefore I set `instances = X_train`.

Next up:

```python
prediction, bias, contributions = ti.predict(rfModel, instances)
```

The `predict` method of `TreeInterpreter` will give you three values: prediction, bias, and contributions. To calculate the contributions we will only need the contributions. However, if you want to check whether the contributions returned sum up, you can do so by using the following code:

```python
assert(numpy.allclose(prediction, bias + np.sum(contributions, axis=1)))
assert(numpy.allclose(rfModel.predict(X), bias + np.sum(contributions, axis=1)))
```

Now you've got the contributions of your random forest model. The contributions returned by `TreeInterpreter` is a list of lists where each of this list refers to each instance. This particular list then contains the contributions from each feature that is defined in your instances dataframe. If you want to see how **each feature** contributes to **each instance** of prediction, you can loop through each instance in the contributions:

```python
for i in range(len(instances)):
    print "Instance", i
    for contribution, feature in sorted(zip(contributions[i], 
                                 list(X_train)), 
                             key=lambda x: -abs(x[0])):
        print feature, round(contribution, 2)
    print "-"*20 
```

This will return an output like the following:

```
Instance 1
img_cnt 3.21
fundraiser_cnt 2.51
fb_share_count 1.23
fb_comment_count 4.21
update_cnt -0.26
...

Instance 2
img_cnt 3.21
fundraiser_cnt 2.51
fb_share_count 1.23
fb_comment_count 4.21
update_cnt -0.26
...
```

What if you want to know how each feature is doing to the **entire model**? I simply calculated the mean of each feature's contributions to every instance. This can be done by making a minor modification to the code above. Instead of printing the results out, I created a dictionary where each key is the feature and is each value is a list of contributions from that feature to each instance.

```python
feature_dict = dict((k,[]) for k in list(X_train))
```

```python
for i in range(len(instances)):
    for c, feature in sorted(zip(contributions[i], list(X_train)), key=lambda x: -abs(x[0])):
        feature_dict[feature].append(round(c,2))
```

Then, I calculated the mean of each list:

```python
for feature in feature_names:
    feature_mean_dict[feature] = np.mean(feature_dict[feature])
```

To see how each feature is doing, print each item in the dictionary and sort them:

```python
sorted(feature_mean_dict.items(), key=lambda x: x[1], reverse=True)
```

This is what I got:

```
[('fb_share_count', 2.4745585048754064),
 ('img_cnt', 1.0555065005417119),
 ('fb_comment_count', 0.51212080173347785),
 ('fundraiser_cnt', 0.28633261105092084),
 ('fb_reaction_count', 0.11352112676056339),
 ('update_cnt', 0.10127573131094257),
 ('avg_weight', 0.034623510292524376),
 ('max_reward', 0.023735102925243768),
 ('story_wc', 0.021405742145178767),
 ('vid_cnt', 0.0099187432286023842),
 ('is_org', 0.0050595882990249188),
 ('reward_cnt', 0.0026652221018418202),
 ('donation_target_amt', 0.0),
 ('min_reward', -0.0033071505958829905),
 ('title_wc', -0.010289815817984833),
 ('short_wc', -0.017594799566630556)]
```

It turns out the features min_reward, title_wc, and short_wc are contributing negatively to our model. Although the values are so small that they're insignificant, this is still an information that we don't get from scikit-learn's feature importances.

You may also quickly notice that the orders between the two results above are slightly different, which to me is expected since they're calculated using different methods anyway. According to scikit-learn's feature importances, the feature img_cnt contributes the most, whereas according to TreeInterpreter, the feature fb_share_count contributes the most.

However, I ended up choosing TreeInterpreter's results for intepretability's sake&mdash;I can be sure how each feature is contributing to the model. Besides, the top six features displayed by both results are still the same features, so I guess it's not too bad.

Of course, we can also make the visualization of our result above using matplotlib:

```python
features = list(feature_mean_dict.keys())
```

```python
values = list(feature_mean_dict.values())
```

```python
coef = pd.Series(values, index = features)
imp_coef = coef.sort_values()
matplotlib.rcParams['figure.figsize'] = (8.0, 10.0)
imp_coef.plot(kind = "barh")
plt.title("Feature Contributions in Random Forest Model")
```

<figure class="figure">
    <img class="figure-image" src="/images/posts/random-forest/feature-importance-ti.png" alt="A picture of a chart that shows the features of a random forest model ordered by its feature contribution.">
    <figcaption class="figcaption">
        <span class="figcaption__text">An example of a random forest model ordered by each of its feature contribution, calculated using TreeInterpreter.</span>
    </figcaption>
</figure>

[^1]: This calls for another post or at least a GitHub repo. The code is still a huge mess &amp; long, comprehensive explanations might be in order. I just haven't got around to it yet, so watch this space!
[^2]: [Understanding Random Forests &mdash; From Theory to Practice](https://arxiv.org/pdf/1407.7502.pdf).
[^3]: [Understanding Variable Importances in Forests of Randomized Trees](https://papers.nips.cc/paper/4928-understanding-variable-importances-in-forests-of-randomized-trees.pdf).