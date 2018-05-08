---
title: SQL Practice 1
date: 2018-05-03 22:12:00
tags: SQL
---

# SQL Practice 1

## Table Description

| Salesperson | Customer |     Orders     | highAchiever |
| ----------- | -------- | -------------- | ------------ |
| ID          | ID       | Number         | Name         |
| Name        | Name     | Order_date     | Age          |
| Age         | City     | Cust_id        |              |
| Salary      | Industry | Salesperson_id |              |
|             |          | Amount         |              |

## Questions
- The names of all salespeople that have an order with Samsonic.
- The names of all salespeople that do not have any order with Samsonic.
- The name of all salespeople that have 2 or more orders.
- Write a SQL statement to insert rows into a table called highAchiever(Name, Age), where a salesperson must have a salary of 100,000 or greater to be included in the table.

## Answers

```SQL
-- The names of all salespeople that have an order with Samsonic.
SELECT sp.Name
FROM Salesperson sp
JOIN Orders o ON sp.ID = o.salesperson_id
JOIN Customer c on c.ID = o.cust_id
WHERE c.Name = 'Samsonic';
```

```SQL
-- The names of all salespeople that do not have any order with Samsonic.
SELECT sp.Name
FROM Salesperson sp
WHERE sp.ID NOT IN (
    SELECT o.salesperson_id
    FROM Orders o JOIN Customer c ON o.cust_id = c.ID
    WHERE c.Name == 'Samsonic'
);
```

```SQL
-- The name of all salespeople that have 2 or more orders
SELECT sp.Name
FROM Salesperson sp
JOIN Orders o ON sp.ID = o.salesperson_id
GROUP BY o.salesperson_id
HAVING COUNT(*) >= 2;
```

```SQL
-- Write a SQL statement to insert rows into a table called highAchiever(Name, Age), where a salesperson must have a salary of 100,000 or greater to be included in the table.
INSERT INTO highAchiever (Name, Age)
    SELECT sp.Name, sp.Age
    FROM Salesperson sp
    WHERE sp.Salary >= 100000
;
```

## Advanced Questions

### Question 1
We want to retrieve the names of all salespeople that have more than 1 order from the tables above. You can assume that each salesperson only has one ID.

If that is the case, then what (if anything) is wrong with the following SQL?:

```SQL
SELECT Name
FROM Orders, Salesperson
WHERE Orders.salesperson_id = Salesperson.ID
GROUP BY salesperson_id
HAVING COUNT( salesperson_id ) > 1;
```

### Answer

`Name` is not included in the `GROUP BY` clause.

```SQL
SELECT Name
FROM Orders, Salesperson
WHERE Orders.salesperson_id = Salesperson.ID
GROUP BY salesperson_id, Name
HAVING COUNT( salesperson_id ) > 1;
```

### Question 2
Find the largest order amount for each salesperson and the associated order number, along with the customer to whom that order belongs to. 

### Answer 2
We do this in two steps.

First, we selected out sales person and top orders.

```SQL
SELECT salesperson_id, MAX( Amount ) AS MaxOrder
FROM Orders
GROUP BY salesperson_id
```

Then we match the result with other table to generate final result.

```SQL
SELECT Salesperson.ID, Salesperson.Name, Number AS OrderNumber, Orders.Amount
FROM Orders
JOIN Salesperson 
ON Salesperson.ID = Orders.salesperson_id
JOIN (
SELECT salesperson_id, MAX( Amount ) AS MaxOrder
FROM Orders
GROUP BY salesperson_id
) AS TopOrders
ON TopOrders.salesperson_id = Salesperson.ID AND Orders.Amount = TopOrders.MaxOrder
GROUP BY Salesperson.ID, Orders.Amount;
```

## Reference

- http://www.programmerinterview.com/index.php/database-sql/practice-interview-question-1/
- https://www.programmerinterview.com/index.php/database-sql/advanced-sql-interview-questions-and-answers/

### Salesperson

| ID  | Name  | Age | Salary |
| --- | ----- | --- | ------ |
| 1   | Abe   | 61  | 140000 |
| 2   | Bob   | 34  | 44000  |
| 5   | Chris | 34  | 40000  |
| 7   | Dan   | 41  | 52000  |
| 8   | Ken   | 57  | 115000 |
| 11  | Joe   | 38  | 38000  |

### Customer

| ID  |   Name   |   City   | Industry |
| --- | -------- | -------- | -------- |
| 4   | Samsonic | pleasant | J        |
| 6   | Panasung | oaktown  | J        |
| 7   | Samony   | jackson  | B        |
| 9   | Orange   | Jackson  | B        |

### Orders

| Number | order_date | cust_id | salesperson_id | Amount |
| ------ | ---------- | ------- | -------------- | ------ |
| 10     | 8/2/96     | 4       | 2              | 540    |
| 20     | 1/30/99    | 4       | 8              | 1800   |
| 30     | 7/14/95    | 9       | 1              | 460    |
| 40     | 1/29/98    | 7       | 2              | 2400   |
| 50     | 2/3/98     | 6       | 7              | 600    |
| 60     | 3/2/98     | 6       | 7              | 720    |
| 70     | 5/6/98     | 9       | 7              | 150    |

## Preparation Statements
```SQL
CREATE TABLE Salesperson (
    ID int,
    Name varchar,
    Age int,
    Salary int
);
INSERT INTO Salesperson ('ID', 'Name', 'Age', 'Salary')
VALUES
(1 , 'Abe'    ,61, 140000 ),
(2 , 'Bob'    ,34, 44000  ),
(5 , 'Chris'  ,34, 40000  ),
(7 , 'Dan'    ,41, 52000  ),
(8 , 'Ken'    ,57, 115000 ),
(11, 'Joe'    ,38, 38000  );
CREATE TABLE Customer (
    ID int,
    Name varchar,
    City varchar,
    Industry varchar
);
INSERT INTO Customer ('ID', 'Name', 'City', 'Industry')
VALUES
(4, 'Samsonic', 'pleasant', 'J'),
(6, 'Panasung', 'oaktown' , 'J'),
(7, 'Samony',   'jackson' , 'B'),
(9, 'Orange',   'Jackson' , 'B');

CREATE TABLE Orders (
    Number int,
    Order_date varchar,
    Cust_id int,
    Salesperson_id int,
    Amount int
);
INSERT INTO Orders ('Number', 'Order_date', 'Cust_id', 'Salesperson_id', 'Amount')
VALUES
(10, '8/2/96',  4, 2, 540 ),
(20, '1/30/99', 4, 8, 1800),
(30, '7/14/95', 9, 1, 460 ),
(40, '1/29/98', 7, 2, 2400),
(50, '2/3/98',  6, 7, 600 ),
(60, '3/2/98',  6, 7, 720 ),
(70, '5/6/98',  9, 7, 150 );
CREATE TABLE highAchiever (
    Name varchar,
    Age int
);
```