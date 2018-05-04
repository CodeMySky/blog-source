---
title: 'Escaping: Predicting Poker Hands, Part 2'
date: 2016-01-17 15:11:08
tags: [predicting-poker-hands,sklearn]
---
We left Tom in a dangerous situation. To win the game, Tom needs to accurately predict poker hands. His predictions must rely on a training and test set. testing.123
<!-- more -->

# Training Set
When data scientists start their analysis process, they usually first split all data into two sets, a training set and a test set. The data used to discover potentially predictive relationships is called a training set. But why do we need a test set? Isn't more data we have the better possibility to find more convincing relationships? The test set tests the validity of relationships established in the training set.

# Test Set
So we need a data set which is independent from a training set to evaluate the result of the training. We assume that the test set has the same distribution as the training set. Usually we take about 30% of the total data set as test set. But while the data set is very large, we can take 5% or 3% to be the test set.

```python
from sklearn.cross_validation import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)
```

In this case, the cards Tom used in games with the tribe becomes the test set.

# Acknowledgement
[https://en.wikipedia.org/wiki/Test\_set](https://en.wikipedia.org/wiki/Test_set)
