# Morning Lecture

## Time Series

Different than regression because regression focuses on the central tendency. In time series the most recent data should be weighted more heavily than long ago data.

### Components
 - Trend
 - Seasonal
  * Doesn't have to be real season seasons
  * Sometimes it's better to think of seasonality as a fraction of the value at the time rather than an absolute difference
 - Cyclic
  * Doesn't have regular repeating interval
 - White noise


# Afternoon Breakout

__Ideally you difference everything out and you are left with noise.__

Noise looks different though. The 'parameterization' is the correlation. How correlated is the point to its preceding point?

## AR = autoregressive model of p

__An autoregressive model of order p predicts y<sub>t</sub> from a linear regression of the p values of y preceding y<sub>t</sub>.__

__y<sub>t</sub> = ε<sub>t</sub> + ψ<sub>1</sub>y<sub>t-1</sub> + ... + ψ<sub>p</sub>y<sub>t-p</sub>__

Create data set where each row is point and its 'features' are the p points before it. Do linear regression to find the ψ for each of the 'features'.

In reality, we wouldn't even know p so we would run several models to see which one did better. We are trying to figure out how much _memory_ our time series have.

We are finding out how many points from the past affect my current point.

It is possible for this model to run away because its so correlated and can eventually become non-stationary.

## MA = moving average model of q

__Instead of regression on my previous values of y, regress on the previous value of the _noise_ term.__

__y<sub>t</sub> = ε<sub>t</sub> + θ<sub>1</sub>ε<sub>t-1</sub> + ... + θ <sub>q</sub>ε<sub>t-q</sub>__

This will tell us how long will it take for a shock to decay.

It is not possible for this model to run away and become non-stationary; shock will always go away.

## Now what?
Once we have got the stationary time series, we must answer two primary questions:

1. Is it an AR or MA process?

2. What order of AR or MA process do we need to use?

## Autocorrelation (ACF)

__AC(y<sub>t</sub>, y<sub>t-h</sub>) = Cov[y<sub>t</sub>, y<sub>t-h</sub>]/stdev(y<sub>t</sub>)stdev(y<sub>t-h</sub>)__

__Cov[y<sub>t</sub>, y<sub>t-h</sub>] = E[(y<sub>t</sub>)] + E[(y<sub>t-h</sub>)] + E[(y<sub>t</sub>)(y<sub>t-h</sub>)]__

__E[(y<sub>t</sub>)] = 0__

x-axis is lag (time difference)

Choose p for MA based on number of lags it takes for partial correlation to become insignificant.

## Partial correlation (PACF)

__PACF = AC<sub>h</sub> - (AC<sub>h-1</sub> + ... + AC<sub>0</sub>)__

__PACF = AC<sub>h</sub> - PACF<sub>h-1</sub>__

At each subsequent point you are only adding on the _additional_ correlation not already accounted for from the first correlation.

At the beginning you are correlating with the same so it should be 1. Choose q for AR based on the number of lags it takes for autocorrelation to become insignificant.

## SARIMA
