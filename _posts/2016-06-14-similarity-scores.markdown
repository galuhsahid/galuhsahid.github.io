---
layout: post
title:  "Similarity Scores"
date:   2016-06-14 19:12:17 +0700
categories: 
- machine learning
---

I'm reading the book [Programming Collective Intelligence][ci] to get started with machine learning and one of the first topics that the book talked about is **similarity scores**. The book only explained two systems: Euclidean distance and Pearson correlation. Both of them are systems that I've learned in class, so I set out to find other systems on my own. 

But anyway, it's probably best to discuss what similarity scores are.

Basically, similarity measures how similar (or different) two data objects are. The scores, of course, don't come out of nowhere--instead, we get the scores from the information known about them. The attempt to recognize similar individuals based on their characteristics is known as similarity matching, which will then generate similarity scores. Today, we can see similarity matching everywhere, from product recommendations to targeted advertisements.

## Measures of similarity scores

### Euclidean distance
Euclidean distance is probably the most simple system that we can use to determine similarity. In fact, it is the basis of many measures of similarity. The Euclidean distance between two points is given by the Pythagorean theorem (which is why it probably looks familiar). Euclidean distance is the square root of the sum of squared differences between corresponding elements of the two vectors.

$$D(x,y) = \sum_{i=1}^n (x_i - y_i)^2$$

