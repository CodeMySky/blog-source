---
title: SQL Practice 2
date: 2018-05-05 14:48:06
tags: SQL
---

# SQL Practice 2

## Table Descirption
|   User    | UserHistory |
| --------- | ----------- |
| user_id   | user_id     |
| name      | date        |
| phone_num | action      |

## Questions
- Return the name, phone number and most recent date for any user that has logged in over the last 30 days. 
    - Every time a user logs in a new row is inserted into the UserHistor table with user_id, current date and action "logged_on"
- Determine which user_ids in the User table are not contained in the UserHistory table.
    - assume the UserHistory table has a subset of the user_ids in User table.
    - Do not use the SQL MINUS statement.
    - The UserHistory table can have multiple entries for each user_id.
- Avoid using subqueries.

## Answers

```SQL
-- Return the name, phone number and most recent date for any user that has logged in over the last 30 days.

SELECT u.name, u.phone_num, max(uh.date) last_login_date
FROM User u JOIN UserHistory uh
ON u.user_id = uh.user_id
WHERE uh.action = 'logged_on' AND uh.date >= date('now','-30 day')
GROUP BY u.user_id, u.name, u.phone_num;
```

```SQL
-- Determine which user_ids in the User table are not contained in the UserHistory table.
SELECT u.user_id
FROM User u LEFT JOIN UserHistory uh
ON u.user_id = uh.user_id
WHERE uh.user_id IS NULL;
```

## Reference
- https://www.programmerinterview.com/index.php/database-sql/practice-interview-question-2/

## Preparation Statements
```SQL
DROP TABLE User;
CREATE TABLE User (
    user_id int,
    name varchar,
    phone_num varchar
);
INSERT INTO User ('user_id', 'name', 'phone_num')
VALUES
(1 , 'Abe'    , '111' ),
(2 , 'Bob'    , '222' ),
(5 , 'Chris'  , '333' ),
(7 , 'Dan'    , '444' ),
(8 , 'Ken'    , '555' ),
(11, 'Joe'    , '666' );
DROP TABLE UserHistory;
CREATE TABLE UserHistory (
    user_id int,
    date varchar,
    action varchar
);
INSERT INTO UserHistory ('user_id', 'date', 'action')
VALUES
(1, '2018-05-05', 'logged_on'),
(2, '2017-12-01', 'logged_on'),
(5, '2017-12-01', 'logged_on'),
(5, '2018-05-05', 'logged_on'),
(7, '2018-05-05', 'logged_off'),
(8, '2018-05-01', 'logged_on'),
(8, '2018-05-05', 'logged_on');
```