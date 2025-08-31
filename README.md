# Walmart-SQL 10K Data

## Overview

This project analyzes Walmart sales data to answer various business-related questions. SQL queries are used to extract insights regarding payment method, revenue generation, customer preferences, and profit generation


<br />




## Business Problems

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


<br />
<br />

Q8 . How many transactions invoices occur in each shift (Morning, Afternoon, Evening) across branches?
--::time → is PostgreSQL’s type casting operator. It converts the column time into the TIME data type (hh:mm:ss format).

<br />

```sql

SELECT 
	branch,
CASE 
		WHEN EXTRACT(HOUR FROM (time :: time)) < 12 THEN 'Morning'
		WHEN EXTRACT(HOUR FROM (time :: time)) BETWEEN 12 AND 17 THEN 'Afternoon'
		ELSE 'Evening'
	END day_time,
	COUNT(*)
	
FROM walmart
GROUP By day_time,branch
ORDER BY 1,3 DESC 

```
<br />
<img width="400" height="600" alt="image" src="https://github.com/user-attachments/assets/4d28b2c3-7a58-4b16-8dc3-79ea64b5b545" />

<br />
<br />

Q9. Which branches experienced the largest decrease in revenue compared to the previous year?
--rdr = last_rev - cr_rev / ls_rev*100
<br />

```sql

-- 2022 sales 

WITH revenue_2022
AS
(
	SELECT 
		branch, 
		SUM(total) as revenue 
	FROM walmart 
	WHERE EXTRACT (YEAR FROM TO_DATE(date,'DD/MM/YY')) = 2022
	GROUP BY branch
), 
-- 2023 sales
revenue_2023
AS
(
	SELECT 
		branch, 
		SUM(total) as revenue 
	FROM walmart 
	WHERE EXTRACT (YEAR FROM TO_DATE(date,'DD/MM/YY')) = 2023
	GROUP BY branch
)

SELECT 
	lasts.branch,
	lasts.revenue as last_year_revenue,
	currents.revenue as cr_year_revenue,
	ROUND((lasts.revenue - currents.revenue)::numeric/lasts.revenue::numeric*100,2) as rev_dec_ratio
	
FROM revenue_2022 as lasts
JOIN 
revenue_2023 as currents
ON lasts.branch = currents.branch

WHERE lasts.revenue > currents.revenue
ORDER BY rev_dec_ratio
Limit 5

```
<br />

<img width="514" height="178" alt="image" src="https://github.com/user-attachments/assets/ba173221-ba37-4313-bd38-734702446fb6" />
<br />

