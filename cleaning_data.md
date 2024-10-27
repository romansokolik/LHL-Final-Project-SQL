What issues will you address by cleaning the data?

The issues:
- Duplicated rows in the tables
- Figure out the relationships between the tables
- Find the right data types for columns with the values provided in CSV

Queries:
Below, provide the SQL queries you used to clean your data.
```sql
WITH 
cte_analytics AS (
	SELECT DISTINCT
		fullvisitorid AS vid,
		units_sold::int,
		ROUND(revenue::DECIMAL/1000000, 2) AS revenue
	FROM analytics
	WHERE units_sold::int > 0
	AND revenue::DECIMAL > 0
),

cte_sessions AS (
	SELECT DISTINCT
		fullvisitorid AS vid,
		country,
		city, 
		productsku AS sku,
		productquantity::INT AS qty
	FROM all_sessions
	WHERE city NOT IN (
		'not available in demo dataset', 
		'(not set)'
	)
	AND productquantity::INT > 0
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
