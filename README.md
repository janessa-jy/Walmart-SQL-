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

<br />

```sql 
SELECT * 
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
<br />
<img width="458" height="322" alt="image" src="https://github.com/user-attachments/assets/e80b1937-3bc4-40e5-85cb-6ff909fa3bfb" />

<br />
<br />
Q3 What is the busiest day of the week for each branch based on transaction volume?

<br />

```sql

SELECT * 
FROM
(
SELECT 
	branch,
	TO_CHAR(TO_DATE(date,'DD/MM/YY'),'Day') as day_name,
	COUNT(*) as no_transactions,
	RANK() OVER (PARTITION BY branch ORDER BY COUNT(*) DESC ) as rank
FROM walmart 
GROUP BY branch, day_name
)
WHERE rank = 1
```


<br />

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/a21cf02d-54d8-483b-868e-63d08d3d1b74" />

<br />
<br />


Q4 .How many items were sold through each payment method?
<br />

```sql

SELECT 
	payment_method,
	SUM(quantity) as no_qty_sold
FROM walmart
GROUP BY payment_method

```
<br />
<img width="300" height="236" alt="image" src="https://github.com/user-attachments/assets/37f57c4e-ecf1-484c-9f47-679926f905e6" />


<br />
<br />

Q5.  What are the average, minimum, and maximum ratings for each category in each city? 
--List the city, average_rating, min_rating and max_rating 
<br />

```sql
SELECT 
	city,
	category,
	MIN(rating) as min_rating,
	MAX(rating) as max_rating,
	AVG(rating) as avg_rating
FROM walmart
GROUP BY city,category

```
<br />



Q6 What is total profit for each category, ranked from highest to lowest?
-- total profit as  ( unit_price * quantity * profit_margin)

<br />

```sql

SELECT 
	category,
	SUM(total) as total_revenue,
	SUM(total * profit_margin) as profit 
FROM walmart
GROUP BY category 
ORDER BY profit DESC 



```

<br />

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/040e000d-80cd-46fd-b012-f45faa9e4e4a" />
<br />





Q7. What is the most frequently used payment method in each branch?

<br />

```sql

WITH cte 
AS
(SELECT 
	branch,
	payment_method,
	COUNT(*) as total_trans,
	RANK() OVER(PARTITION BY branch ORDER BY COUNT(*) DESC) as rank 
FROM walmart
GROUP BY branch, payment_method
)

SELECT * 
FROM cte 
WHERE rank = 1

```
<br />


<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/1575bd91-88ad-43da-92e7-17b2b5de886b" />



