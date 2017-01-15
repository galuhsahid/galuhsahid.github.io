---
layout: post
title:  "K-means Clustering"
date:   2016-06-23
categories:
- machine learning
---

## Unsupervised vs. Supervised Learning
Before getting into clustering, I guess it makes sense to first discuss unsupervised &amp; supervised learning. In unsupervised learning, we're trying to find how the data are organized. We're trying to find the pattern. We're only given unlabeled examples. Whereas in supervised learning, we have training data that the machine can learn from. Clustering itself is a form of unsupervised learning.

## Clustering
Onto the next question: what is clustering? In clustering, we assign a set of observations into subsets (which we will continue to call clusters). The observations in the same cluster should be similar (according to some criteria) to each other than those in other groups.

## K-means Clustering
What k-means clustering aims to do is to cluster n observations into k clusters. How do we determine which observation belongs to which mean? In k-means clustering, each observation belongs to the cluster with the nearest mean. The resulting data space is going to consist of [Voronoi cells][voronoi].

Something that we need to know is that computing k-means clustering is computationally difficult; [it is an NP-hard problem even in d = 2 dimensions][np-hard], with NP standing for non-deterministic polynomial time. [Here][complexity] is a *really* nice answer that explains the definitions of &amp; differences between P, NP, NP-complete, and NP-hard (although the answer doesn't use the original definition of *non-deterministic* for NP).

### K-means Algorithm
The k-means algorithm input consists of: a dataset X of n points and a parameter k (<= n) specifying the number of clusters. The output is a set of k cluster centroids and a labeling of X that assigns each of the points in X to one of the k clusters.

Since each observation belongs to the cluster with the nearest mean, it means that all points within a cluster are closer in distance to their centroid than they are to any other centroid. In mathematical terms, we're trying to find:

$$minimize \sum_{k=1}^k \sum_{x_n \setminus C_k} ||(x_n - \mu_k)||^2$$

with respect to $$C_k, \mu_k.$$

### Lloyd's Algorithm
The problem is NP-hard, thus there exists a variety of heuristic algorithms that we can use. The most common is the standard algorithm (or Lloyd's algorithm). Lloyd's algorithm **guarantees convergence, though to a local minimum**. Lloyd's algorithm consists of the following operations:

- Place centroids $$c_1 .. c_k$$ at random locations
- Repeat until convergence:
  - Assignment step: for each point $$x_i:
    - find the nearest centroid $$c_j$$ &ndash; *for every x in our dataset, find the nearest centroid to that x. We can do this by calculating the most minimum Euclidean distance.*
    - assign the point $$x_i$$ to the cluster j &ndash; *so this x belongs to the cluster which centroid is closest to that x.*

    $$minimize D(x_i, C_j)$$

  - Update step: for each cluster $$j = 1 .. k$$
    - new centroid $$c_j$$ = mean of all points $$x_i$$ assigned to cluster j in previous step &ndash; *basically, we're going to assign a new centroid for each cluster. This new centroid is the average of all points belonging to that cluster.*

    $$\mu_k = \frac{1}{C_k}\sum_{x_n C_k}X_n$$

- Stop when none of the cluster assignments change

An interesting note here is that obviously, k-means clustering can only be applied for **numerical data**, since at some point of the algorithm, we will have to find the average of our points (see step 2).

#### Wait, does it have to be Euclidean distance? Why/why not?
I asked myself this and [some interesting answers][euclidean] turned up. Paraphrasing one of the answers, k-means does **not** explicitly use distances between data points. Another way to say this is that k-means isn't constructed based on distances at the first place. What k-means does is that it minimizes within-cluster variance (and maximizes between-cluster variance), which is identical to the sum of squared Euclidean distances from the center.

Now onto the question, can we use arbitrary [distances][distances]? The answer is no, because **k-means may stop converging with other distance functions**. How is that so? Both the assignment step and the update step optimize the same criterion. There is a finite number of assignments, and therefore the algorithm should converge after a finite number of improvements. When we're using other distance functions, we must guarantee that the mean *also* minimizes the distance. Otherwise, k-means may stop converging.

#### Complexity
The complexity for Lloyd's algorithm is $$O(nkdi)$$, with n for the number of iterations, k for the number of clusters, d for the number of dimensions, & i for the number of instances.

#### Python implementation
*Coming soon!*

#### Is that it?
Well, naturally, two questions come up in my mind:

1. **How do we initialize the proper *k*?** Lloyd's algorithm unfortunately doesn't tell us how to initialize the proper *k*. Obviously, the outcome of the Lloyd's algorithm heavily depends on the *k* that we choose. When we've run the algorithm and obtained our result, it's possible that there is a more optimal solution had we chosen another *k*.

2. **How do we initialize the proper centroids?** The argument is similar to point 1.

### Initializing the proper *k*

### Initializing the proper centroids

### Drawbacks, or when not to use k-means clustering
When learning algorithms, it is always best to know when *not* to use them.

[voronoi]: http://google.com
[np-hard]: http://cseweb.ucsd.edu/~avattani/papers/kmeans_hardness.pdf
[complexity]: http://cs.stackexchange.com/questions/9556/what-is-the-definition-of-p-np-np-complete-and-np-hard
[euclidean]: http://stats.stackexchange.com/questions/81481/why-does-k-means-clustering-algorithm-use-only-euclidean-distance-metric
[distances]: http://galuhsahid.com/similarity-scores
