# Walmart-SQL
This project analyzes Walmart sales data to answer various business-related questions. SQL queries are used to extract insights regarding payment method, revenue generation, customer preferences, and profit generation





```SELECT 
	payment_method,
	COUNT(*) as no_payments,
	SUM(quantity) as no_qty_sold
FROM walmart
GROUP BY payment_method

```pip install pandas numpy matplotlib seaborn plotly```
