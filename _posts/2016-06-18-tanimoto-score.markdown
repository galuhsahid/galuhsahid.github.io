---
layout: post
title:  "Tanimoto Score"
date:   2016-06-18
categories: 
- machine learning
---

Today I stumbled upon the term **Tanimoto score**. It's quite surprising that the search turned up only [2680 results][res] so I had to do more digging than the usual. The search led me to the terms Tanimoto similarity and distance, which reminded me of the [Jaccard similarity and distance][jaccard].

Surprisingly, Tanimoto similarity and distance are sometimes used interchangibly with Jaccard similarity and distance. Some uses are referring to the same formula, but others are actually mathematically different. The similarity ratio given by Tanimoto similarity is equivalent to Jaccard similarity, but the distance function isn't the same as Jaccard distance.

First appeared in the paper "A Computer Program for Classifying Plants" which was published in 1950, the similarity ratio is given over bitmaps. In the paper, each bit of a fixed-size array represents the presence or absence of a characteristic in the plant being modelled. 

## Tanimoto similarity

Tanimoto similarity is given by the number of common bits divided by the number of nonzero bits in either sample.

If samples X and Y are bitmaps, $$X_i$$ is the i-th bit of X, $$\land$$ is bitwise and, and $$\lor$$ is bitwise or operator, then the similarity ratio is:

$$T_s(X,Y) = \frac{\sum_{i}(X_i \land Y_i)}{\sum_{i}(X_i \lor Y_i)}$$

If we model X and Y as sets of attributes instead of bits, we can see that it will give the same result as the Jaccard similarity:

$$J(X,Y) = {\frac {X \cap Y}{X \cup Y}} = {\frac{X \cup Y}{|X| + |Y| - |X \cap Y|}}$$

## Tanimoto distance

Tanimoto then defines a distance coefficient based on the ratio above:

$$T_d(X,Y) = {-log_2(T_s(X,Y))}$$

However, the Tanimoto distance is not a proper distance metric since it violates the triangle inequality. This definition of Tanimoto distance allows the possibility of two specimens, which are quite different from each other, to both be similar to a third.

## Uses
It appears that the Tanimoto score is commonly used in cheminformatics, such as [finding similarities between fingerprints][fingerprints] and [matching protein-ligand binding sites][protein]. 

[res]: https://www.google.com/search?q=%22tanimoto+score%22&ie=utf-8&oe=utf-8&client=firefox-b-ab
[jaccard]: http://x.galuhsahid.com/similarity-scores
[fingerprints]: http://www.ncbi.nlm.nih.gov/pmc/articles/PMC4456712/
[protein]: https://www1.maths.leeds.ac.uk/statistics/pgstats/theses/davies.pdf