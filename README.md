# My solutions for Leetcode's SQL 50 study plan challenges
[You can find all the challenges here](https://leetcode.com/studyplan/top-sql-50/)

**Problem 1 -** [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)
```sql
SELECT product_id FROM Products
WHERE low_fats = 'Y'
AND recyclable = 'Y'
```

**Problem 2 -** [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/)
```sql
SELECT name
FROM Customer
WHERE referee_id != 2
OR referee_id IS NULL
```

**Problem 3 -** [595. Big Countries](https://leetcode.com/problems/big-countries/)
```sql
SELECT name, population, area
FROM World
WHERE area >= 3000000
OR population >= 25000000
```

**Problem 4 -** [1148. Article Views I](https://leetcode.com/problems/article-views-i/)
```sql
SELECT name
FROM Customer
WHERE referee_id != 2
OR referee_id IS NULL
```

**Problem 5 -** [1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)
```sql
SELECT tweet_id
FROM Tweets
WHERE CHAR_LENGTH(content) > 15
```

**Problem 6 -** [1378. Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)
```sql
SELECT unique_id, name
FROM Employees
LEFT OUTER JOIN EmployeeUNI
    ON Employees.id = EmployeeUNI.id
```

**Problem 7 -** [1068. Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/)
```sql
SELECT product_name, year, price
FROM Sales AS S
    LEFT JOIN Product AS P
    ON S.product_id = P.product_id
GROUP BY sale_id, product_name, year, price
```

**Problem 8 -** [1581. Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/)
```sql
SELECT DISTINCT customer_id, COUNT(V.visit_id) as count_no_trans
FROM Visits as V
    LEFT JOIN Transactions as T
    ON V.visit_id = T.visit_id
WHERE T.transaction_id IS NULL
GROUP BY customer_id
```

**Problem 9 -** [197. Rising Temperature](https://leetcode.com/problems/rising-temperature/)
```sql
SELECT W.id
FROM Weather AS W
CROSS JOIN Weather AS W2
WHERE W.recorddate - W2.recorddate = 1
    AND W.temperature > W2.temperature
```

**Problem 10 -** [1661. Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/)
```sql
SELECT start.machine_id,
   ROUND(SUM(end.timestamp - start.timestamp) / COUNT(start.process_id), 3) AS processing_time
FROM Activity AS start
CROSS JOIN Activity AS end
WHERE start.machine_id = end.machine_id
    AND start.process_id = end.process_id
    AND start.activity_type = "start"
```

**Problem 11 -** [577. Employee Bonus](https://leetcode.com/problems/employee-bonus/)
```sql
SELECT E.name, B.bonus
FROM Employee as E
LEFT JOIN Bonus as B
ON E.empId = B.empId
WHERE B.bonus < 1000
OR B.bonus IS NULL
```

**Problem 12 -** [1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/)
```sql
SELECT Students.student_id, Students.student_name, Subjects.subject_name,
        COUNT(Examinations.student_id) AS attended_exams
FROM Students
CROSS JOIN Subjects
LEFT JOIN Examinations
     ON Students.student_id = Examinations.student_id
    AND Subjects.subject_name = Examinations.subject_name
GROUP BY Students.student_id, Students.student_name, Subjects.subject_name
ORDER BY Students.student_id, Subjects.subject_name
```

**Problem 13 -** [570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/)
```sql
SELECT E.name
FROM Employee AS E
JOIN Employee AS M
    ON E.id = M.managerId
GROUP BY E.id, E.name
HAVING COUNT(M.managerId) >= 5
```

**Problem 14 -** [1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)
```sql
SELECT s.user_id,
    ROUND(AVG(CASE WHEN C.action = 'confirmed' THEN 1 ELSE 0 END), 2) AS confirmation_rate
FROM Signups AS S
LEFT JOIN Confirmations AS C
    ON S.user_id = C.user_id
GROUP BY s.user_id
```

**Problem 15 -** [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)
```sql
SELECT *
FROM Cinema
WHERE description != 'boring'
   AND (id % 2) != 0
GROUP BY 1,2,3,4
ORDER BY rating DESC
```

**Problem 16 -** [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)
```sql
SELECT P.product_id,
        COALESCE(ROUND(SUM((units * price)) / CAST(SUM(units) AS numeric) , 2), 0) AS average_price
FROM Prices AS P
LEFT JOIN UnitsSold AS U
    ON P.product_id = U.product_id
    AND purchase_date BETWEEN start_date AND end_date
GROUP BY 1
```

**Problem 17 -** [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/)
```sql
SELECT *
FROM Cinema
WHERE description != 'boring'
   AND (id % 2) != 0
GROUP BY 1,2,3,4
ORDER BY rating DESC
```

**Problem 18 -** [1075. Project Employees I](https://leetcode.com/problems/project-employees-i/)
```sql
SELECT P.project_id, ROUND(CAST(SUM(E.experience_years)AS numeric) / COUNT(E.employee_id),2) AS average_years
FROM Project AS P
JOIN Employee AS E
    ON P.employee_id = E.employee_id
GROUP BY 1
```

**Problem 19 -** [1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/)
```sql
SELECT R.contest_id, ROUND(100 * COUNT(DISTINCT R.user_id)
/
(SELECT COUNT(U.user_id) FROM Users AS U)::numeric, 2) AS percentage
FROM Register AS R
GROUP BY 1
ORDER BY 2 DESC, 1 ASC
```

**Problem 20 -** [1211. Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/)
```sql
SELECT query_name,
    ROUND(SUM(rating::numeric / position) / COUNT(result),2) AS quality ,
    ROUND((COUNT(rating) FILTER ( WHERE rating < 3 ) * 100.00) / COUNT(rating), 2) AS poor_query_percentage
FROM Queries AS Q
GROUP BY 1
```

**Problem 21 -** [1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/)
```sql
SELECT TO_CHAR(trans_date,'YYYY-MM') AS month, country,
    COUNT(id) AS trans_count,
    SUM(amount) AS trans_total_amount,
    COUNT(id) FILTER(WHERE state = 'approved') AS approved_count,
    COALESCE(SUM(amount) FILTER(WHERE state = 'approved'),0) AS approved_total_amount
FROM Transactions AS T
GROUP BY 1,2
```

**Problem 22 -** [1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/)
```sql
SELECT ROUND((COUNT(delivery_id) FILTER (WHERE order_date = customer_pref_delivery_date) 
       / COUNT(delivery_id)::numeric) * 100,2) AS immediate_percentage
FROM (
    SELECT DISTINCT ON (customer_id) *
    FROM Delivery
    ORDER BY customer_id, order_date
) AS FirstOrders
```
