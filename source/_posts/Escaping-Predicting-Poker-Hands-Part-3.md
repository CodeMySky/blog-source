---
title: 'Escaping: Predicting Poker Hands, Part 5'
date: 2016-01-17 15:11:08
tags: [predicting-poker-hands,sklearn,random-forest]
---
Tom decided to use random forest model, a famous model which is easy to use, to predict poker hands. He wanted to introduce it, but first, he want to introduce decision tree model.
<!--more-->
# Decision Tree
Random forest is a famous algorithm that has a good performance, and it is easy to explain how it works.
But before Tom introduce Random forest model to you, he want first cover the concept of a decision tree.
A decision tree is a tree-like classification model of decisions and their possible consequences. Here is a decision tree of how to distinguish products from Apple.

![An image of a decision tree](/2016/01/17/Escaping-Predicting-Poker-Hands-Part-3/decision-tree.png)

Sklearn provides an simple way to generate a decision tree
```python
from sklearn.tree import DecisionTreeClassifier
clf = DecisionTreeClassifier()
clf = clf.fit(X_train, y_train)
```

# Random Forest
A decision tree is too simplistic and very sensitive to biased data, to avoid this bias, Tom can use multiple decision trees, which ensembles a random forest model. A random forest arbitrarily chooses a subset of all the features and builds a decision tree with them. With the increase of tree numbers, the biases will tend to balance.

Sklearn also provides an simple way to use the random forest model directly.
```python
from sklearn.emsemble import RandomForestClassifier
clf = RandomForestClassifier()
clf = clf.fit(X_train, y_train)
```

# Baseline
So Tom decided to use random forest directly to generate a baseline.

{% gist 6728a29115522ab83bdb baseline.py %}
Result of the classifier:

|Class      |precision|recall|f1-score|support|
|-----------|---------|------|--------|-------|
|0          |0.64     |0.81  |0.71    |501209 |
|1          |0.57     |0.49  |0.53    |422498 |
|2          |0.38     |0.00  |0.01    |47622  |
|3          |0.50     |0.00  |0.01    |21121  |
|4          |0.25     |0.00  |0.00    |3885   |
|5          |1.00     |0.00  |0.01    |1996   |
|6          |0.00     |0.00  |0.00    |1424   |
|7          |0.00     |0.00  |0.00    |230    |
|8          |0.00     |0.00  |0.00    |12     |
|9          |0.00     |0.00  |0.00    |3      |
|avg / total|0.59     |0.61  |0.58    |1000000|

Confusion Matrix:

|		|0      |1      |2		 |3     |4    |5    |6    |7    |8    |9    |
|---|-------|-------|------|------|-----|-----|-----|-----|-----|-----|
|0  |404468 |96730  |8     |1     |2    |0    |0    |0    |0    |0    |
|1  |213328 |208879 |245   |39    |7    |0    |0    |0    |0    |0    |
|2  |12214  |35180  |214   |14    |0    |0    |0    |0    |0    |0    |
|3  |3589   |17411  |55    |66    |0    |0    |0    |0    |0    |0    |
|4  |352    |3525   |5     |0     |3    |0    |0    |0    |0    |0    |
|5  |1810   |179    |0     |0     |0    |7    |0    |0    |0    |0    |
|6  |67     |1317   |33    |7     |0    |0    |0    |0    |0    |0    |
|7  |3      |219    |4     |4     |0    |0    |0    |0    |0    |0    |
|8  |2      |10     |0     |0     |0    |0    |0    |0    |0    |0    |
|9  |1      |2      |0     |0     |0    |0    |0    |0    |0    |0    |

From all the statistical result shown above, we can see that the model did a poor job classifying every thing except class 0 and class 1. In fact no class other than class 0 or class 1 are in the prediction result. The result was bad but Tom decided to start from here.

# Acknowledgement
[https://en.wikipedia.org/wiki/Decision_tree](https://en.wikipedia.org/wiki/Decision_tree)
[http://scikit-learn.org/stable/modules/tree.html](http://scikit-learn.org/stable/modules/tree.html)
[http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)
