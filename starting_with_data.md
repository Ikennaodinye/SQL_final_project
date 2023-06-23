Question 1: Total sales of each product

SQL Queries:
<br>SELECT p.name, SUM (a.productprice * p.orderedquantity) AS total_sales
FROM products p
JOIN allsessions a
ON p.sku = a.productsku
WHERE (a.productprice * p.orderedquantity) > 0
GROUP BY p.name
ORDER BY total_sales DESC;

Answer: Query returned 17 products with their total sales
<br>"name"	"total_sales"
" 22 oz Water Bottle"	30225
" Sunglasses"	28708
" Men's Vintage Badge Tee Sage"	2123
"Gift Card - $25.00"	1625
" Men's 100% Cotton Short Sleeve Hero Tee Black"	1260
" Toddler 1/4 Zip Fleece Pewter"	988
" Men's Bike Short Sleeve Tee Charcoal"	656
" Infant Short Sleeve Tee Green"	646
" Men's Vintage Badge Tee White"	605
" Women's Short Sleeve Performance Tee Charcoal"	550
" Men's Short Sleeve Hero Tee Charcoal"	323
" Womens 3/4 Sleeve Baseball Raglan Heather/Black"	322
" Infant Short Sleeve Tee Royal Blue"	289
" Men's Airflow 1/4 Zip Pullover Lapis"	105
" Men's Short Sleeve Badge Tee Charcoal"	95
" Men's Colorblock Tee White/Heather"	50
"Android Men's Pep Rally Short Sleeve Tee Navy"	20



Question 2: 
<br>Average order value of each product

SQL Queries:
<br>SELECT p.name, AVG (a.productprice * p.orderedquantity) AS avg_sales
FROM products p
JOIN allsessions a
ON p.sku = a.productsku
WHERE (a.productprice * p.orderedquantity) > 0
GROUP BY p.name
ORDER BY avg_sales DESC;

Answer:
<br>Query returned same number of rows as question 1 but with their average sales values



Question 3: The most expensive product

SQL Queries:
<br>SELECT v2productname, productprice
FROM allsessions
WHERE productprice = (SELECT MAX(productprice)FROM allsessions)

Answer:
<br>"Google Women's Zip Hoodie Grey"	140



Question 4: The least expensive product

SQL Queries:
<br>SELECT v2productname, productprice
FROM allsessions
WHERE productprice > 0
ORDER BY productprice 
LIMIT 1

Answer:
<br>"v2productname"	"productprice"
"Google Sunglasses"	3




Question 5:Average order value for each month in 2017 

SQL Queries:
<br>SELECT EXTRACT (MONTH FROM date)AS orderdate, 
AVG (a.productprice * p.orderedquantity) AS avg_sales
FROM allsessions a
JOIN products p 
ON a.productsku = p.sku
WHERE EXTRACT (YEAR FROM date) = 2017
AND orderedquantity > 0
GROUP BY EXTRACT (MONTH FROM date)
ORDER BY avg_sales DESC;

Answer:
<br>The month of July, 2017 has the most Ave. order value
6	30225.000000000000
1	22022.000000000000
5	1389.5000000000000000
7	395.5000000000000000
4	323.0000000000000000
2	20.0000000000000000
