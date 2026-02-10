# SQL-Practice

This repository contains SQL questions and solutions practiced during self-study.
The focus is on joins, subqueries, aggregation, and filtering.

## Question 1:
```Table: Employee
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+
empId is the primary key column for this table.
Each row of this table indicates the name and the ID of an employee in addition to their salary and the id of their manager.
 
 
Table: Bonus
+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+
empId is the primary key column for this table.
empId is a foreign key to empId from the Employee table.
Each row of this table contains the id of an employee and their respective bonus.
 
 
Write an SQL query to report the name and bonus amount of each employee with a bonus less than 1000.
Return the result table in any order.
The query result format is in the following example.
 
Example 1:
Input: 
Employee table:
+-------+--------+------------+--------+
| empId | name   | supervisor | salary |
+-------+--------+------------+--------+
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |
+-------+--------+------------+--------+
Bonus table:
+-------+-------+
| empId | bonus |
+-------+-------+
| 2     | 500   |
| 4     | 2000  |
+-------+-------+
Output: 
+------+-------+
| name | bonus |
+------+-------+
| Brad | null  |
| John | null  |
| Dan  | 500   |
+------+-------+
```

## Solution 1
```
SELECT E.name, B.bonus
FROM EMPLOYEE  AS E LEFT JOIN BONUS AS B
ON E.empId = B.empId
WHERE B.bonus < 1000 OR B.bonus IS NULL;
```

## Question 2:
```Table: Customer
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+
id is the primary key column for this table.
Each row of this table indicates the id of a customer, their name, and the id of the customer who referred them.
 
Write an SQL query to report the names of the customer that are not referred by the customer with id = 2.
Return the result table in any order.
The query result format is in the following example.
 
Example 1:
Input: 
Customer table:
+----+------+------------+
| id | name | referee_id |
+----+------+------------+
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |
+----+------+------------+
Output: 
+------+
| name |
+------+
| Will |
| Jane |
| Bill |
| Zack |
+------+
```

## Solution 2:
```
SELECT name FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```

## Question 3:
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student     | varchar |
| class       | varchar |
+-------------+---------+
(student, class) is the primary key column for this table.
Each row of this table indicates the name of a student and the class in which they are enrolled.
 
 
Write an SQL query to report all the classes that have at least five students.
Return the result table in any order.
The query result format is in the following example.
 
Example 1:
Input: 
Courses table:
+---------+----------+
| student | class    |
+---------+----------+
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |
+---------+----------+
Output: 
+---------+
| class   |
+---------+
| Math    |
+---------+
Explanation: 
- Math has 6 students, so we include it.
- English has 1 student, so we do not include it.
- Biology has 1 student, so we do not include it.
- Computer has 1 student, so we do not include it.
```
 
## Solution 3:
```
SELECT class FROM Courses
GROUP BY class
HAVING  COUNT(class) >= 5;
```

## Question 4:
```Table: SalesPerson
+-----------------+---------+
| Column Name     | Type    |
+-----------------+---------+
| sales_id        | int     |
| name            | varchar |
| salary          | int     |
| commission_rate | int     |
| hire_date       | date    |
+-----------------+---------+
sales_id is the primary key column for this table.
Each row of this table indicates the name and the ID of a salesperson alongside their salary, commission rate, and hire date.
 
 
Table: Company
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| com_id      | int     |
| name        | varchar |
| city        | varchar |
+-------------+---------+
com_id is the primary key column for this table.
Each row of this table indicates the name and the ID of a company and the city in which the company is located.
 
 
Table: Orders
+-------------+------+
| Column Name | Type |
+-------------+------+
| order_id    | int  |
| order_date  | date |
| com_id      | int  |
| sales_id    | int  |
| amount      | int  |
+-------------+------+
order_id is the primary key column for this table.
com_id is a foreign key to com_id from the Company table.
sales_id is a foreign key to sales_id from the SalesPerson table.
Each row of this table contains information about one order. This includes the ID of the company, the ID of the salesperson, the date of the order, and the amount paid.
 
 
Write an SQL query to report the names of all the salespersons who did not have any orders related to the company with the name "RED".
Return the result table in any order.
The query result format is in the following example.
 
Example 1:
Input: 
SalesPerson table:
+----------+------+--------+-----------------+------------+
| sales_id | name | salary | commission_rate | hire_date  |
+----------+------+--------+-----------------+------------+
| 1        | John | 100000 | 6               | 4/1/2006   |
| 2        | Amy  | 12000  | 5               | 5/1/2010   |
| 3        | Mark | 65000  | 12              | 12/25/2008 |
| 4        | Pam  | 25000  | 25              | 1/1/2005   |
| 5        | Alex | 5000   | 10              | 2/3/2007   |
+----------+------+--------+-----------------+------------+
Company table:
+--------+--------+----------+
| com_id | name   | city     |
+--------+--------+----------+
| 1      | RED    | Boston   |
| 2      | ORANGE | New York |
| 3      | YELLOW | Boston   |
| 4      | GREEN  | Austin   |
+--------+--------+----------+
Orders table:
+----------+------------+--------+----------+--------+
| order_id | order_date | com_id | sales_id | amount |
+----------+------------+--------+----------+--------+
| 1        | 1/1/2014   | 3      | 4        | 10000  |
| 2        | 2/1/2014   | 4      | 5        | 5000   |
| 3        | 3/1/2014   | 1      | 1        | 50000  |
| 4        | 4/1/2014   | 1      | 4        | 25000  |
+----------+------------+--------+----------+--------+
Output: 
+------+
| name |
+------+
| Amy  |
| Mark |
| Alex |
```

## Solution 4:
```
SELECT s.name
FROM SalesPerson s
WHERE NOT EXISTS (
  SELECT 1
  FROM Orders o
  JOIN Company c ON c.com_id = o.com_id
  WHERE o.sales_id = s.sales_id
    AND c.name = 'RED'
);
```

## Question 5:
```
Table: Orders

+----------+-------------+--------+
| order_id | customer_id | amount |
+----------+-------------+--------+
| 1        | 101         | 500    |
| 2        | 102         | 300    |
| 3        | 101         | 200    |
| 4        | 103         | 400    |
+----------+-------------+--------+

Question:
Write an SQL query to return the total order amount for each customer.
```

## Solution 5:
```
SELECT customer_id, SUM(amount)
FROM Orders
GROUP BY customer_id;
```

## Question 6:
```
Table: Orders

+----------+-------------+------------+
| order_id | customer_id | order_date |
+----------+-------------+------------+
| 1        | 101         | 2023-01-01 |
| 2        | 102         | 2023-01-02 |
| 3        | 101         | 2023-01-03 |
| 4        | 103         | 2023-01-04 |
| 5        | 101         | 2023-01-05 |
+----------+-------------+------------+

Question:
Write an SQL query to return the customer_id values for customers who placed more than one order.
```

## Solution 6:
```
SELECT customer_id
FROM Orders
GROUP BY customer_id
HAVING COUNT(customer_id) > 1
```
