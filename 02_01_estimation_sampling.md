# Morning Lecture

## Central moments

The m<sup>th</sup> central moment (or moment around the mean) is the expected value of (x - μ)<sup>m</sup> for population or (x - x̅)<sup>m</sup> for sample.

1. Zeroeth central moment = 1
 - Integral of entire PDF from -∞ to +∞. Ensures that the PDF is normalized correctly.
2. First moment = MEAN
 - Based on the distance from the mean
2. Second moment = VARIANCE
 - The things that are further away will have bigger weights now because we are squaring the distance
3. Third central moment = SKEWNESS
 - Stretching to left (-) or right (+)
4. Fourth central moment = KURTOSIS
 - The peakedness of a distribution; (+) greater than normal, (-) less than normal

## Using scipy

 The location (loc) keyword specifies the mean. The scale (scale) keyword specifies the standard deviation. The probability density is always defined in the “standardized” form. To shift and/or scale the distribution use the loc and scale parameters.

## Method of Moments (MOM)

0. Choose an underlying distribution/model
1. If a model has _d_ parameters (i.e. number of parameters required to initialize a probability distribution), compute the first _d_ moments. This gives us _d_ equations with _d_ unknowns.
2. Solve for _d_ parameters, as a function of the moments
3. Based on data, calculate the first _d_ sample moments
4. Insert sample moments into the distributional moments. These are our _method of moment estimators_

## Maximum Likelihood Estimation (MLE)

1. Choose an underlying distribution/model
2. Estimate parameters by finding parameters that maximizes the likelihood of seeing given observations

### f(x|θ) = MLE

## Maximum a Posteriori (MAP)

1. Make MLE estimate
2. Take MLE estimate and multiply by it by a prior. A prior is something that changes our confidence about the MLE.

### f(x|θ)g(θ) = MAP

## Non-parametric

Non-parametric distribution. Non-parametric statistic models has a parameter set that is not fixed and can increase or decrease if new relevant information is collected. Versus, parametric statistics, which assumes that sample data comes from a population that follows a probability distribution based on a fixed set of parameters.

### Kernel Density Estimation (KDE)

1. Make no assumptions about underlying distribution
2. Fit it to the data we see. We do decide how 'thick' the kernels will be

# Morning Breakout

## MLE

What is MLE?
 - model parameter estimation based on maximizing the likelihood of observing the given data

 What is the major difference between MLE and MAP?
  - estimate model parameters given observed data AND a prior belief

Non-coin related example of MAP?
 - suppose you sample a group of people about who they are voting for and you get like 10/100 people saying they will vote for the Democrats. This could be seen as Bernoulli and here you would get 0.1 for p based on MLE
 - However, you know that 50% of the population usually votes for Democrats so MAP allows you to incorporate this prior belief into your model

## Confidence Intervals

Why do we calculate confidence intervals?
 - maybe from a business point of view, we care about the actual value. If bottom of CI is ok then maybe it doesn't even matter
 - to give range of for the estimated value of a parameter

Why is 95% the most common?
 - convention??

Where would we be ok with 70% or 99%?
 - 70%: something we want to happen but if we're close that's cool too. You suspect someone might be counting cards you probably will throw them out at 70%

When you might not want to give CI?
 - if you don't want to make a feedback loop that will actually affect your results by giving a CI

## Facebook Interviews

__Russian roulette__

You already know that you are in one of the blank spots. If you move to the next one, you have a 1/4 chance of getting bullet.

If you spin again, you'll end up somewhere random so you have a 2/6 = 1/3 chance of getting bullet.

Best to pull trigger again.

__Coloured and numbered cards__

We want outcomes we want/total outcomes. Total outcomes = 49 (because we already chose one card). Outcomes we want = 49-9(other cards of that colour)-4(other cards that have that same number) = 36.

36/49

# Afternoon Lecture

## Bootstrapping

1. Start with dataset of size n
2. Sample from dataset _with replacement_ to create 1 bootstrap sample of size n. Bootstrap samples are _always_ done to the same size as your original sample.
3. Repeat B times
4. Each bootstrap sample can then be used as a separate dataset for estimation and model fitting

Can use on things that are not confidence interval-able (e.g. median).

We can do this through two methods but the recommended is to do it empirically, i.e. the percentile method.

As I make my bootstrap samples, I make histogram of them and I want the middle 95% of the samples. Now I could say my median is somewhere within a range between the medians of the bottom and top bootstrapped samples.

# Afternoon Breakout

__Why do we sample?__

To gather information about population.

__What is sampling?__

Take a subset of the greater population, that is hopefully representative.

__What does sampling accomplish/enable?__

Draw conclusions about population without having to gather entire population data (which is probably unrealistic)

__What would we need to do if we could not sample?__

Find proxies and measure those? Use best knowledge available from a similar sample/different population.

__When is sampling useless/superfluous/not helpful?__

If population is really small then there is no point in sampling. We might as well take the whole thing. 
