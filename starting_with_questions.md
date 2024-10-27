Answer the following questions and provide the SQL queries used to find the answer.

```sql
WITH 
cte_analytics AS (
	SELECT DISTINCT
		fullvisitorid AS vid,
		units_sold::int,
		revenue::DECIMAL
	FROM analytics
	WHERE units_sold::int > 0
	AND revenue::DECIMAL > 0
),

cte_sessions AS (
	SELECT DISTINCT
		fullvisitorid AS vid,
		country,
		city, 
		productsku AS sku
	FROM all_sessions
	WHERE city NOT IN (
		'not available in demo dataset', 
		'(not set)'
	)
),

cte_sales_by_sku AS (
	SELECT DISTINCT
		productsku AS sku,
		total_ordered::INT
	FROM sales_by_sku
	WHERE total_ordered::INT > 0
),

cte_sales_report AS (
	SELECT DISTINCT
		productsku AS sku,
		total_ordered::INT
	FROM sales_report
	WHERE total_ordered::INT > 0
),

cte_products AS (
	SELECT DISTINCT
		sku, 
		name
	FROM products
)
```    


**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```sql
SELECT DISTINCT 
	city, 
	country, 
	SUM(revenue) AS sum_of_transactions
FROM cte_sessions 
JOIN cte_analytics USING(vid)
GROUP BY city, country
ORDER BY sum_of_transactions DESC
```

Answer:




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```sql
SELECT DISTINCT 
	country,	
	city, 
	ROUND(AVG(units_sold)) AS avg_units_sold
FROM cte_sessions
JOIN cte_analytics USING(vid)
GROUP BY country, city
ORDER BY country, city, avg_units_sold DESC
```


Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







