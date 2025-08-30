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


