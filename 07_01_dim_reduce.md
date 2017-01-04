# Morning Lecture

__PCA__

## Dimensionality

Number of features in X matrix

How many features in an image? __The number of pixels in image! x colour channels (i.e. 1 for greyscale, 3 for RBG, 4 for CMYK)__

Turn every image into a row that will go into our giant matrix that will have size (n = number of images, p = number of dimensions). The image becomes a row by appending each row of pixels to form one huge long row.

This was a driving force for using dimensionality reduction. This is a lot of features for computer. Also, most of these pixels are useless.

### Why?

 - curse of dimensionality

 - visualization: hard to visualize anything with more than 2D

 - latent features: when there is a lot of correlation between features you can create meta features that contain more data

 - remove correlation: create new version of matrix that has no covariance

### How?

 - select manually forward-wise

 - use LASSO and then use (relaxed lasso) only the features that have non-zero β at the λ

 - upper layer features in neural networks: matrix multiplications at every layer and eventually you get to a point that has lower dimensionality than what you started with. Run image through to classification step and then back off and say hey whatever my features are here I'll use these. Network will learn for you what important features are.

 - autoencoders in certain circumstances (get rid of non-linearities in neural network) will actually give you same result as PCA. Bottle neck is secret sauce

 - PCA. Easy to abuse. Doesn't require labelled data and mathematically rigorous. Neural networks are mathematically hazy. There is no reason it should work - it just seems like it does.

### Basis of matrix

Any member of that space can be linear combination of basis elements. If we have an n-dimensional space we should have n spanning elements. In this space, any vector should be able to be represented as sum of coefficients and unit vector that is a member of spanning basis. These spanning vectors must also be linearly dependent. Basis can be orthogonal but doesn't have to be.

We often represent basis as a matrix itself and it is useful that the vectors are orthogonal. This basically means that the basis for any space is just the identity matrix of that size.

If the matrix is also ortho-normal (i.e. orthogonal and all vectors have unit length) then V<sup>T</sup> = V<sup>-1</sup> and so VV<sup>T</sup> = V<sup>T</sup>V = I

## How many principal components to keep?

Look at graph of eigenvalue magnitude vs. number of dimensions. Look for the elbow like we did with k-means. Can also pick percentage of variance we want to explain.

# Morning Breakout

Use cases of dimensionality reduction:
 - visualization
 - remove curse
 - remove correlation
 - remove correlation

Why do we do dimensionality reduction for supervised learning? What models are important?
 - we are assuming that everything we reduce is related to the dependent variable
 - __Danger: the things with highest variation might not actually have any meaning in relation to the dependent variable!!__
 - could potentially reduce our runtime
 - important for KNN

Why do we do dimensionality reduction for unsupervised learning? What models are important?
 - curse of dimensionality for clustering
 - clustering (e.g. hierarchical) takes forever and runtime can be lowered if we do dim reduction

 How do we use dimensionality reduction for data visualization?
  - reduce down to max 3 features and then humans can understand

 What is a transformation matrix?
  - square matrix that when multiplied to column vector will change it: stretch/shrink, rotate, shear (which we'll ignore for now)
  - let's just assume for now that all we are doing is rotating and stretching

 What are axes of new plot?
  - take identity matrix and transform and then you get new basis

# Afternoon Lecture

When you do PCA the global distances will be preserved (proportionally). However, we usually care more about local distances and so in this case we use TSNE. It preserves local closeness. The ones that are close after TSNE were locally close but ones that look far apart in TSNE are not actually that far apart.

Has good application for visualization but not that useful for actual modelling because you cannot go backwards.

## When you can use PCA
 - visualization
 - kNN or clustering
 - image stuff

## Singular Value Decomposition

Without proof, just accept that any matrix M = UΣV<sup>T</sup>

 - U: unitary, orthogonal (n, n)

 - Σ: diagonal, real, positive (n, p)

 - V<sup>T</sup>: unitary (p, p)

If rank is less than matrix size then you are guaranteed to get 0 for eigenvalue. 0 as eigenvalue necessarily means that there is degeneracy in the matrix.

In dimensionality reduction sense, we are going to pretend some eigenvalues are 0 if they are below a certain threshold (to be determined as previously with % explained variance, etc.)

Ultimately SVD is not very interpretable but it is mathematically rigorous. Later on we will move onto things that are more mathematically hazy and SVD will be useful then. SVD is really nice because it is perfectly invertible.

### Latent feature extraction

Given utility matrix where we have n rows (number of users) and p columns (number of movies) we can do decomposition to find a use profile matrix (n rows and d columns) and a item profile matrix (d rows and p columns). We will determine d with % explained variance (or something like that).

Each d column/row represents the value for newly extracted meta feature, e.g. science fiction-y-ness, romance-y-ness, etc. For item profiles it corresponds to how 'latent-feature-y' that movie is (e.g. 'action-y'). For users it represents how much that one user likes 'action-y' movies for instance. In this case, we don't even need to find similar users, we can just throw action movies at that user.

Matrix and Star Wars in our example have the same value because our users rated them the exact same.

These _latent features_ have been discovered by doing mathematical transformation of decomposition.

It is our job to figure out what exactly that latent feature actually represents and means. Recall clustering day. We tried doing this with a variety of techniques: look at random sample of articles from that cluster, look at top words (by frequency, etc.)

Can also apply to things you discover from images with neural networks.

### Cold start

 - Can just force ask new user to answer a bunch of questions

 - Just recommend overall most popular ones in general

### Missing ratings

Could use -1 instead of 0 to fill in missing ratings as a way to encode 'invalid rating'.

Could do approximate SVD on sparse data. (Alternating least squares)

# Afternoon Breakout

Our new _covariance_ matrix after doing PCA is:

8  0  0
0  3  0
0  0  1

What does each value in diagonal mean?
 - Variance

Why are the non-diagonal values all 0?
 - Because after PCA now all the covariances between features is 0

Why is this desirable?
 - It will be useful to us to have uncorrelated features

If you're building a model with only one feature, which do you expect to have more predictive power?
 - 1st PC or one original feature? It depends!

Case where 1st PC is better?
 - images
 - each individual pixel does not have that much predictive power but using PC allows us to use some effect of all the pixels
 - if variance in y is also related to variance in x then that is good

Case where an original feature is better?
 - feature that has high variance but is does not have good predictive power
 - PC will capture that variance but it will not help our prediction (e.g. zip code?)

__Principal component regression__ using PCA with regression. Hit or miss. Not used very often in general.

__SVD vs PCA__

Σ vs λ from PCA? Σ is just squared.
