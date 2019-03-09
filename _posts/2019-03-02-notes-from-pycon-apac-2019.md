---
layout: post
title:  "Notes from PyCon APAC 2019"
date:   2019-03-02
tags: [conference]
---

> Indonesian translation is available [here](/notes-from-pycon-apac-2019-id/)

A few weeks ago, I attended PyCon APAC 2019 in Manila. This was my second PyCon APAC, and this time I came as an attendee! Below I'm going to write down the keynotes/talks I've watched, what I learned, & what my key takeaways are. There were two tracks & I was mostly on the data track, so most of these talks were going to be from that track.

# Talks
## Keynote 1: PyCon APAC - Back to the Future! by Beng Keat Liew
In this keynote, Dr Liew Beng Keat talked about the evolution of the Python community around the APAC region. He founded the Python Singapore User Group initiated the first PyCon APAC (!) 10 years ago. I couldn't help but think: where was I 10 years ago? I was probably holed up in my room playing Stardoll.com. :P

I'm pretty much new to the Python community, & even today I still don't really pop up in meetups/conferences that often, just occassionally. So it was super interesting to see how the community evolved over time, and to know the initiators behind each community. I believe that these communities also play a great role in introducing Python to a wider range of people, which makes Python the programming language that we come to know & love today.

## Keynote 2: Interviewing as a Python engineer by Jacob Kaplan-Moss
There have been lots of talks about interviewing in tech companies---be it on Twitter or among my friends---so this talk is a very timely one I think! 

I have lots of key takeaways to bring home from the talk. One important thing that helps me understand the interviewing process is that, interview techniques are proxies that help companies *predict* how you will perform on the job. The best technique is to ask candidates to actually perform on the job, but that's unfeasible, so we resort to other techniques. None of these other techniques are perfect (which can also be a great reminder to not get discouraged when a company rejects you!). Some of these techniques are great predictors, some are hit-and-miss, & some are just terrible despite the fact that many companies are still doing it. One interesting thing that was mentioned is: you can infer the culture of a company by the types of interviews they choose for hiring---if they choose the terrible ones, then you can probably tell that they don't prioritize effective hiring. :)

He divided the types of interview techniques into three categories: **question/answer**, **coding challenges**, & **exercises**. 

In the Q/A category, behavioral interviews are great, & they are the ones that usually start with "tell me about a time...". Thankfully, I've never had someone ask *trivia* questions. These are specific technical questions that is probably a Google search away, & that's the point. If you can look it up on the docs, why do you need to remember the answer to "what sorting algorithm does Python use?". These questions don't say anything about how one would perform in their job, & if the company you're applying for asks these questions, then it actually says more about the company (& it's not saying a good thing).

For coding challenges, take-home exercises are great. I've never had take-home exercises before, but it does make sense that solving problems in a more realistic environment (aka, where no one's watching every single letter that you're typing) better predicts one's performance on a job than, say, whiteboarding. Coding challenges using online tools such as HackerRank are also red flags. They don't judge how you communicate---you can't think out loud, you're not given the chance to probe further... also, they don't judge your code quality. All they measure is whether you pass or not within the pre-defined time/space complexity.

And then there are exercises. There are lab exercises, where you have to perform a task in an environment that has been set up prior. This is a good sign because it's an effective technique, & since it takes lots of efforts, it shows that the company is serious about the hiring process. There are also starter projects, where you get to work together in an actual team. I've never been through both so I can't speak much about them.

Moving on from interview techniques to interviewing tips. The general advice goes as follows: a) prepare, b) ask questions, c) take notes. He mentioned a particular research on how taking notes on paper is way more effective than typing notes on, say, on your laptop. Most importantly, after every interview, take a time to reflect on how you did, as this could be useful for future interviews. It's also OK to ask for feedback, although you may not always get one. 

