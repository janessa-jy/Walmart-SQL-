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
	AVG(rating) as avg_rating
FROM walmart
GROUP BY branch,category 
Order by branch , avg_rating DESC
```


<br />

<img width="398" height="516" alt="image" src="https://github.com/user-attachments/assets/9ea9d1eb-e56b-4ca5-a8ca-c38ade4f0680" />




