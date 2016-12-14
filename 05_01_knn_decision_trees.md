# Morning Breakout

__The curse of dimensionality__

As dimensions are added, your sample space becomes very very large. You need many, many more points to fill it in in order for points to become close enough for their 'nearness' to have any meaning. Any relationship drawn from adjacency of points in a huge space can only be tenuous at best.

__Training runtime vs. predicting runtime__

You want training runtime to be low for any model that needs to be quickly updatable and changed on the fly. Any situation that ingests a large amount of information quickly. Examples: music recommender system should immediately change if you give thumbs up or down; fraud detection system should immediately be able to incorporate new information (i.e. attemps at fraud).

You want prediction runtime to be low for any model that is predicting something that has a time limit. Examples: fraud (again); ad recommender system because you only have 20 ms to predict best ads and load page before person leaves.

# Afternoon Lecture

## What are we actually shipping?

__Offline__

Given data and asked to tell story behind data. Build model from this. This model is used to make some sort of business decision.

Complete loop where we start with static data and then train and predict in complete circle.

E.g. price of used car (blue book); insurance pricing model

__Online__

World of data _products_. In real time (or close) take data and predict that point with models that were already trained.

Training model here is trying to figure out the function that gives you closest output to reality given input. Keep this function around and use on new data points that are coming in.

## Cross-Validation and Information Hygiene

Common training, validation, and test splits: 60/20/20, 70/15/15, somewhere in that range.

If you are going to impute (SMOTE, undersample, etc.) - _do that on the training subset only!!!_ (The 60, 70, whatever section).

If you get a really bad score - sorry... you're done. _You do not get to start over_. You've now tainted all your data. _Any decision you make after this point was made using all of the data._

It is not impure to check if your splits are truly random. If you realise that your split led to very weird partitions then you are allowed to go back an split. It is ok to go back and seek randomness.

# Afternoon Breakout

## Decision Tree Regression

1. Decide splits based on reducing standard deviation of resultant bins
2. Try every single point as a split. I.e. for each feature you consider n number of potential splits (where n is the number of data points). Then do this for all features and that's just to decide one split.
3. Follow the tree to make prediction
