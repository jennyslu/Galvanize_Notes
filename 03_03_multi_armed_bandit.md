# Morning Lecture

## Frequentist
* We cannot say that there is a x likelihood that site A is better than site B with frequentist statistics because

* We must wait until we have accumulated the number of observations required for the power we desire with frequentist statistics. We are not allowed to stop the test early

* We cannot update the test while it is running

## Bayesian
* The rise of Bayseian statistics has largely come as a result of the increase of computing power

* __Now__ we can say that it is 95% likely that site A is better than site B. We can calculate the posterior probability that A is better than B, etc.

* We _can_ stop test early if we see suprising data

* We _can_ update the test with new stuff while it is running. Because our posterior becomes our prior and it's a continuous cycle, we never really have to design a new "test" per se.

* Likelihood function:
 - Binomial(n = number of visitors, p = conversion rate, k = number of conversions). This is something we can calculate explicitly given a certain number of visitors and conversions


* Prior:
 - Beta distribution (defined by p, α, and β)


* Posterior:
 - beta(certain shape parameters) X binomial -> beta(different shape parameters)
 - Now, if we were to run this forever and ever we would only ever get binomial and beta distributions. We just go from a beta distribution to beta distribution
 - We don't technically need to drop the normalizing constants (ie. B(a,b) for beta and (n choose k) for binomial) - they will work themselves out
 - These are called conjugate priors: prior and likelihood pair results in posterior being the same distribution as prior


* Lee thinks all A/B tests should be run with Bayesian methodology. Beta distributions are a natural fit for A/B testing. They are also conjugate to Bernoulli and binomial likelihood functions

## Multi-Arm Bandit

 * Was named for slot machines. Slot machines were referred to as one-armed bandits because of the pull arm and for stealing your money. Multi-armed bandit states that there is a bank of _K_ slot machines with one that has a higher payout. How do we figure out which one that is and spend the least amount of money doing so?

 * There are K actions we could take and each action gives us a certain reward. Each action's reward distribution is different but they do not change over time.

 * Ultimately, we want to decide which machines to play, how many times to play each machine, and in which order to play them to maximize the sum of rewards from our sequence of level pulls.

 * __Exploration__ - agent attempts to acquire new knowledge

 * __Exploitation__ - agent attempts to optimize decisions based on existing knowledge

 * The agent attempts to simultaneously balance these competing tasks in order to maximize total value over the period of time considered

 * Regret is a measure of how often you picked the wrong machine to play. 0 regret would mean we only ever play the best machine.
  - _Lower is better_
  - You can't actually calculate it when you run real test


 * This problem maps perfectly to A/B testing

 * Bandit algorithms attempt to __minimize regret__

  - epsilon-greedy: Explore with fixed probability. Typically use ε of 10% or less. The best lever is selected for a proportion 1-ε of the trials, and a lever is selected at random (with uniform probability) for a proportion ε. A typical parameter value might be ε=0.1, but this can vary widely depending on circumstances and predilections. This is kind of a crappy strategy

  - upper confidence bound (ucb1): Calculate a probability for each bandit (i.e. for each action j, record average reward and number of times we tried j). Try the action that maximizes formula. Forces you to explore, but you still _tend_ to play the best one the most. Achieves regret that only grows logarithmically with the number of actions

  - softmax: Choose the bandit random in proportion to its estimated value. Happens to be same as Boltzmann distribution. Softmax function gives probability distribution over K different possible outcomes (i.e. K bandits). We do not just play the one with the maximum value. There is always some probability that we will play any of them, but we are weighting them based on the reward that they have paid out in the past. Depends _a lot_ on τ. If τ is large, it basically becomes uniform (i.e. play all equally). If τ is small, we basically keep playing the first one to pay out over and over.

  - Bayesian bandit: Treat each bandit as a beta distribution and look at their posterior distributions. We have website that we will use as our prior (passing baseline conversion and 1-baseline as shape parameters). Anytime someone comes in, we draw from each of the beta distributions for _each version of the website_ and then, whichever is higher - we send that person to the page that this highest number came from. We essentially always end up playing the best one.

# Afternoon Breakout

In the frequentist world, the thing we are investigating is a fixed element and its probability is the average result given an infinite number of trials. The Bayesian approach is that probabilities measure beliefs abou the outcome. In this case, there is no need - nor is there any meaningful discussion - to talk about what happens with an infinite number of trials.

Frequentist hypothesis tests return p-values, which are both hard to understand and hard to make business decisions upon. A Bayesian approach, on the other hand, returns a posterior probability distribution that is more easily understood than a p-value. It can also yield expected values and other metrics that make for easier decision making

Personally, I find Bayesian statistics to be a little unsettling. Maybe it's because of my background and lack of exposure to it but I do think there is something to be said about the safety and comfort you get from the 'strength of numbers' for lack of better terminology.
