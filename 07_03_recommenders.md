# Morning Lecture

## Classification/Regression

 - classifier to predict interaction/rating using past interactions/ratings as training data

  - using features of people I've liked so far on ? as training data, predict/rank who I'll like

  - using features of people who have bought product X, predict who else is likely to buy it

## Collaborative Filtering

Find similar users and recommend what those users like.

Privacy issues?

# Morning Breakout

Implicit or explicity?
 - Liking a song on Pandora = E
 - Listening to a song on Pandora = I
 - Sharing a song on Pandora with a friend = E/I
 - Giving a rating on Netflix = E
 - Watching a movie on Netflix = I
 - Viewing an item on Amazon = I
 - Purchasing an item on Amazon = E/I
 - Adding an item on Amazon to your wish list = E/I

# Afternoon Lecture

## Similarity/Content-based

Find stuff that is similar (content-based)

## Matrix Factorization

__We have a problem. PCA, SVD, NMF all must be computed on a dense data matrix (X)__

Potential solution: impute missing values naively with something like the mean of known values. _This is what sklearn does it says it factorizes sparse matrices_

So do we actually have a way to do this? Factorize a sparse matrix.

This is a model! The reconstructed utility matrix is equivalent to our prediction.

Recommender systems tend to blur the line between supervised and unsupervised learning.

### Funk SVD

Overfitting very likely possibility with sparse matrix.

## Additional considerations

 - user state
  * basket analysis

 - user preference shift
  * build in decay to have ratings from years ago not apply so much any more

 - overall succsess evaluation
  * ideally A/B test to find out which is better because cross validation is hard

# Afternoon Breakout

1. Classification/regression
 - __data__: use past interactions and features of people and items as training data
 - classify will like or will not like
 - _individual models_: build one model for each user
  * gets better over time
 - _one model_: could also build model that uses data about user and item
  * does not really get better over time (e.g. use gender, age location, genre, bpm, instruments to predict song rating)
  * get more data and model will change but won't really get better
2. Collaborative filtering
 - __data__: interaction data
 - recommend things that similar (based on whatever metric you use) users also like
 - useless at first
 - pretty good value for seasoned user
 - in general, if you have
3. Similarity
 - __data__: item data
 - recommend similar items to ones user has liked in past
 - pretty good at first
 - not get better over time; not good diversity and also not incorporating any of the new interaction data

__1 versus 3: 1 is supervised and 3 is unsupervised. 3 uses only item data.__

How would you compare the matrix factorization recommender to the matrix factorization techniques we saw earlier (SVD, PCA, NMF)?

 - sparse vs dense matrix
 - cost function for recommender matrix factorization is only across cells with data but for NMF is for all cells - including imputed ones

Why do we have to be careful about overfitting matrix factorization recommender?

 - lots of features (i.e. things we can tweak are two full matrices!) and not so many data points
 - few data points, so many features we have the _ability_ to _reproduce_ data but this does not necessarily _represent_ data

In what ways can we regularize our factorization?

 - limit the topics (k)