Euclidean distance is most often used to compare cases, such as those arranged as a [respondent-by-variable matrix][matrix], where the columns represent the variables while the rows represent the respondents. When looking for similarities, we're comparing the rows (the respondents) instead of the columns (the variables). We don't need to adjust the differences of scale (which we'll talk about in more detail later) because the rows aren't scales--they're not even variables. Therefore, Euclidean distance is most suitable to compare cases.

Euclidean distance in Python:

{% highlight python %}

from math import *

def euclid_distance(x,y):
	return sqrt(sum(pow(x-y,2) for a, b in zip(x, y)))

{% endhighlight %}

### Pearson correlation coefficient
The **correlation coefficient** determines the degree to which two variables' are associated. Another way to look at correlation coefficient is as a measure of how well two sets of data fit on a straight line. The range of values is -1 (indicating a perfect negative correlation) to 1 (indicating a perfect positive correlation). Values between that range indicates that there's a variation around the best-fit line. A value of 0 indicates that there is no correlation between the two variables measured.

> It should be noted that correlation coefficient only measures the linear relationship between two variables; non-linear relationships between two variables **cannot** be measured by the correlation coefficient.

Pearson correlation coefficient in Python:

{% highlight python %}

from math import *

def avg(x):
	if len(x) > 0
		return float(sum(x)) / len(x)

def pearson(x,y):
	if len(x) == len(y)
		n = len(x)
		if n > 0
			avg_x = avg(x)
			avg_y = avg(y)
			diff_product = 0
			xdiff2 = 0
			ydiff2 = 0
			for idx in range(n):
				xdiff = x[idx] - avg_x
				ydiff = y[idx] - avg_y
				diff_product += xdiff*ydiff
				xdiff2 += pow(xdiff2, 2)
				ydiff2 += pow(ydiff2, 2)

				return diff_product / math.sqrt(xdiff2*ydiff2)

{% endhighlight %}

### Manhattan distance
In Manhattan distance, the distance between two points is the sum of the absolute differences of their Cartesian coordinates. 

$$D(x,y) = {\sum_{i=1}^n |x_i-y_i|}$$

Manhattan distance in Python:

{% highlight python %}

from math import *

def manhattan_distance(x,y):
	return sum(abs(a-b) for a, b in zip(x, y))

{% endhighlight %}

### Minkowski distance
Minkowski distance is a generalized metric distance. The Minkowski distance is defined for any p >= 1, but is commonly defined for p = 0 (Manhattan distance), p = 1 (Euclidean distance), and p = $$\infty$$ (Chebyshev distance).

$$D(x,y) = {\sqrt[p]{\sum_{i=1}^n |x_i-y_i|^p}}$$

I stumbled upon an interesting answer on Quora that answered ["what is the difference between Manhattan and Euclidean distance measures?"][quora]. The difference depends on our data, it says, but generally Manhattan works better than Euclidean for high dimensional vectors. Consider Chebyshev distance (with p = $$\infty$$) which equation(s) for p = $$\infty$$ and p = -$$\infty$$ respectively are:

$$D(x,y) = \lim_{x\to \infty}{\sqrt[p]{\sum_{i=1}^n |x_i-y_i|^p}} = {\max_{i=1}^n |x_i-y_i|}$$

$$D(x,y) = \lim_{x\to -\infty}{\sqrt[p]{\sum_{i=1}^n |x_i-y_i|^p}} = {\min_{i=1}^n |x_i-y_i|}$$

As we can see, the distance becomes the highest difference between any two dimensions of the vectors. This shows that we would be ignoring the rest of the dimensions that we have, only taking into account the dimension that generates the highest difference between the two vectors. Reducing the exponent (or the p value) will further involve other features in the calculation.

Now we may wonder, will Minkowski distance work even better with p < 1? According to the answer, it's true. However, p < 1 violates the triangle inequality, and therefore is not a metric.

> **Triangle inequality**: the sum of the lengths of any two sides of a triangle is greater than the length of the remaining side.

Minkowski distance in Python:
{% highlight python %}
from math import*
from decimal import Decimal

def nth_root(val, n_root):
	root_value = 1/float(n_root)
	return round (Decimal(value) ** Decimal(root_value), 3)
def minkowski_distance(x, y, p_val):
	return nth_root(sum(pow(abs(a-b), p_val) for a, b in zip(x, y)), p_val)

{% endhighlight %}

### Cosine similarity
Cosine similarity is quite interesting and took me longer to grasp, but hey.

We're basically calculating the cosine of the angle between two objects. The value is ranging from 0 to 1, with the value of 0 indicating that the two vectors are at 90, while the value of 1 indicating that the two vectors are at 0. Two vectors that are diametrically opposed have a similarity of -1. It should be palpable that cosine similarity measures orientation and not magnitude.

Now we're going into a more technical detail (can't help it--I've always loved linear algebra anyway). Basically I'm just going to recite what I learned from [this post][cosine], but reciting (or writing down) helps me remember so I'm going to write it down anyway.

Cosine similarity is a variant on the inner product. The basic underlying principle of similarity then comes down to the inner product:

$$Inner(x, y) = {\sum_{i=1}x_i y_i} = {\langle x, y\rangle}$$

**Refresher:** Inner product is a generalization of dot product. In a vector space, it is a way to multiple vectors together, resulting in a scalar. There are examples of inner product spaces:

1. The real numbers $${R}$$, with the inner product given by

$${\langle x, y\rangle} = {x y}$$

2. The Euclidean space $${R^n}$$, with the inner product given by

$${\langle (x_1, x_2, ..., x_n), (y_1, y_2, ..., y_n) \rangle}$$
$${= x_1 y_1 + x_2 y_2 + ... x_n y_n}$$

The idea is that if x tends to be high where y is high, and low where y is low, then the inner product will be similar. In other words, the vectors are more similar.

The inner product is unbounded, and a way to make it bounded so that it will give a value between -1 and 1 is to divide by the vectors $$l^2$$ norms, which will then give the cosine similarity:

$$CosSim(x, y) =  {\frac{\sum_{i=1}x_i y_i}{\sqrt{\sum_{i=1}x_i^2}{\sqrt{\sum_{i=1}y_i^2}}}}$$

It should be noted that the cosine similarity is invariant to shifts.

Cosine similarity in Python, given two lists:
{% highlight python %}
from math import *

# Basic function, very Python but very slow
# Zip is evil -> O(nm)

def cos_sim(x,y):
	sum_xy = sum(a*b for a, b in zip (x, y))
	sum_x2 = sum(pow(a,2) for a in x)
	sum_y2 = sum(pow(b,2) for b in y)

	return sum_xy/float(pow(sum_x2,0.5)*pow(sum_y2,0.5))

# Faster, more imperative

def cos_sim_2(x,y):
	sum_xy, sum_x2, sum_y2 = 0, 0, 0
	for i in range(len(x)):
		a = x[i]
		b = y[i]
		sum_xy += a*b
		sum_x2 += a*a
		sum_y2 += b*b
	return sum_xy/float(pow(sum_x2,0.5)*pow(sum_y2,0.5))

{% endhighlight %}

#### Cosine similarity in action
Say that we have two sentences that we would like to compare based on the word count (and ignoring word order):

1. Budi loves me more than Ari loves me
2. Bejo likes me more than Budi loves me

Count the number of times each of these words appears in each text:

- me  2  2
- Bejo 0 1
- Budi 1 1
- Ari 1 0
- likes 0 1
- loves 2 1
- more 1 1
- than 1 1

We're not interested with the words; we're only interested with the counts, which will be our basis of calculation. The two vectors will be:

a: [2, 1, 0, 2, 0, 1, 1, 1]

b: [2, 1, 1, 1, 1, 0, 1, 1]

The cosine similarity is 0.822.

#### Cosine similarity vs. Euclidean distance
Naturally, a question comes up: when should we use cosine similarity instead of Euclidean distance? Remember that there are two components of a vector: magnitude and direction. As said before, cosine similarity measures orientation and not magnitude, whereas Euclidean distance measures is perceptible to magnitude. We can think of orientation (or direction) in vector as its "sentiment". The magnitude, on the other hand, is how strong it is towards that direction.

So when we're classifying documents by their overall sentiment, we should use cosine similarity.

In Euclidean distance, vectors with different directions would be clustered because their distances from origin are similar. By the end of the day, it all comes down to our data and what we want to obtain from our data (well, duh).

An example would be this: cosine similarity would tell us that a document containing the word "Python" 5 times and "Flask" 8 times and a document containing the word "Python" 500 times and "Flask" 800 times are heading to the same direction. This wouldn't be the case when we're using Euclidean distance (in which we're taking magnitudes into account). In short, both Cosine similarity and Euclidean distance can be used to determine similarity, although they measure different aspects of similarity.

Another way to see cosine similarity is as measuring the relative proportions of the various features or dimensions--when all the dimensions between to vectors are are in proportion (correlated), they get maximum similarity. Meanwhile, Euclidean distance, Manhattan distance, etc. are more concerned with absolutes.

#### Why does the cosine similarity work?
In terms of two dimensions, let's say we have Document A. If Document A contains the word "Python" 6 times, and the word "Java" 4 times, then the point (6,4) would present this document. Speaking in terms of vectors, Document A would be the vector that goes from the origin to the point (6,4).

Then comes Document B. We can say that Document A and Document B are similar when they have the same number of references to the word "Python" and "Java", or the same ratio of references (maybe Document B could reference "Python" 12 times and "Java" 8 times because it is a longer text, but still the proportion is just the same). Less similar documents may also contain references to the same words but with a different proportion.

If we draw these vectors for Document A and Document B, we can see that when they're similar, they will overlap (although it's possible that one will be longer than the other). When they have less in common, the vectors begin to diverse; they will have a bigger angle. Thus, by measuring the angle between the vectors, we can have an idea of their similarity. 

If Document A only mentions "Python" and Document B only mentions "Java", then they have nothing in common. Document A will have its vector on the x-axis, while Document B will have its vector on the y-axis. The angle, then, will be 90 degrees, which cosine will result in 0.

This idea can easily be conceptualized for higher dimensions (let's say we have another word to compare with, such as "Ruby") although it is hard to draw.

### Jaccard index
Jaccard index is used for comparing the similarity and diversity of sample sets. The Jaccard coefficient measures similarity between finite sample sets, and is defined as follows:

$$J(A,B) = {\frac {A \cap B}{A \cup B}} = {\frac{A \cup B}{|A| + |B| - |A \cap B|}}$$

With $$0 \le J(A, B) \le 1 $$.

We're basically dividing all the features that are common with the number of properties.

If both sets are empty, we define J(A, B) = 1.

We also have Jaccard distance, which measures the dissimilarity of sample sets. It is simply the complement of Jaccard coefficient and is defined as follows:

$$d_J (A,B) = 1 - J (A,B) = {\frac {|A \cup B| - |A \cap B|}{|A \cup B|}}$$

{% highlight python %}
from math import *

def jaccard_sim(x,y):
	intersection_car = len(set.intersection(*[set(x), set(y)]))
	union_car = len(set.union(*[set(x), set(y)]))
	return intersection_car/float(union_car)

{% endhighlight %}

## When to use which one?

### Numeric and plottable
Our dataset is numeric when it is comprised of only number fields and values. When our dataset is numeric and can be portrayed in n-dimensional data, we can use various geometric metrics such as Euclidean, Manhattan, or Minkowski distance metrics. 

### Numeric and non-plottable
If our data is numeric but non-plottable (such as curves instead of points), we can obtain similarity scores based on **differences between data**, instead of the actual values of the data itself.

### Non-numeric
We can use metrics like the Jaccard distance metric or cosine similarity.

Note: I haven't really taken the time to re-review all the codes, so apologies if mistakes are found! Kindly let me know if you find any. :)

[ci]: https://www.amazon.com/Programming-Collective-Intelligence-Building-Applications/dp/0596529325
[matrix]: http://www.analytictech.com/networks/kindsofmatrices.htm
[quora]: https://www.quora.com/What-is-the-difference-between-Manhattan-and-Euclidean-distance-measures#
[cosine]: https://brenocon.com/blog/2012/03/cosine-similarity-pearson-correlation-and-ols-coefficients/
[github]: http://github.com/galuhsahid/machine-learning