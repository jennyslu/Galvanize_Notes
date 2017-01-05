# Afternoon Lecture

We got pretty uninterpretable results from SVD. The negative values in particular were annoying because you would have to look up corresponding U value for that negative value in V to know whether or not the ultimate result is positive or negative.

Let's make everything positive instead!

__Additive__ W and H only have positive values and so we know that the dot product will always be negative and only ever increase with additional terms.

## Interpretation of topic-ness/latent features

* __W__ Pick some number of the documents that have high values of topic. Look at those documents and try to figure out what topic is.
 - Each row in W is the amount that latent feature comprises each document


* __H__ Pick some number of words that have high values of topic. Look at those words and try to figure out what that topic is.
 - Each column in H is the amount that word contributes to all latent features

__Never yield a unique solution due to non-negativity constraint.__ This is the price we pay for interpretability. If you do multiple runs the qualities of each topic will probably remain the same but numbers will vary wildly.

## Versus PCA and SVD

 - PCA and SVD decompose into three matrices
 - NMF only into two

 - __Non-negativity contraint__ is the main difference

 - __Bases__ are also not orthogonal like in PCA & SVD (also not even known and not technically an official vector basis)

## How do we actually do this?

### ALS: Alternating Least Squares

Hold W constant and solve for best H we can. Enforce non-negativity by clipping all negative values to be 0.

Hold H constant now and then solve for best W. Enforce non-negativity.

* Fast, works well in practice

* Non-negativity enforced in a weird ad-hoc way, not guaranteed to find a local minimum must less global, may not even converge (no convergence theory)

* Have only reconstruction error as quantitative metric. PCA and SVD topics are ordered by importance so we know % explained variance of each topic and we know which ones are more important. We do not know this with NMF!

### Gradient descent

We are descending the gradient of cost function. So what is our cost function? __Forbenius norm of (X - WH)__. This is basically norm of squared difference between X and WH.

Updates will get smaller and smaller (there is convergence theory).

# Afternoon Breakout

NMF is an unsupervised model!!

NMF is a useful hammer.

What are we modeling?
 - Latent features
 - It is doing this by trying to BEST describe (i.e. minimize reconstruction error) X with WH

What hyperparameter choice must you make before performing NMF?
 - k
 - why would we choose one value of k over another? Domain knowledge? Prior constraint where you've already decided k?

Why might we run NMF using same hyperparameter multiple times?
 - all the non-unique values will minimize the reconstruction error the same amount
 - however, pick the one that you like best. The most aesthetically pleasing one.

Eg. NMF on Strava
 - with only one latent feature we would expect that LF to be 'power' (technically inverse power because smaller time means more powerful rider) because that is the best single value to attempt to reconstruct ride times for all riders

Should we standardize?
 - Dividing by standard deviation is probably not awful but will probably not help
 - Subtracting mean will result in negative numbers which will fail

Sparse matrices?
 - Single latent feature will be 0 if times are all 0 for that one rider
 - Still will have value for rider with times but will do a _bad job of reconstructing the 0s_
 - This is the issue with NMF and sparse matrices - will do bad reconstruction

Moving rating utility matrix?
- Audiences for movies??
- W: users like certain 'topics' a certain amount
- Many things about movies. Not just genre. Cast, producer, era, etc.

Differs from k-means. NMF is a soft clustering technique.
