# Morning Lecture

## Ensembles

We combine predictors. Could be the same or could be different.

Classifiers: take majority vote

Regression: Average

## Bootstrap AGGgregatING (BAGGING)

1. Draw random bootstrap sample of size n (where some points will be repeated and some will not be)

2. Grow decision tree from the bootstrapped sample. At every node:

 * Split using the best feature that provides best split based on information gain (or other metric)


3. Repeat 1-2 k times

4. When it comes time to predict, get prediction from each of the k tree and:

 * Assign class label by majority vote for classification
 * Average result for regression


Basically averaging away the high variance of decision trees.

Pruning is another way to reduce variance in decision trees.

__You can probably do both__

__But what if all the bagged trees are the same?__ If we are just averaging trees that are the same we are not learning anything.

So we want a way to de-correlate our trees.

## Random Forest

1. Draw random bootstrap sample of size n

2. Grow decision tree from sample. At every node:
 * Random select d features without replacement
 * Split using best feature - out of the selected d - that provides best split based on information gain (or other metric)


3. Repeat 1-2 k times

4. When it comes time to predict, get prediction from each of the k tree and:
 * Assign class label by majority vote for classification
 * Average result for regression

# Afternoon Lecture

## Out-of-bag error

The replaces validation. __Not testing__

Each data point appears in only about 2/3 of the training sets.

1. Draw bootstrapped sample

2. Grow tree

3. __OOB__: This tree has an out-of-bag set of data that was not selected by bootstrapping. Use just built tree to predict for these observations.

4. Repeat steps 1-3 k times

5. Aggregate predictions

### Caveats

If we're going to impute a mean to replace missing values we can't use any of the out-of-bag stuff. You should impute that again at every level using __only__ training data.

## How many trees?

More is better but not really worth it if you increase prediction time a lot.

## Feature importance

The following is Python's implementation:

1. Start with an array of 0s that is the same size as the number of features in your model

2. Traverse tree with data that the tree was built with

3. Calculate gain at every node

4. Count how many points are at the node

5. Sum the product of values from 3 and 4 for each feature that nodes split on

6. Normalize all values

7. Repeat 1-5 for all the other trees and then average the results for each feature to get importance.


The following is R's implementation:

1. Calculate base OOB error

2. For every feature, determine the range of values (i.e. max and min)

3. One feature at a time, randomly permute the values of that feature between min and max values

4. After permuting one feature, calculate new OOB

5. Calculate difference between regular OOB and permuted OOB. If the permuted OOB wasn't that much worse then we know that feature wasn't that important

The safe thing to do is to look at the _rank_ of feature importance but not the _magnitudes_. Typically, the more features you have - the less important any one feature will be.

Importance is not directional. In the absence of truly important feature, other ones can serve as proxies for those missing features and appear to be very important (when they aren't).

## Tuning

 - Number of trees:
  * More is better but when you add more your runtime increases


 - Max features:
  * Classification: sqrt(p)
  * Regression: p/3 or p


 - Tree depth/min leaf sample (or min sample split equivalently)
  * Can do pre-pruning on our trees
  * Start with None
  * Typically not limited in random forest but in boosting we will


 - Jobs:
  * -1 will make it run on maximum # of processors

## Feature engineering

Unlike in linear regression - _do not drop one of the categories_ - if you dummy variables because we need every single option available for splitting. We were worried about perfect multicollinearity with linear regression that isn't applicable here.

Random forest doesn't understand strings. Could also turn into continuous variable feature rather than dummifying.
