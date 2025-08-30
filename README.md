# Walmart-SQL
This project analyzes Walmart sales data to answer various business-related questions. SQL queries are used to extract insights regarding payment method, revenue generation, customer preferences, and profit generation


<br />



Q1.What are the different payment methods, and how many transactions and items were sold with each method?

```sql
SELECT 
	payment_method,
	COUNT(*) as no_payments,
	SUM(quantity) as no_qty_sold
FROM walmart
GROUP BY payment_method
```

<br />

<img width="396" height="122" alt="image" src="https://github.com/user-attachments/assets/0a700029-ddf1-450e-aab4-990e0e4f485b" />

<br />

<br />

Q2 Identify the Highest-Rated Category in Each Branch. Displaying branch, category and average rating

```sql
SELECT 
	branch,
	category,
	AVG(rating) as avg_rating,
	RANK() OVER(PARTITION BY branch ORDER BY AVG(rating) DESC) as rank
FROM walmart
GROUP BY branch,category 
```


<br />

<img width="460" height="542" alt="image" src="https://github.com/user-attachments/assets/2d67ca04-e5cb-4c00-9e1b-825c4f54317f" />


```SELECT * 
FROM 
(  SELECT 
	branch,
	category,
	AVG(rating) as avg_rating,
	RANK() OVER(PARTITION BY branch ORDER BY AVG(rating) DESC) as rank
FROM walmart
GROUP BY branch,category 

)

WHERE rank =1

```
<img width="458" height="322" alt="image" src="https://github.com/user-attachments/assets/e80b1937-3bc4-40e5-85cb-6ff909fa3bfb" />





