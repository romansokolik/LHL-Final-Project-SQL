Question 1: 

Identify Primary Key in products table.

SQL Queries:
```sql
SELECT count(sku) FROM products
GROUP BY sku
HAVING count(sku) > 1
```
Answer:

In the table product.sku has unique values across the whole table

Question 2: 
Are there any duplicate rows in the tables?
SQL Queries:
```sql
SELECT * FROM all_sessions
EXCEPT ALL
SELECT DISTINCT * FROM all_sessions;
SELECT * FROM analytics
EXCEPT ALL 
SELECT DISTINCT * FROM analytics;
SELECT * FROM products
EXCEPT ALL
SELECT DISTINCT * FROM products;
SELECT * FROM sales_report
EXCEPT ALL
SELECT DISTINCT * FROM sales_report;
SELECT * FROM sales_by_sku
EXCEPT ALL
SELECT DISTINCT * FROM sales_by_sku;
```
Answer:

Table analytics has **2561814** duplicated rows out of 4301122

Question 3: 

Is the table sales_by_sku representing the sales_report table?

SQL Queries:
```sql
SELECT productSku, total_ordered FROM sales_by_sku
EXCEPT ALL
SELECT productSku, total_ordered FROM sales_report;
```
Answer:

Table sales_by_sku has 8 more products recorded then sales_report.
Sales report data on 8 products has not been provided.

Question 4: 

What are the unique product viewed by each visitor?

SQL Queries:
```sql
SELECT DISTINCT
	fullVisitorId, 
	productSku, 
	p.Name,
	COUNT(*) OVER(PARTITION BY fullVisitorId, productSku) 
	AS unique_product_per_visitor
FROM all_sessions
JOIN products p ON p.sku = productSku
ORDER BY unique_product_per_visitor DESC
```
Answer:

The maximum unique products visited per visitor is **2**.  

Question 5:

What is the total number of unique visitors by referring sites?

SQL Queries:
```sql
SELECT COUNT(*) FROM (
	SELECT DISTINCT fullVisitorId, visitId
	FROM all_sessions
)
```
Answer:

The total number of unique visitors by referring sites is **14561**
