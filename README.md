# SQL-Practice

This repository contains SQL questions and solutions practiced during self-study alongwith few conecpts of SQL commands.
The focus is on joins, subqueries, aggregation, and filtering.

What is the difference between DROP and TRUNCATE statements?
- If a table is dropped, all things associated with the tables are dropped as well. This includes - the relationships defined on the table with other tables, the integrity checks and constraints, access privileges and other grants that the table has.
- To create and use the table again in its original form, all these relations, checks, constraints, privileges and relationships need to be redefined. However, if a table is truncated, none of the above problems exist and the table retains its original structure.



What is the difference between DELETE and TRUNCATE statements?
- The TRUNCATE command is used to delete all the rows from the table and free the space containing the table.
- The DELETE command deletes only the rows from the table based on the condition given in the where clause or deletes all the rows from the table if no condition is specified. But it does not free the space containing the table.

## Quick References:
```
Logical order SQL runs:

1. FROM  
2. WHERE  
3. GROUP BY  
4. HAVING  
5. SELECT  
6. ORDER BY  
7. LIMIT

🧠 Aggregation functions (SUM, COUNT, AVG, MAX, MIN):
	•	Go in SELECT
	•	Go in HAVING
	•	Do NOT go in WHERE
🧠 DISTINCT is written in the SELECT clause
```

## Quick Examples:
```
SELECT *
FROM patient_visits
WHERE cost > 500;

SELECT
    patient_id,
    COUNT(*) AS visit_count
FROM patient_visits
GROUP BY patient_id
HAVING COUNT(*) > 2;

SELECT DISTINCT patient_id
FROM patient_visits;

SELECT
    emp_id,
    dept,
    salary,
    RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS dept_rank
FROM employee_salary;
```


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
## Question 7:
```
Table: Orders

+----------+-------------+------------+--------+
| order_id | customer_id | order_date | amount |
+----------+-------------+------------+--------+
| 1        | 101         | 2023-01-01 | 500    |
| 2        | 102         | 2023-01-02 | 300    |
| 3        | 101         | 2023-01-03 | 200    |
| 4        | 103         | 2023-01-04 | 400    |
| 5        | 101         | 2023-01-05 | 150    |
| 6        | 102         | 2023-01-06 | 100    |
+----------+-------------+------------+--------+

Question:
Write an SQL query to return the customer_id and total_amount
for customers whose total order amount is greater than 500.
```

## Solution 7:
```
SELECT customer_id, SUM(amount)
FROM Orders
GROUP BY customer_id
HAVING SUM(amount) > 500;
```
## Question 8:
```
Find the Second Highest Salary
Table: employees
id	name	salary
1	Alice	90000
2	Bob	85000
3	Charlie	85000
4	David	80000
5	Eva	70000
Requirement:
 Write a SQL query to find the second highest distinct salary.
Expected Output:
second_highest_saly
85000
```

 ## Solution 8:
 ```
SELECT salary
FROM (
SELECT salary,
RANK ( ) OVER (ORDER BY salary DESC) AS salary_rank
FROM employees
) t
WHERE salary_rank = 2;
```
## Question 9:
```
customers
customer_id	customer_name
1	Alice
2	Bob
3	Charlie
orders
order_id	customer_id	amount
1	1	200
2	2	150
Write a SQL query to find customers who have not placed any orders.
Expected Output:
customer_nae
Charlie
```

## Solution 9:
```
SELECT customers.customer_name
FROM customers
LEFT JOIN orders
ON customers.customer_id = orders.customer_id
WHERE orders_id IS NULL;
```
## Question 10:
```
## Patient Visits Sample Data

| patient_id | visit_date | cost |
|------------|------------|------|
| P101 | 2026-01-25 | 400 |
| P101 | 2026-02-05 | 700 |
| P101 | 2025-12-20 | 900 |
| P102 | 2026-02-01 | 1200 |
| P102 | 2026-02-10 | 1500 |
| P103 | 2026-01-15 | 300 |
| P103 | 2026-02-12 | 200 |
| P104 | 2025-11-01 | 500 |

Assume today = 2026-02-15

Find:
- Each patient’s total cost in the last 30 days
-  Only include patients with total cost > 1000
- Sort by total cost descending

```
## Solution 10:
```
SELECT patient_id, SUM(cost) AS total_cost
FROM patient_visits
WHERE visit_date > '2026-1-15'
GROUP BY patient_id
HAVING SUM(cost) > 1000
ORDER BY total_cost DESC;
```
## Question 11:
```
Find Daily Active Users

Table: user_events
user_id | event_date | event_type
1 | 2025-01-01 | login
2 | 2025-01-01 | click
1 | 2025-01-02 | purchase
3 | 2025-01-02 | login

Problem:
Write a SQL query to find the number of distinct active users per day.
```

## Solution 10:
```
SELECT event_date, COUNT(DISTINCT user_id) AS daily_active_users
FROM user_events
GROUP BY event_date
ORDER BY event_date;

Explanation:
COUNT(DISTINCT user_id) counts unique users per day.
```
## Question 12:
```
Find Top Product by Revenue Per Category

Table: product_sales
product_id | category | revenue
101 | Electronics | 500
102 | Electronics | 700
201 | Clothing | 300
202 | Clothing | 450

Problem:
Find the product generating the highest revenue in each category.
```
## Solution 12:
```
SELECT product_id, category, revenue
FROM (SELECT product_id, category, revenue, RANK() OVER (PARTITION BY category ORDER BY revenue DESC) AS rnk
FROM product_sales) ranked_products
WHERE rnk = 1;

Explanation:
RANK() assigns ranking within each category.
```
## Question 13:
```
Find Users Who Purchased but Never Logged In

Table: user_events
user_id | event_type
1 | login
1 | purchase
2 | purchase
3 | login
4 | purchase

Problem:
Find users who made a purchase but never logged in.
```
## Solution 13:
```
SELECT DISTINCT user_id
FROM user_events
WHERE event_type = ‘purchase’
AND user_id NOT IN (
SELECT user_id
FROM user_events
WHERE event_type = ‘login’
);

Explanation:
This selects users who purchased but excludes users who logged in.
```
## Find second highest salary?
```
SELECT MAX(salary) AS second_highest_salary
FROM employees
WHERE salary < (
    SELECT MAX(salary) FROM employees
);

OR,

SELECT salary
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees
) t
WHERE rnk = 2;
```
## Conceptual: ROW NUMBER () vs RANK() vs DENSE_RANK()
```
Example salaries: 100, 90, 90, 80

ROW NUMBER()
	•	100 → 1
	•	90 → 2
	•	90 → 3
	•	80 → 4

RANK()
	•	100 → 1
	•	90 → 2
	•	90 → 2
	•	80 → 4

DENSE_RANK()
	•	100 → 1
	•	90 → 2
	•	90 → 2
	•	80 → 3

Key difference
	•	RANK() skips numbers after ties.
	•	DENSE_RANK() does not skip after ties.
	•	ROW NUMBER() basically counts numbers.
```
## Categorize amount using CASE WHEN
```
SELECT
    amount,
    CASE
        WHEN amount < 100 THEN 'Low'
        WHEN amount BETWEEN 100 AND 500 THEN 'Medium'
        ELSE 'High'
    END AS category
FROM transactions;

Key point:
	•	CASE is used for business logic inside SQL.
```
