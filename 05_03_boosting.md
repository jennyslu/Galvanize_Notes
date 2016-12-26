# Morning Lecture

Decision trees split data.

Number of trees is a measure of complexity for boosting but not for random forests.

We are going to make our trees small with boosting. It is common to have a stump with just a single split (i.e. depth 1). We don't have to do this if we have interacting features. E.g. if you have only depth 1 you can only split on latitude or longitude but not both and thus never localize a neighborhood. If you had depth = 2, you could split on both latitude and longitude.

## AdaBoost

Classification

The ones that are misclassified have their weights increased.

## GradientBoost

Regression

First tree fits to real data

Remaining trees fit to residuals from previous

Final prediction is sum of all the individual trees

GBRT d=1 means adding together many tree stumps (of depth 1).

As learning rate goes down the number of trees needs to go up! Decreasing learning rate does give you better results but you need more trees to achieve that result.

# Stochastic Gradient Boosting

Random subsample features just like in random forests.

Can also subsample the training set. Why sub sampling instead of bootstrapping? Empirically seems to perform better.

__Boosting generally better than random forests but it takes longer to get there because there are many hyperparameters to tune__

# Afternoon Lecture

AdaBoost for classification.
