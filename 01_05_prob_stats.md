# Morning Lecture

 - if we have A -> B then:
  * if we observe A, B is true
  * if we observe B', A is false
  * But what about if we observe B? In deductive reasoning this means nothing. Here is where we want probability/statistics
 - (AB)' is not the same thing as A'B'
  * (AB)' = NOT AB, either A or B could be False and statement would evaluate to True
  * A'B' = NOT A AND NOT B
 - A' is set notation, Abar is probability notation

# Morning Breakout

Assumptions we make for this: P(7d and Qh)
 - a standard deck of 52 cards with 4 suits and 13 ranks
 - order doesn't matter
 - draw 2 cards?
 - 7d might mean 7 diamonds?
 - replacement?
 - shuffling?
 - cards are face down

What about this: P(7 or Q)
 - I took this to mean the probability of getting either 7 or Queen when drawing one card. = 8/52

# Afternoon Lecture

 - A random variable maps possibilities from sample space to a real number so we can do math with it
 - we use standard deviation instead of variance because of units. Units of standard deviation will match with units of distribution but variance will be units<sup>2</sup>.
 - we use correlation over covariance because it is unitless and also standardized value between -1 and 1
 - _Beware_ Anscombe's Quartet. Totally different distributions can give the same summary statistics (mean, stddev, and r<sup>2</sup>)
  * However, we often don't have the opportunity to plot our data and we don't have anything beyond summary statistics and correlations! We need to be very careful in these situations to be very certain that you aren't fooling yourself.
  * How do we resolve these issues? This problem lies at the core of why data science exists.
 - __Useful Thoughts:__
  * Discrete or continuous? If there's stuff after decimal it's usually Discrete


# Afternoon Breakout

 - What is a random variable? It is a way to map from sample space to real numbers so that we can do math. It is a variable whose possible values are numerical outcomes of a random phenomenon
 - Examples?
  * H -> 1 and T -> 0
  * Dice: 1 -> 12, 2 -> 27, 3 -> -5, 4 -> 0, etc. _this random variable sucks_
 - P(X = x): for PROBABILITY MASS FUNCTION
 - f(X = x): for SOME FUNCTION, in general a PROBABILITY DENSITY FUNCTION. There is some value for X at x, but there is typically no meaning.

__USEFUL RELATIONSHIPS BETWEEN RANDOM VARIABLES__
 - E[aX + bY] = aE[X] + bE[Y]
 - var[aX + bY] = avar[X] + bvar[Y] + 2*covariance[X,Y]
 - covariance[X,Y] = E[X * Y] - E[X]\*E[Y]

Products of random variables are even harder to deal with than sums. Everything is hard. 
