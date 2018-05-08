---
title: SQL Practice 3
date: 2018-05-07 20:44:31
tags: SQL
---

# SQL Practice 3

## Question
Given this table

```SQL
CREATE TABLE Compare (
    Numbers INT
)
```
Write a SQL query that will return the maximum value from the “Numbers” column, without using a SQL aggregate like MAX or MIN.

## Answer
There are two ways to do that, first we can use a self join

```SQL
SELECT DISTINCT Numbers
FROM Compare
WHERE Numbers NOT IN (
    SELECT Smaller.Numbers
    FROM Compare AS Larger
    JOIN Compare AS Smaller ON Smaller.Numbers < Larger.Numbers
)
```
Second, which is easier, we can sort and limit the number of returned results.

```SQL
SELECT Numbers
FROM Compare
ORDER BY Numbers DESC
LIMIT 1
```
## Reference
- https://www.programmerinterview.com/index.php/database-sql/find-maximum-value-without-using-aggregate/