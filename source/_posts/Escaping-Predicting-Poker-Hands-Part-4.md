---
title: 'Escaping: Predicting Poker Hands, Part 4'
date: 2016-01-17 15:11:08
tags: [predicting-poker-hands,sklearn,random-forest,feature-extraction]
---
New features are created to help improve performance.
<!-- more -->
So what may be a good feature, maybe the number of types for each card is important,
So Tom added number of value counts as feature and see the performance.

|Class    |precision|recall|f1-score|support|
|---------|---------|------|--------|-------|
|0        |0.59     |0.75  |0.66    |501209 |
|1        |0.52     |0.44  |0.47    |422498 |
|2        |0.24     |0.00  |0.00    |47622  |
|3        |0.18     |0.00  |0.00    |21121  |
|4        |0.05     |0.00  |0.00    |3885   |
|5        |0.99     |1.00  |0.99    |1996   |
|6        |0.00     |0.00  |0.00    |1424   |
|7        |0.00     |0.00  |0.00    |230    |
|8        |0.00     |0.00  |0.00    |12     |
|9        |0.00     |0.00  |0.00    |3      |
|avg/total|0.53     |0.56  |0.53    |1000000|

The performance was worse, the average f1-score is only 0.53. but we can see that class 5 improves a lot. That means class 5 is very sensitive to the number of classes. So for the sake of class 5, Tom decided to keep these four features. Then Tom decided to add the value counts of the value of each card as feature and see the performance

|      class|  precision|    recall|  f1-score|   support|
|-----------|-----------|----------|----------|----------|
|          0|       0.99|      1.00|      0.99|    501209|
|          1|       1.00|      1.00|      1.00|    422498|
|          2|       0.97|      1.00|      0.99|     47622|
|          3|       0.99|      1.00|      0.99|     21121|
|          4|       0.99|      0.07|      0.14|      3885|
|          5|       0.98|      0.06|      0.11|      1996|
|          6|       1.00|      0.01|      0.02|      1424|
|          7|       0.00|      0.00|      0.00|       230|
|          8|       0.00|      0.00|      0.00|        12|
|          9|       1.00|      0.33|      0.50|         3|
|avg / total|       0.99|      0.99|      0.99|   1000000|

Class 0,1,2,3 were performing very well with a very high F1 score. Amazingly, Tom saw that, previously, the class 5, which had very high f1-score in the previous experiment, had a very low f1-score this time. So Tom decided to look into the four types of cards of class 5.

|   | t1| t2| t3| t4|
|---|---|---|---|---|
|73 |  0|  0|  0|  5|
|660|  0|  0|  0|  5|
|698|  0|  0|  0|  5|
|814|  0|  5|  0|  0|
|922|  5|  0|  0|  0|
|976|  0|  0|  0|  5|

We can see that the features of all class 5 is that it has 5 cards of same type. So the number of cards of same type is important. Tom need to perform a value_count of number of types again.

|      class|  precision|    recall|  f1-score|   support|
|-----------|-----------|----------|----------|----------|
|          0|       0.99|      1.00|      1.00|    501209|
|          1|       1.00|      1.00|      1.00|    422498|
|          2|       0.97|      1.00|      0.99|     47622|
|          3|       0.99|      1.00|      0.99|     21121|
|          4|       1.00|      0.07|      0.13|      3885|
|          5|       0.99|      1.00|      1.00|      1996|
|          6|       1.00|      0.01|      0.02|      1424|
|          7|       0.00|      0.00|      0.00|       230|
|          8|       0.00|      0.00|      0.00|        12|
|          9|       0.67|      0.67|      0.67|         3|
|avg / total|       0.99|      0.99|      0.99|   1000000|

Aha, we have class 5 back again, with 1.00 f1-score. We can still see that for class 4,6,7,8 even though they have a lower support, their f1-score is nearly zero.

Since the value of a card means something, so maybe the fluctuation of the value may be a useful feature. Let's try to add standard deviation to the feature list.

|      class|  precision|    recall|  f1-score|   support|
|-----------|-----------|----------|----------|----------|
|          0|       1.00|      1.00|      1.00|    501209|
|          1|       1.00|      1.00|      1.00|    422498|
|          2|       0.97|      0.99|      0.98|     47622|
|          3|       0.99|      1.00|      0.99|     21121|
|          4|       1.00|      0.90|      0.95|      3885|
|          5|       1.00|      1.00|      1.00|      1996|
|          6|       1.00|      0.04|      0.08|      1424|
|          7|       0.00|      0.00|      0.00|       230|
|          8|       1.00|      0.58|      0.74|        12|
|          9|       0.75|      1.00|      0.86|         3|
|avg / total|       1.00|      1.00|      1.00|   1000000|

We can see that class 4,8,9 improves significantly. So we still have class 6 and class 7 to go.
Let's see some sample of class 6.

