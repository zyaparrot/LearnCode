# Recyclable and Low Fat Products

Table: Products
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |
+-------------+---------+
```
product_id is the primary key (column with unique values) for this table.
low_fats is an ENUM (category) of type ('Y', 'N') where 'Y' means this product is low fat and 'N' means it is not.
recyclable is an ENUM (category) of types ('Y', 'N') where 'Y' means this product is recyclable and 'N' means it is not.

 

Write a solution to find the ids of products that are both low fat and recyclable.

Return the result table in any order.

The result format is in the following example.



Example 1:

Input: 
Products table:
```
+-------------+----------+------------+
| product_id  | low_fats | recyclable |
+-------------+----------+------------+
| 0           | Y        | N          |
| 1           | Y        | Y          |
| 2           | N        | Y          |
| 3           | Y        | Y          |
| 4           | N        | N          |
+-------------+----------+------------+
```
Output: 
```
+-------------+
| product_id  |
+-------------+
| 1           |
| 3           |
+-------------+
```
Explanation: Only products 1 and 3 are both low fat and recyclable.

Solution:
```
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```
텍스트를 쓸때 따옴표로 감싸는 것을 헷갈린것 말고는 별로 어려울게 없다.

다만 Oracle 에서는 큰따옴표=컬럼명, 작은따옴표=문자열 이다.

---

<br>
<br>

# Find Customer Referee

Table: Customer
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+
```
In SQL, id is the primary key column for this table.
Each row of this table indicates the id of a customer, their name, and the id of the customer who referred them.

 

Find the names of the customer that are not referred by the customer with id = 2.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Customer table:
```
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
```
Output: 
```
+------+
| name |
+------+
| Will |
| Jane |
| Bill |
| Zack |
+------+
```

Solution:
```SQL
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```

`NULL`의 DataType은 `Unkown` 이거나 `no data` 라는 뜻이므로 별도로 처리해야함.

---

<br>
<br>

# Big Countries

Table: World
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |
+-------------+---------+
```
name is the primary key (column with unique values) for this table.
Each row of this table gives information about the name of a country, the continent to which it belongs, its area, the population, and its GDP value.

 

A country is big if:

    it has an area of at least three million (i.e., 3000000 km2), or
    it has a population of at least twenty-five million (i.e., 25000000).

Write a solution to find the name, population, and area of the big countries.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
World table:
```
+-------------+-----------+---------+------------+--------------+
| name        | continent | area    | population | gdp          |
+-------------+-----------+---------+------------+--------------+
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |
| Andorra     | Europe    | 468     | 78115      | 3712000000   |
| Angola      | Africa    | 1246700 | 20609294   | 100990000000 |
+-------------+-----------+---------+------------+--------------+
```
Output: 
```
+-------------+------------+---------+
| name        | population | area    |
+-------------+------------+---------+
| Afghanistan | 25500100   | 652230  |
| Algeria     | 37100000   | 2381741 |
+-------------+------------+---------+
```

Solution:
```SQL
SELECT name, population, area
FROM World
WHERE population >= 25000000 OR area >= 3000000;
```

At lease = 최소한

숫자가 커지면 구분해 줄 필요가 있다.

  -> `FORMAT(number, decimal_places)`

---

<br>
<br>

# Article Views I
Table: Views
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |
+---------------+---------+
```
There is no primary key (column with unique values) for this table, the table may have duplicate rows.
Each row of this table indicates that some viewer viewed an article (written by some author) on some date. 
Note that equal author_id and viewer_id indicate the same person.

 

Write a solution to find all the authors that viewed at least one of their own articles.

Return the result table sorted by id in ascending order.

The result format is in the following example.

 

Example 1:

Input: 
Views table:
```
+------------+-----------+-----------+------------+
| article_id | author_id | viewer_id | view_date  |
+------------+-----------+-----------+------------+
| 1          | 3         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |
+------------+-----------+-----------+------------+
```
Output: 
```
+------+
| id   |
+------+
| 4    |
| 7    |
+------+
```

Solution:
```SQL
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY id;
```

작성 순서

- `SELECT` - `FROM` - `WHERE` - `GROUP BY` - `HAVING` - `ORDER BY`

실행 순서

- `FROM` - `WHERE` - `GROUP BY` - `HAVING` - `SELECT` - `ORDER BY`

---

<br>
<br>

# Invalid Tweets

Table: Tweets
```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| tweet_id       | int     |
| content        | varchar |
+----------------+---------+
```
tweet_id is the primary key (column with unique values) for this table.
This table contains all the tweets in a social media app.

 

Write a solution to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is strictly greater than 15.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Tweets table:
```
+----------+----------------------------------+
| tweet_id | content                          |
+----------+----------------------------------+
| 1        | Vote for Biden                   |
| 2        | Let us make America great again! |
+----------+----------------------------------+
```
Output: 
```
+----------+
| tweet_id |
+----------+
| 2        |
+----------+
```
Explanation: 
Tweet 1 has length = 14. It is a valid tweet.
Tweet 2 has length = 32. It is an invalid tweet.

Solution:
```SQL
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;
```



---
