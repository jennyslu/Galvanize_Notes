# Morning Lecture

How do we pick a model? Money $$$ Pick the model that makes us the most money or loses the least money.

We want to figure our profit matrix from confusion matrix.

For example: churn. We're predicting churn (positive). So there is a profit to be gained from correctly predicting churn because we can win them back (_true positive_). There is a loss for predicting someone will churn when they weren't going to because you waste money on trying to win them back (_false positive_). There is also a loss for predicting someone isn't going to churn when they are going to (_false negative_). Finally, there is no real profit from correctly predicting that they won't churn - but at least we don't waste money (_true negative_).

What is bigger? Benefit from being right or loss from being wrong?

Credit card fraud?
 - TN: small positive
 - FP: small negative
 - TP: big positive - but not really we just don't lose money
 - FN: big negative (lose money, but it's relative)

Let's always assume we get it right. We aren't actually making any money, we're just not losing money. What's important is the relative value of B<sub>P+</sub> (benefit from predicting true positive) versus C<sub>P-</sub> (cost from predicting false positive).

## Profit Curve

1. Predict everything negative

2. Move threshold down one point (i.e. top probability of being positive is positive)

3. Multiply together confusion matrix and cost benefit matrix and get value

4. Plot this value on

5. Repeat 2-4

__This curve is highly dependent on what we make our cost benefit matrix!__

## Imbalanced Data

Examples of imbalanced data? Fraud, medical stuff, crimes, click-through-rate.

Most classifiers just focus on accuracy. But with imbalanced data we have to tell it to not worry so much about accuracy because this isn't a good metric with imbalanced data.

How do we choose the threshold?
 - Could just pick the point on ROC closest to upper left corner.
 - Decide specificity you want
 - Decide sensitivity you want

Or get a new ROC curve. How? Resample!
 - Under sampling: remove points from large class until they are about the same. Randomly discard majority class observations. (There are currently about 17 methods of accepted under sampling methods.)
  * +: Makes calculations way faster

  * -: Throws away data. This is probably ok if we have a ton of data overall


 - Over sampling: draw points from small class more times than there are actual data. (There are currently about 9 methods of over sampling.)
  * +: Did not throw away any data

  * -: Could turn one outlier into 10 outliers in the same spot and your model will follow that mistake. Could force decision boundary to be really close to the point as you oversample.


 - SMOTE: made up new data for small class. Found to work well in practice.

    1. First decide how many neighbors I would like to investigate (5 is typical, also 3)

    2. Choose random point

    3. Select k nearest neighbors

    4. Pick a random number between 1 and k. SMOTE with that neighbor. Only SMOTE minority class.

    5. For each feature:

        5. Pick a random number between 0 and 1 (gap).

        6. New point's value for that feature that is gap% of the value of current feature for selected point + (100-gap)% of value of current feature for selected neighboring point

    7. Repeat 2 - 5

    This will change our decision boundary by creating data that we may have seen. This is the theory. Tends to generalize better for over sampling for small datasets.

 - Tomek Links: there are some points that make building a convenient decision boundary difficult. There are the points with little distance between majority and minority classes. We just erase all those points (from both classes). This will make a smooth decision boundary that tends to generalize well. Although SMOTE is typically better. Perhaps same idea as maximum margin classifier?

## Model Comparison

Model is strictly better than another model if, at every point on ROC curve, it is higher than another model (and similar shape).

Pick model with profit curve that has the maximum value.

# Afternoon Breakout

Credit card fraud: 1% minority

 - SMOTE to create similar data points because fraudsters will try different things to commit crimes

Animal pictures: 25% minority

 - Could do nothing
 - Could also under sample

Spam emails: 2.5%

 - Do nothing because spam is probably like an island in a sea of normal stuff

Cost benefit matrix
 - A: 96, -100, -4, 0
 - B: -4, -100, -4, 0
 - c: 96, 0, -4, 0

B and C are both correct! A is wrong because you are double counting. Units are wrong!! __The difference between top row is the important thing!!!__

__What's important is that we choose the same optimum!!! The constants themselves don't really matter.__

Let's consider money saved.

$58 = 0.5 * 6 more months * $200 * 0.1 - 2 top left
-$4 bottom left
$0 top right
$0 bottom right
