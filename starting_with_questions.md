Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
<br>SELECT country, city, SUM(totaltransactionrevenue::integer) AS totalrevenue
FROM allsessions
WHERE totaltransactionrevenue IS NOT NULL
AND city != ''
GROUP BY country, city
ORDER BY totalrevenue DESC;



Answer: 
<br>"country"	"city"	"totalrevenue"
"United States"	"Chicago"	123940000
"Canada"	"Toronto"	82160000
"United States"	"New York"	51330000
"United States"	"Mountain View"	43810000
"United States"	"Austin"	35780000
"United States"	"Columbus"	21990000





**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
<br>SELECT a.country, a.city, AVG(p.orderedquantity)AS Avgorderedquantity
FROM allsessions a
JOIN products p
ON a.productsku = p.sku
WHERE p.orderedquantity > 0
AND city != ''
GROUP BY a.country, a.city
ORDER BY AVG(p.orderedquantity)DESC



Answer:
<br>"country"	"city"	"avgorderedquantity"
"United States"	"Detroit"	10075.0000000000000000
"United States"	"Chicago"	534.5000000000000000
"United States"	"Mountain View"	117.0000000000000000
"United States"	"Austin"	64.0000000000000000
"United States"	"New York"	26.0000000000000000
"United States"	"San Francisco"	10.0000000000000000
"United States"	"Columbus"	5.0000000000000000





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL queries:
<br>SELECT a.country, a.city, a.v2productcategory, SUM(p.orderedquantity) AS numberoforders
FROM allsessions a
JOIN products p 
ON a.productsku = p.sku
WHERE p.orderedquantity > 0
AND city != ''
GROUP BY a.country, a.city, a.v2productcategory
ORDER BY SUM(p.orderedquantity)DESC



Answer:
<br>"country"	"city"	"v2productcategory"	"numberoforders"
"United States"	"Detroit"	"Drinkware"	10075
"United States"	"Chicago"	"Lifestyle"	1046
"United States"	"Mountain View"	"Apparel"	234
"United States"	"Austin"	"Apparel"	90
"United States"	"Austin"	"Home/Apparel/Kid's/Kid's-Infant/"	38
"United States"	"New York"	"${escCatTitle}"	26
"United States"	"Chicago"	"Apparel"	23
"United States"	"San Francisco"	"Home/Apparel/Women's/Women's-Performance Wear/"	10
"United States"	"Columbus"	"(not set)"	5




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
<br>SELECT a.country, a. city, a.v2productname, SUM(p.orderedquantity) AS totalsold
FROM allsessions a
JOIN products p 
ON a.productsku = p.sku
GROUP BY a.country, a.city, a.v2productname
HAVING SUM(p.orderedquantity) = (SELECT
MAX(sumquantity)
FROM(
SELECT a.country, a.city, a.v2productname, SUM(P.orderedquantity)as sumquantity
FROM allsessions a
JOIN products p ON a.productsku = p.sku
GROUP BY a.country, a.city, a.v2productname)as product)
ORDER BY a.country, a.city;



Answer:
<br>"United States"	"Detroit"	"Google 22 oz Water Bottle"	10075

--Google 22 oz Water Bottle is returned as the top-selling product and 
--it is under the Drinkware productcategory, which was the most ordered.





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

<br>SQL Queries:
SELECT country, city, SUM(totaltransactionrevenue::integer)AS total_revenue
FROM allsessions
WHERE totaltransactionrevenue IS NOT NULL
AND city !=''
GROUP BY country, city
ORDER BY total_revenue DESC;



Answer:
<br>"country"	"city"	"total_revenue"
"United States"	"Chicago"	123940000
"Canada"	"Toronto"	82160000
"United States"	"New York"	51330000
"United States"	"Mountain View"	43810000
"United States"	"Austin"	35780000
"United States"	"Columbus"	21990000







