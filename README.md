**Problem 1 -** [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)
```
SELECT product_id FROM Products
WHERE low_fats = 'Y'
AND recyclable = 'Y'
```

**Problem 2 -** [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/)
```
SELECT name
FROM Customer
WHERE referee_id != 2
OR referee_id IS NULL
```

**Problem 3 -** [595. Big Countries](https://leetcode.com/problems/big-countries/)
```
SELECT name, population, area
FROM World
WHERE area >= 3000000
OR population >= 25000000
```
