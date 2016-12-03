# Morning Lecture

In linear regression we are trying to minimize the sum of square errors (i.e. e<sup>T</sup>e where e is a column vector of errors with same length as y).

We assume independence of errors.

e = (Y-Xβ)

We want to minimize (Y-Xβ)<sup>2</sup> (this is cheating because this isn't how matrix calculus works). Take derivative of this with respect to β and set to 0.

- -2X(Y-Xβ) = 0
- X(Y-Xβ) = 0
- X'Y - X'Xβ = 0
- X'Y = X'Xβ
- (X'X)<sup>-1</sup>X'Y = β

Most computers are not going to solve this equation for OLS because it turns out finding the inverse of (X'X) is hard computationally and running gradient descent is easier.

Residual sum of squares (RSS), as an absolute value is not very useful. Has to considered relative to the quantities you are trying to model. On the other hand, the residual standard error (RSE) tells us - on average - the error for each data point.

Fraction of unexplained variance = 1 - ∑(y-yhat)<sup>2</sup>/∑(y-ybar)<sup>2</sup>

 - (y-yhat): unexplained deviation
 - (yhat-ybar): explained deviation
 - (y-ybar): total deviation

How do we know if we should try adding another feature in? We could just try it; if it's useless its β will just be 0. We can also do an F-test to see if the β is 0.

The constant typically loses its meaning at the y-intercept (i.e. when x is 0).

If the coefficients are significant then most likely the F-statistic is also going to be significant. It's not really that useful.

Recall: p-value is the probability of seeing test statistic the same or more extreme as the one observed - given that the null hypothesis β = 0 is true. If p-values are small then we reject the null and so the independent variable is probably useful.

## Assumptions:

 - There is actual a linear relationship between ind and dep variables
  * Can try a non-linear transformation of data


 - Constant variance (homoscedasticity)
  * Can fix coning with log transformation of y


 - Independence of errors. This can trick you into thinking you're awesome at linear regression when you actually suck. __This is probably the most important assumption in linear regression.__
  * If we have a time series then we add time step?


 - Normality of errors
  * Outliers will violate this assumption


 - Lack of multicollinearity


## Leverage
 - Linear regression will want to chase one very extreme outlier and try to fit it. That point is said to have a lot of _leverage_. THey are not necessarily bad, and may even be good data, but they have a lot of weight.

 - Leverage can sometimes not be a problem at all if it directly falls in line with the other data. It still has a lot of leverage but doesn't change the regression anyway.

 - Leverage

## Multicollinearity
 - __Does not cause bias in model__

With perfect multicollinearity, it is easy to detect because our model will fail to run.

Can I predict another ind variable with a ind variable? If I can, then drop one.

## QQ Plots
 1. Make n predictions.
 2. Take normal distribution and split it up into n chunks whose regions under curve are equal to 1/n
 3. Match up my predictions with the normal distribution
 4. This should give us straight line

# Afternoon Lecture

For categorical variables, create vector with 1 for 'yes' and 0 for 'no' and still follow same: Y = β<sub>0</sub> + βX<sub>male</sub> + βX<sub>female</sub>

Cannot really include multiple categorical variables. In general, drop one of the options.

We can lose some intuition if we add multiple categorical variables and drop options. E.g.

Y = β<sub>0</sub> + βX<sub>male</sub> + βX<sub>caucasian</sub> + βX<sub>masters</sub>

The constant β<sub>0</sub> should correspond to the 'baseline person'. This baseline person may not actually exist in the dataset in this case. Went from being (y-ybar) to (y-0).

I get a benefit from increasing TV advertising but the existing radio advertising makes the effect of increased TV advertising even more. The combined effect is greater than the sum of parts! Our intuition is that people who see it from multiple mediums are more likely to buy.

The most useful thing we can have domain knowledge - not just a fast computer. Being able to create features that best represent the real world will give us a much better model. Anybody can run gridsearch and try all possibilities. But only people who understand the underlying structure will be able to create features that represent the true relationship.

__Gotta brush up on ma tractor knowledge__

## Non-linear Features

Just because relationship is non-linear, doesn't mean we can't use linear regression.

It is good practice to put in regular variable term in along with polynomial term. E.g. you should put in TV and radio along with TV\*radio.

We are not only limited to transforming X. We can also transform Y.

__Clumping is the worst case scenario for linear regression__

If we see clumping, we want to make a transformation such that the data clump becomes a cloud.

When we do these transformations, we no longer have easy to understand correlation: for a given 1 unit increase in x, we get β increase in y.

## OLS stuff

- Let's fit a simple model using the fancy formulas

- Similar to R

- Y ~ X means Y is somehow dependent on X
    * 'Balance ~ Income + I(Age>60)' I is if you're going to transform variable here we're doing a boolean test for whether or not people are older than 60

    * 'Balance ~ Income + Student + Income*Student. This is because we think that low income people who are students will have different spending than people who are actually low income

    * 'Sales - TV*Radio-TV-Radio-1'. We must explicitly remove these otherwise OLS will put these in for you will see increase in R2 because we removed constant but it doesn't mean our model is actually better!

- Which model is best? 2-degree, 3-degree, or linear?
    * We want adjusted R squared, not regular R squared
    * And t-statistic for betas


- First try using engineered features of independent variables - so long as there no clumping in Y.