[Slides](https://jacobian.org/speaking/technical-interviews/)

## Keynote 3: How did you know? Explaining Black Box Model Predictions in Python by Suzy Lee
Another very timely talk, now that there are more conversations around the need to make models more interpretable & explainable. I also can relate to this talk so much because this is exactly the problem that I had two years ago, & I wasn't quite sure how to solve it. Having a pretty decent R-squared was cool, but that was not the end of it---we still had to find a way to explain the contributing factors behind our random forest model to answer our research questions, & a single number weren't enough to answer them.

Suzy Lee started with some damning statistics. I didn't take note of the exact number & sample, but most people still don't trust AI to handle financials & hiring processes. It's a huge reminder to people: no matter how cool your model is & how accurate your validation tells you, if people don't trust it, they won't use it. 

I also like how she really stressed the importance of explaining black box models by giving some concrete examples, because I guess it's an effective way to make people really *get* it. At least that's how it's worked for me. The first time I was really aware of how important this issue is was when I read Cathy O'Neil's *Weapons of Math Destruction*. Each chapter discusses one case, from how big data impacts online advertising to getting insurance, & at the end I got a strong sense of how terrible things will be if we don't do anything towards making models more interpretable. Suzy Lee gave her personal experience as an example---she was making a model for a bank, & all went smooth-sailing until she and/or her team was asked: why? Surely, institutions with certain regulatory compliance such as banks wouldn't accept an answer such as "the model says so". And as more & more parts of our lives are (whether we like it or not) being determined by such models, I believe that we as human beings deserve better answers & explanations.

From the talk, I learned about the **interpretability-accuracy tradeoff**. Techniques such as deep learning are able to handle more complex scenarios & thus can yield more accurate results than, say, linear regression. However, they can be very hard to interpret, in contrast with linear regression models that are more explainable. The more complex & accurate a model is, most of the time it becomes less interpretable.

I also learned about local & global interpretation, & I found the example helpful. Say that you're building a credit scoring model. Local interpretation tries to explain what variables make someone a bad borrower for *that* particular instance, while global interpretation tries to explain what variables make someone a bad borrower *in general*, not just for that particular instance.

Now, onto the tools! I wish I knew about this two years ago, because these tools would have saved me from a lot of headache. [Lime](https://github.com/marcotcr/lime), short for Local Interpretable Model-Agnostic Explanations, is able to explain any black box classifier with two or more classes. Lime does local interpretation---it generates fake data points around the instance & learn a linear model that approximates the model near the instance. [Here's a blog post](https://www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime) that elaborates LIME with more details.

There's also [Skater](https://github.com/datascienceinc/Skater), which is based on the idea that if you play with a feature & turns out the change affects your accuracy a lot, then the said feature contributes a lot to your model. Otherwise, it doesn't contribute much.

Next, there's [Shap](https://github.com/slundberg/shap)---short for SHapley Additive exPlanations. The interesting part is, it relies on game theory! In the context of explaining model predictions, the game is the prediction task. 

Last but not least, we have [ELI5](https://github.com/TeamHG-Memex/eli5), which does global interpretation. It supports lots of machine learning frameworks & package, from scikit-learn to XGboost.

I haven't had the chance to play around with these libraries yet, but I definitely will whenever I have the chance!

## Talk: A Framework for Transfer Learning on Multi-lingual Text Data for Sentiment Classification by Gyenn Neil Ibo
Having worked with mainly text data for both of my past researches, if there's something about Indonesia's informal text data that I really noticed, it is that we really, really mix Indonesian & English a lot. It turns out that this issue is also prevalent in the Philippines. Both of us also have a lot of different dialects, thus the necessity to have a framework that can handle multilingual text data for tasks such as sentiment classification.

The talk discusses how we can handle multi-lingual text data by merging word embedding (e.g. merging English + Filipino word embeddings). Word embeddings represent text data as numeric form, & they are based on the idea that words that are closer in meaning have vectors that are within proximity of each other. [I love word embeddings](http://indonesian-word-embedding.herokuapp.com/), so I'm excited to learn more about it!

To merge word embedding, there are 2 approaches: deriving an approximate rotation matrix, or by using the "hacker's approach". This talk focused on the "hacker's approach". The hacker's approach is comprised of a few steps: first, use the words that are present in both the standard word embedding & custom word embedding. Next, we compute the relative displacement vectors of the new/out-of-vocabulary words, & finally we project the positions of the new words into the vector space of the original word embedding.

So why do we want to merge word embeddings? By merging word embeddings, we can leverage pretrained word embeddings such as word2vec & GloVe. I remember that when I was working on my bachelor's thesis, I trained my own word embedding. However, my training data was too small to yield a good result. If only I thought of merging the word embedding that I trained with pretrained word2vec...

Another interesting point is this: language is constantly evolving, & this is especially true when we're working with informal texts such as texts found in social media. Merging word embeddings lets us "update" the standard word embedding. Plus, by merging word embeddings, we can leverage VADER-labeled data as training data. 

Now, I haven't played around with VADER yet so I can't speak much about it, but here's what I know. In short, Valence Aware Dictionary and sEntiment Reasoner (VADER) is a sentiment analysis tool that is lexicon & rule-based. You can see the lexicon [here](https://github.com/cjhutto/vaderSentiment/blob/master/vaderSentiment/vader_lexicon.txt). These are all annotated by humans (from [Amazon's Mechanical Turk](https://www.mturk.com/)). What's great about VADER is that we don't need any training data, since it's all lexicon & rule-based. In fact, we can use the result from VADER to train our own custom word embedding, which we will then merge with, say, GloVe's word embedding. This merged word embedding is then used as the initial feature layer for the neural network model that we're going to use for sentiment classification.

What's next? We still need a more academically rigorous approach---remember, this is a "hacker's approach". :) We probably need to use an optimization method instead of just calculating the relative displacement to project the new/OOV word into the vector space of the original word embedding.

## Keynote 4: Here Come The Robots - Python and Machine Learning by Tom Dyson
The keynote provides a great bird-eye view of what machine learning is capable of today & how you can create a machine learning application with a few lines of Python. It does not go too in-depth on the technical matters, but that's totally fine. Instead of complicated mathematical formulas, there were a bunch of practical demos. I had fun looking through each demo---I'm sure everyone had as much fun as I did, & I think the keynote definitely inspired a lot of people to get started with machine learning.

There was, for example, the website [This Person Does Not Exist](https://thispersondoesnotexist.com/) that generates a photo of a fictional person upon refresh. These photos are produced by generative adversarial network (GAN) & was released just a few days before the conference (to much fanfare!), so it was nice that it managed to get included in the keynote. However, although watching the computer generates photos of fictional people that look like real people is sooo fun, we also need to know how we can *identify* fake images. I like that the keynote also made a point about things we need to be aware about---the advancement of ML is not all sunshine & rainbows.

Tom Dyson also explained how his company managed to leverage machine learning for a UK & Ireland based charity called [Samaritans](https://www.samaritans.org/), which aims to provide emotional support to anyone battling distressing thoughts or at risk of suicide. There were also examples of practical applications such as entity extraction & outcome prediction.

A discussion on how machine learning is used today wouldn't be complete without also touching on this question: what's next? I'm glad to find that the answer to the question "what's next for machine learning" is not "more accuracy!", but:
- Reduced complexity (remember the Suzy Lee's keynote on the first day---more complex models are usually difficult to interpret!)
- Reduced cost (costs are still high most of the time if you want to get really good results)
- Generation, aside of comprehension (there was also a mention of OpenAI's infamous [GPT-2](https://blog.openai.com/better-language-models/))
- ML at the edge

[Slides](https://s3.amazonaws.com/tom/Here-Come-The-Robots-PyCon-APAC.pdf) & [demo](https://robots.now.sh)

## Talk: Using Artificial Intelligence and Satellite Imagery to Zero In on the Philippines’ Most Vulnerable Communities
Using AI to solve societal issues in Indonesia is---to this day---one of my lifetime goals, & so I was particularly excited for this talk. It did not disappoint!

Like Indonesia, the Philippines are also facing poverty. The government makes this data available---for example, we can see that Cebu is one of the richest provinces in the Philippines. However, when you zoom in, you will see that the province has pockets with a high level of poverty that we couldn't see when we see it from the higher level. Now this is an issue, because usually data regarding household income is conducted through manual household surveys is often reported on a regional/provincial level. Plus, they're super costly & infrequent (once in 3-5 years!). Thus, we need a faster & cheaper way to measure poverty in the Philippines that also gives more granular results.

OK, so how can we do this? Perhaps the obvious first answer is a straightforward supervised learning model. We can use satellite images as the input---wealthier areas have more structured roads, wider roofs, & tend to be less dense. However, there is an issue of data gap here---to use this approach, we would need a lot of labeled data, & although deep learning techniques are now advanced enough to learn from satellite images, there simply aren't a lot of training data for this case in the first place, making it harder for supervised models to solve this issue. 

How to overcome this issue? They adopted the methods from [a study by Jean et. al.](http://science.sciencemag.org/content/353/6301/790) from the Stanford University Sustainability and Artificial Intelligence Lab. The answer is: transfer learning. Long story short, transfer learning allows us to use whatever knowledge we have obtained from solving a problem to solve another problem (a different one, but still related!). In this case, they first started with a convolutional neural network (CNN) model that was trained on ImageNet. In this step, the model learns how to classify each image into one of 1000 different categories, & in the process they learn how to identify low-level image features (e.g. corners, edges, etc.). 

Next, they took this knowledge & use them to solve another task: predicting nighttime lights intensities based on the corresponding daytime satellite imagery. The idea here is that nighttime lights as a proxy for economic development, based on the idea that wealthier places tend to be brighter at night. The purpose of this task is to extract features that can be used to predict nighttime lights into one of three classes: low, medium, or high luminosity. Remember that since nighttime lights are proxy for economic development, these features can be used to predict economic development too.

OK, so now each daytime satellite image have a feature vector that represents patterns that the model used to differentiate between low, medium, and high light luminosity. Cool! But we're not done yet. We still need to estimate the poverty measures themselves, such as wealth index, & we do this by using mean cluster-level values that were obtained from the survey data & the image features from the previous step, to train ridge regression models that eventually predict the wealth index. This model, it turns out, is able to explain 63% of the variance.

This being a Python conference, there was also some discussion regarding the tech stack used. :) I've never played around with satellite images before. I think I've heard of some of my friends that majored in geography that they use QGIS. Turns out, there's a library called geopandas that you can use too (in fact, one of the speakers did encourage us to use geopandas over QGIS!).

However, there are a few challenges. The current model is not very interpretable & they're working on it. There is another issue with the fact that the ground truth data is very limited, & for the ground truth data they're depending on the government. Despite the challenges, there are lots of potentials for real-world impact here. Once we figure out how we can map poverty in a faster & more scalable way, we can use this for many awesome things: targeted humanitarian aid! Data-driven social policies! Data-driven programs! This project shows that there are a lot of opportunities to use AI for social good & I'm suuuuper excited for that. :)

[Related blog post #1](https://stories.thinkingmachin.es/philippines-most-vulnerable-communities/) & [related blog post #2](https://stories.thinkingmachin.es/using-transfer-learning-and-satellite-imagery-to-map-poverty-in-the-philippines)

# Overall
Now onto the conference itself! The topics were interesting, the atmosphere was great, & it was also well-organized. This year's PyCon APAC also has an awesome initiative called group lunches. During lunch time, each table will have a group facilitator & a topic assigned. The topics range from games to communities & so on. There was even a table for introverts! :-D If you're interested with, say, board games, you can sit at the Board Games table & have conversations with people who have the same interest as you. Without this initiative I probably would have had my lunch alone :) Throughout the conference I also met many awesome folks & had great & englightening conversations too.

So - what's my big takeaway from my experience at PyCon APAC 2019? It took me a few days of reflection but I guess now I know what it is, not only from the conference but also from my time in Manila (which I'll write about on a separate blog post!). Both Indonesia & the Philippines are similar in many ways & we're also facing identical issues. Someone I met at the conference summarized it nicely: "we're all on the same boat", & that's when it dawned upon me: joining forces & collaborations among, say, APAC countries or Southeast Asian countries makes so much sense. It's something I wish to see more of in the future, especially in the tech field.