|        |1   |2   |3   |4   |5   |6   |7   |8   |9   |10  |11  |12  |13 |
|--------|----|----|----|----|----|----|----|----|----|----|----|----|---|
|425     |0   |2   |0   |0   |0   |0   |3   |0   |0   |0   |0   |0   |0  |
|513     |2   |0   |0   |3   |0   |0   |0   |0   |0   |0   |0   |0   |0  |
|1533    |0   |2   |0   |0   |0   |0   |0   |0   |0   |0   |3   |0   |0  |
|2582    |0   |0   |0   |0   |2   |0   |0   |0   |0   |0   |3   |0   |0  |
|2710    |2   |3   |0   |0   |0   |0   |0   |0   |0   |0   |0   |0   |0  |
|3408    |0   |2   |0   |0   |0   |0   |0   |3   |0   |0   |0   |0   |0  |
|4732    |0   |0   |0   |3   |0   |0   |0   |0   |0   |2   |0   |0   |0  |
|4900    |0   |0   |0   |0   |0   |0   |0   |0   |2   |0   |0   |0   |3  |

We can clearly see that every single of class 6 have 2 cards of same value and 3 cards of another value. So we add another value_count to card value. Let's see the result.

|           |  precision|    recall|  f1-score|   support|
|-----------|-----------|----------|----------|----------|
|          0|       1.00|      1.00|      1.00|    501209|
|          1|       1.00|      1.00|      1.00|    422498|
|          2|       1.00|      1.00|      1.00|     47622|
|          3|       1.00|      1.00|      1.00|     21121|
|          4|       1.00|      0.90|      0.95|      3885|
|          5|       1.00|      1.00|      1.00|      1996|
|          6|       1.00|      1.00|      1.00|      1424|
|          7|       1.00|      1.00|      1.00|       230|
|          8|       1.00|      0.83|      0.91|        12|
|          9|       0.00|      0.00|      0.00|         3|
|avg / total|       1.00|      1.00|      1.00|   1000000|

It's almost perfect, except f1-score of the class 9 has fallen back to 0. Let's take a last look at class 9, only 9 of them in training set.

|   | t0| t1| t2| t3| t4| t5| v0| v1| v2| v3| v4|       std|
|---|---|---|---|---|---|---|---|---|---|---|---|----------|
|0  |  3|  0|  0|  0|  0|  1|  8|  5|  0|  0|  0|  4.827007|
|1  |  3|  0|  0|  0|  0|  1|  8|  5|  0|  0|  0|  4.827007|
|2  |  3|  0|  0|  0|  0|  1|  8|  5|  0|  0|  0|  4.827007|
|3  |  3|  0|  0|  0|  0|  1|  8|  5|  0|  0|  0|  4.827007|
|4  |  3|  0|  0|  0|  0|  1|  8|  5|  0|  0|  0|  4.827007|

They are identical in all features we provided. What's going wrong?
Let's see the importance generated by the random forest for each variable.

Original Data: 3.13840253e-04   8.63378290e-04   2.93833913e-04   8.93158186e-04 2.66203550e-04 7.21029001e-04   2.99621421e-04   7.63922006e-04 3.22009278e-04   7.75312025e-04

Card Type: 3.63704650e-03   1.12721683e-03   5.78188916e-04   4.75681163e-04   3.92180262e-04

Card Value: 3.09162294e-03   3.28160255e-01   3.13719755e-01   2.95350427e-01   3.06464011e-02 3.06507102e-04

Standard Deviation: 1.70024106e-02

The result shows that the original features may interfering the judgement of the random forest.
It's OK to remove features if they are not helpful. So let's try to run the random forest without the original data.

 You can see that after all our feature extraction, the first 10 features, the original features, has a very low importance. We can remove them to see if the performance increased.

|      class|  precision|    recall|  f1-score|   support|
|-----------|-----------|----------|----------|----------|
|          0|       1.00|      1.00|      1.00|    501209|
|          1|       1.00|      1.00|      1.00|    422498|
|          2|       1.00|      1.00|      1.00|     47622|
|          3|       1.00|      1.00|      1.00|     21121|
|          4|       1.00|      0.90|      0.95|      3885|
|          5|       1.00|      0.98|      0.99|      1996|
|          6|       1.00|      1.00|      1.00|      1424|
|          7|       1.00|      1.00|      1.00|       230|
|          8|       1.00|      1.00|      1.00|        12|
|          9|       0.07|      1.00|      0.12|         3|
|avg / total|       1.00|      1.00|      1.00|   1000000|

Even though class 9 still has a pretty low precision, but since the number of it is very large, the recall is 1.00 which means all real class 9 hands are found. That is quite satisfying. And by using this predicting machine, Tom successfully won the game with the tribe, and the tribe let him go.
