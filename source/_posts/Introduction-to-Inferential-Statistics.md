---
title: Introduction to Inferential Statistics
date: 2016-02-22 15:24:01
tags: ['z-test','t-test','f-test','statistics']
---
As a data scientist, we have different methods to treat the data. But how can we know the whether our methods are doing good or our model is just guessing.
We can use a serious of test to decide whether our model is good or not including z-test, t-test and f-test.
<!-- more -->
# Estimation
## Population Mean vs. Sample Mean
For a very large population, it is very hard to get the distribution of the entire population. Whatever distribution of population is, the sample mean will have a normal distribution.
## Margin of Error
$$
\frac{2\sigma}{\sqrt{n}}
$$
## Confidence Bond
## Z-Score
# Hypothesis Testing
## Alpha Value
## Z Critical Value
## One Tail/Two Tail Test/Critical Value
## Reject/Retain Null
## Alternative Hypothesis

# Z-test, T-test and F-test
We can use statistics to compare whether two things are the same.
For example, we want to know if a drug takes effect on patients.
We can compare treated patients with normal patients.

# Z-test
If we know the mean and standard deviation of the population, we can 
determine whether a sample mean fails in the range of the sampling distribution of 
the population.

That is Z-test.

## Z-Distribution
The distribution of Z score looks like this.
TODO: Image of Z Distribution.

## Way to calculate Z-score
$$
Z = \frac{Sample Mean}{Standard Error}
$$

## Z-Critical Value
If the Z-score is larger than Z critical value, we can determine that the sample is different from the population.
normally we use three alpha levels:
1. alpha = 0.05 (5%)  <->  z critical value = 1.65
2. alpha = 0.01 (1%)  <->  z critical value = 2.33
3. alpha = 0.001 (0.1%) <-> z critical value = 3.08

## One tail test and two tail test
As we can see from the Z-distribution, we can have test the treatment in two ways.
### One tail test
We can only test the direction of the treatment group. Like is the treatment lower than the normal group or the treatment higher than normal group.
We can also test both directions of the treatment group. For example, whether the treatment is different from the normal group, either higher or lower.

# T-Test
## What is T-Test

## T-Distribution
## Degree of freedom
## One Tail T-test/Two Tail T-test
## T-statistics
## P-Value
## Cohen's d
## R^2
## SEM
## Dependent Sample
## Independent Sample
## Pooled Variance
## Corrected Standard Error


Credit to [Intro to Inferential Statistics](https://classroom.udacity.com/courses/ud201/) 
