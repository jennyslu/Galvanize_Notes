# Morning Lecture

## Cross-Validation

__The goal of any model building process is to increase the predictive performance of a statistical model. But you must try to be sure that you aren't tricking yourself__

__Bias__: tends to not change as much with different underlying sample
__Variance__: more likely to vary as underlying sample changes

We are not talking about a _particular_ model but the _procedure_ we use to obtain the model (e.g. logistic vs. sinusoidal)

As complexity is increased - training error should always go down. However, test error is usually more U shaped. Test error is still the sum of bias and variance. But, when you see test and training error diverge you know you are in a high variance regime and could benefit from reducing complexity. When you see test and training error are both high you could benefit from increasing complexity.

1. Randomize

2. Split into training and validation

3. True test set should only ever be looked at once. If you ever go back and redo the model you have compromised the test data and are underestimating the error.

4. If you have enough data then split into three groups: training, validation, and testing. Leave test untouched and validate with validation.

5. Even better: cross-validation. Split into k folds and use 1 fold as test and the rest as training. We don't technically have to have the testing data not overlap in all cases but we get a better result in the end if we do.

6. Do not keep any of them and throw all of them away. Use this to __make decisions__. Cross validation is not for model optimization - it is for decision making.

## Model Selection

Iterate through features and pick the best model you find. Start with simple model with one feature and train.

__Never compare models based on R<sup>2</sup> (because it will always decrease with more features) or MSE on training set is essentially meaningless (guaranteed to be minimum for linear for example).__

## Conclusions

Cross-validation does not make anything better on its own. It gives you data you didn't have.

You could also do bootstrapped cross-validation.

# Morning Breakout

1. How can we increase the complexity of model? Add more features. Feature engineering is very important. Interaction terms are one thing multiplied by another thing.

2. Why does variance increase and bias decrease as complexity increases? As you increase model complexity, you are increasing its flexibility. There will always be noise in the data that your model will be sensitive to if your model is very flexible.

3. How do you determine optimal complexity? Make decisions with validation data and then when you finally report your estimated error you will use your test data, which you kept locked in a vault from the beginning and you don't go back and change anything after that.

4. If you get low train error and high test error, what do you think is happening and what can you do about it? Can do something like backward stepwise selection for example: pick model with p-1 features that increases MSE the least. How do we compare model of p features, p-1 features, p-2 features, etc.? Cross-validation!!! F-test, adjusted R<sup>2</sup>, etc. are all just estimates of the training error that existed before we had the computing power to do validation, which we can better get from cross-validation.

5. Why is k-fold better than simple train/test split? Each data point ends up in validation set and gives you better estimate of error.

6. How many models created in 5-fold cross validation to create two models (A and B)? 10

7. What do we do with al the models we create and how do we determine what single model to keep? We throw all those 10 models away and pick either procedure A or B. If we pick A then we train a new model A on _all_ of the data without sub-setting.

# Afternoon Lecture

Lee thinks anything that reduces features is regularization but others will only be referring to this specific thing when they say it. We will be modifying the cost function to add a term to penalize for extra parameters.

__Normalize__: for each column, subtract mean and divide by standard deviation for each value. This is important for linear regression so that all the coefficients are on approximately the same scale.

## Ridge Regression: L2 Regularization

+ λ\*Σβ<sub>i</sub><sup>2</sup> (not including β<sub>0</sub>)

+ This new term shifts our cost function, which we want to minimize to find solution for βs

If the regularization parameter (λ) is non-negative and not zero, we will increase our training error. The hope, however, is that this causes a reduction of our test error. The larger we make λ, the smaller the sum of β need to be.

It is possible for optimization routine to trade off budget of βs between the individual βs and so it is not a _direct_ penalization of adding extra features.

As we increase λ, we increase model's bias and decrease its variance. λ is a real value that we can choose. Our first hyperparameter!

Tends to force parameters to be small. Computationally easier.

## Lasso Regression: L1 Regularization

+ λ\*Σ|β<sub>i</sub>| (not including β<sub>0</sub>)

+ This new term shifts our cost function, which we want to minimize to find solution for βs. Here we use absolute value instead of β<sub>i</sub><sup>2</sup>.

Even though the value of a coefficient may become 0 and therefore, it makes it irrelevant to the model - this gives us a _different_ result than if we had just manually removed that column from the dataset because the _other_ coefficients are not the same.

Tends to promote sparse models by setting coefficients exactly equal to 0. More computationally demanding than ridge.

## Ridge vs Lasso

For ridge coefficients asymptotically approach 0 while they actually become 0 for lasso. This is because derivative of L1 cost function does not exist at 0. The L1 normal has constant value slope for all values <0 and for all values >0 but there is no value at 0. On the other hand, L2 normal slope is constant line that passes through 0. (Side note, the idea of gradient descent is to move along this line for L2).

In the case that λ is small the results for both lasso and ridge are almost the same because this is almost the same as not regularizing.

When λ becomes large enough we are essentially predicting the mean because the coefficients all become 0 or become close enough to 0.

sklearn calls λ alpha.

All models are classes. Instances have fit() method, predict() method, and score() method.

## Example

1. Clean up X, etc.

2. Decide we want to do linear regression.

3. Linear regression too much so we decide to do lasso and ridge instead.

4. Choose set of alphas to try

5. Do k-fold CV on both ridge and lasso models for each alpha. (Ultimately train and validate k\*number of alpha models)

6. Look at mean scoring metric (i.e. average of k folds) for each of the models. Decide which alpha and model (ridge or lasso) was best.

7. Throw away all these models. Take chosen model and .fit(X_train) <- on all of X data.

8. Get result by scoring final trained model from step 7 on X_test. This is our estimate of what true error will be for model. 
