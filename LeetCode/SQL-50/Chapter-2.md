# Replace Employee ID With The Unique Identifier

Table: Employees
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
```
id is the primary key (column with unique values) for this table.
Each row of this table contains the id and the name of an employee in a company.

 

Table: EmployeeUNI
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
```
(id, unique_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id and the corresponding unique id of an employee in the company.

 

Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Employees table:
```
+----+----------+
| id | name     |
+----+----------+
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
+----+----------+
```
EmployeeUNI table:
```
+----+-----------+
| id | unique_id |
+----+-----------+
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
+----+-----------+
```
Output: 
```
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |
+-----------+----------+
```
Explanation: 
Alice and Bob do not have a unique ID, We will show null instead.
The unique ID of Meir is 2.
The unique ID of Winston is 3.
The unique ID of Jonathan is 1.

Solution:
```
SELECT EmployeeUNI.unique_id, Employees.name
FROM Employees
LEFT JOIN EmployeeUNI
ON Employees.id=EmployeeUNI.id;
```

`LEFT JOIN` = 왼쪽 테이블에 있는 값만 합침

왼쪽 테이블이란 `FROM <테이블>`에 있는 테이블임

`ON <조건문>` 을 사용해서 쪼인 기준을 잡음

`RIGHT JOIN`, `INNER JOIN`, `OUTER JOIN`, `CROSS JOIN`, `SELF JOIN` 등이 있다.

[참고링크 : All the Joins in SQL- Visualized and Simplified](https://spardhax.medium.com/all-the-joins-in-sql-visualised-and-simplified-3df0687a8624)

---

<br>
<br>

# Product Sales Analysis I
Table: Sales
```
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
```
(sale_id, year) is the primary key (combination of columns with unique values) of this table.
product_id is a foreign key (reference column) to Product table.
Each row of this table shows a sale on the product product_id in a certain year.
Note that the price is per unit.

 

Table: Product
```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
```
product_id is the primary key (column with unique values) of this table.
Each row of this table indicates the product name of each product.

 

Write a solution to report the product_name, year, and price for each sale_id in the Sales table.

Return the resulting table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Sales table:
```
+---------+------------+------+----------+-------+
| sale_id | product_id | year | quantity | price |
+---------+------------+------+----------+-------+ 
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |
+---------+------------+------+----------+-------+
```
Product table:
```
+------------+--------------+
| product_id | product_name |
+------------+--------------+
| 100        | Nokia        |
| 200        | Apple        |
| 300        | Samsung      |
+------------+--------------+
```
Output: 
```
+--------------+-------+-------+
| product_name | year  | price |
+--------------+-------+-------+
| Nokia        | 2008  | 5000  |
| Nokia        | 2009  | 5000  |
| Apple        | 2011  | 9000  |
+--------------+-------+-------+
```
Explanation: 
From sale_id = 1, we can conclude that Nokia was sold for 5000 in the year 2008.
From sale_id = 2, we can conclude that Nokia was sold for 5000 in the year 2009.
From sale_id = 7, we can conclude that Apple was sold for 9000 in the year 2011.

Solution:
```SQL
SELECT p.product_name, s.year, s.price
FROM Sales AS s
LEFT JOIN Product AS p
ON s.product_id=p.product_id;
```

`AS` 를 사용하여 Alias를 정할 수 있다

## [SQL Aliasing Convention](https://www.sqlstyle.guide/ko/#aliasing-or-correlations)
- 별칭을 붙이는 개체 또는 표현과 어떤 식으로든 관련있어야 한다.
- 일반적으로 correlation 이름은 개체 이름에 포함된 각 단어의 첫 글자가 되어야 한다.
- 이미 같은 이름의 correlation이 있는 경우, 번호를 붙인다.
- 항상 AS 키워드를 붙인다. — 명시적이므로 가독성이 좋다.
- 계산된 값(SUM() or AVG())이 스키마에 정의된 컬럼일 경우 이름을 부여하라.

---

<br>
<br>

# Customer Who Visited but Did Not Make Any Transactions
Table: Visits
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+
```
visit_id is the column with unique values for this table.
This table contains information about the customers who visited the mall.

 

Table: Transactions
```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
```
transaction_id is column with unique values for this table.
This table contains information about the transactions made during the visit_id.

 

Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.

Return the result table sorted in any order.

The result format is in the following example.

 

Example 1:

Input: 
Visits
```
+----------+-------------+
| visit_id | customer_id |
+----------+-------------+
| 1        | 23          |
| 2        | 9           |
| 4        | 30          |
| 5        | 54          |
| 6        | 96          |
| 7        | 54          |
| 8        | 54          |
+----------+-------------+
```
Transactions
```
+----------------+----------+--------+
| transaction_id | visit_id | amount |
+----------------+----------+--------+
| 2              | 5        | 310    |
| 3              | 5        | 300    |
| 9              | 5        | 200    |
| 12             | 1        | 910    |
| 13             | 2        | 970    |
+----------------+----------+--------+
```
Output: 
```
+-------------+----------------+
| customer_id | count_no_trans |
+-------------+----------------+
| 54          | 2              |
| 30          | 1              |
| 96          | 1              |
+-------------+----------------+
```
Explanation: 
Customer with id = 23 visited the mall once and made one transaction during the visit with id = 12.
Customer with id = 9 visited the mall once and made one transaction during the visit with id = 13.
Customer with id = 30 visited the mall once and did not make any transactions.
Customer with id = 54 visited the mall three times. During 2 visits they did not make any transactions, and during one visit they made 3 transactions.
Customer with id = 96 visited the mall once and did not make any transactions.
As we can see, users with IDs 30 and 96 visited the mall one time without making any transactions. Also, user 54 visited the mall twice and did not make any transactions.

Solution:
```SQL
SELECT v.customer_id, COUNT(*) AS count_no_trans
FROM Visits AS v
LEFT JOIN Transactions AS t
ON v.visit_id=t.visit_id
WHERE transaction_id IS NULL
GROUP BY customer_id;
```

`COUNT(*)` = 모든 ROW
`COUNT(<컬럼명>)` = 컬럼 중 값이 있는 ROW

`NULL`을 세려면 `COUNT(*) - COUNT(<컬럼명>)`을 하거나
위 답처럼 `WHERE <컬럼명> IS NULL`로 불러와서 `COUNT(*)`을 해주면 된다

예시 입력
```SQL
SELECT *
FROM Visits AS v
LEFT JOIN Transactions AS t
ON v.visit_id=t.visit_id
WHERE transaction_id IS NULL;
```
Output:
```SQL
| visit_id | customer_id | transaction_id | visit_id | amount |
| -------- | ----------- | -------------- | -------- | ------ |
| 4        | 30          | null           | null     | null   |
| 6        | 96          | null           | null     | null   |
| 7        | 54          | null           | null     | null   |
| 8        | 54          | null           | null     | null   |
```

---

<br>
<br>

# Rising Temperature
Table: Weather
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
```
id is the column with unique values for this table.
This table contains information about the temperature on a certain day.

 

Write a solution to find all dates' Id with higher temperatures compared to its previous dates (yesterday).

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Weather table:
```
+----+------------+-------------+
| id | recordDate | temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+
```
Output: 
```
+----+
| id |
+----+
| 2  |
| 4  |
+----+
```
Explanation: 
In 2015-01-02, the temperature was higher than the previous day (10 -> 25).
In 2015-01-04, the temperature was higher than the previous day (20 -> 30).

Solution:
```SQL

```



---

<br>
<br>

# 
