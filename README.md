# nfl

Simple XGBoost classifiers aiming to classify NFL Draft prospects as hits or misses in terms of fantasy football production.

##How it Works
This is a binary classifier, that means it classifies samples as either 0 or 1, in reality, it outputs the probability of a sample being classified as 1, which is what we want (It consider a 1 everything that has a probability of over 50%).

In our case, the positive outcome (the 1) is whether a player finished among the top 24 in terms of PPR scoring for his position at any given point in his career.

The data we feed the model includes draft position, college conference, college production stats (raw stats, team relative stats, dominator scores, etc) and physical profile (Combine/Pro Day measurements and RAS/Freak score among others). In total, our model looks at more than 200 variables to output our probability.

##So, how do we train this model?

We gather the data for players drafted between 2004 and 2016 and validate it with a 5-fold cross-validation. This mean that we separate 20% of the data and train on the remaining 80%, then we do it 4 more times changing which part of the data is the 20% each time, at the end, we have an average performance for the model. At the same time, we iteratively try to optimize the model's parameters, this means we redo the 5-fold cross-validation 100 times and get the best average performance (if you've been keeping up, it means we trained a total of 500 models for each position).

##But how do we gauge model performance?

First we need to understand what false positives and false negatives are.

False Positive - A sample classified as a positive, but it's actually a negative. In our case, a player the model thought was good, but wasn't, a bust essentially.

False Negative - A sample classified as negative, but it's actually a positive. In our case, a good player that the model didn't pick up.

In our model, when we talk about performance, we're talking about the false positive rate, which means we're trying to minimize the amount of false positives. So the model is learning to avoid busts at all costs, even if it may miss on home-runs.

##So, how good is the performance?

Our model achieved a precision of 0.77 for WRs, and 0.72 for RBs. That means that, on average, 75% of the players the model said would "hit" (get a top 24 season) did in fact hit.

Note, however, that this is the average cross-validation performance, so the risk of overfitting is a bit high, in order to get a better idea of model performance we could check how the model performed on players drafted between 2017 and 2021, which we do, here are the predictions the model made for WRs and RB drafted between 2016 and 2021.

**Ideally we would measure the false positive rate these predictions as the true measure of performance of this model, but I couldn't be bothered to do it, some younger player might still put good numbers in the future and I didn't want to deal with that.**

Rookie Predictions
So, without further ado, here are the predictions made by the model for the 2022 rookie class:

WRs - https://pastebin.com/W9iGtHbq
RBs - https://pastebin.com/bXxygMNN
You'll probably note that the probabilities for the best RBs are much higher than the probabilities for the best WRs across both rookies and 2016-2021 players, I have no idea why this happens (Probably easier to pattern match successful RBs than WRs).
