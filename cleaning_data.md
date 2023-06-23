What issues will you address by cleaning the data?
Answer:
<br>The issues to address were:

Identifying duplicate values and removing them
Identifying NULL values and replace them with actual values where possible and changing inconsistent data to NULL
Changing the data types to aid further analysis e.g date and time cloumns to date and time formarts
Dividing numerical data by 1000000 for easy manipulation






Queries:

#cleaning the allsessions table
#Retrieved all data
<br>SELECT *
FROM allsessions

<br>SELECT fullvisitorid
FROM allsessions
#Query returned 15134

<br>SELECT DISTINCT fullvisitorid
FROM allsessions
#Query returned 14223
<br>SELECT visitid
FROM allsessions

<br>SELECT DISTINCT visitid
FROM allsessions

<br>SELECT productsku
FROM allsessions

<br>SELECT DISTINCT productsku
FROM allsessions
#Above queries show there are duplicate values

<br>DELETE FROM allsessions
WHERE (visitid, fullvisitorid, productsku)
IN (SELECT visitid, fullvisitorid, productsku
   FROM allsessions
   GROUP BY visitid, fullvisitorid, productsku
   HAVING COUNT(*)> 1);
   
<br>DELETE FROM allsessions
WHERE visitid
IN (SELECT visitid
   FROM allsessions
   GROUP BY visitid
   HAVING COUNT(*)> 1);
   
<br>DELETE FROM allsessions
WHERE fullvisitorid
IN (SELECT fullvisitorid
   FROM allsessions
   GROUP BY fullvisitorid
   HAVING COUNT(*)> 1);

<br>DELETE FROM allsessions
WHERE productsku
IN (SELECT productsku
   FROM allsessions
   GROUP BY productsku
   HAVING COUNT(*)> 1);
   
#create a new column for time and make it into a time format
<br>SELECT  CAST (time AS integer), '00:00:00' +(interval '1 second' * time::integer) 
FROM allsessions;

<br>ALTER TABLE allsessions ADD COLUMN time_formatted TIME;
UPDATE allsessions SET time_formatted = time '00:00:00' + (interval '1 second' * time::integer);
ALTER TABLE allsessions DROP COLUMN "time";
ALTER TABLE allsessions
RENAME COLUMN time_formatted TO time;  

#Creating a new column for timeonsite

<br>SELECT  CAST (timeonsite AS integer), '00:00:00' +(interval '1 second' * timeonsite::integer) 
FROM allsessions;	
ALTER TABLE allsessions ADD COLUMN timeonsite_formatted TIME;
UPDATE allsessions SET timeonsite_formatted = time '00:00:00' + (interval '1 second' * timeonsite::integer);
ALTER TABLE allsessions DROP COLUMN "timeonsite";
ALTER TABLE allsessions
RENAME COLUMN timeonsite_formatted TO timeonsite;		

#productprice convertion

<br>SELECT productprice
FROM allsessions;

<br>SELECT CAST(productprice/1000000.0 AS DECIMAL(10, 2)) AS productprice, productprice
FROM allsessions;

#Creating a new column for product_price

<br>ALTER TABLE allsessions ADD COLUMN product_price integer;

<br>UPDATE allsessions SET product_price = CAST(productprice/1000000.0 AS DECIMAL(10, 2));   

#Dropping column productprice
<br>ALTER TABLE allsessions DROP COLUMN "productprice";
ALTER TABLE allsessions
RENAME COLUMN product_price TO productprice;

#Changing inconsistent value to NULL
<br>UPDATE allsessions
SET city = ''
WHERE city = 'not available in demo dataset';


#Cleaning Analytics table
#Retrieving data
<br>SELECT *
FROM analytics;

#SELECT visitid FROM analytics and SELECT DISTINCT visitid FROM analytics returned diffnumber of rows indicating duplicates

#To remove duplicates
<br>DELETE FROM analytics
WHERE (visitid, fullvisitorid)
IN (SELECT visitid, fullvisitorid
   FROM analytics
   GROUP BY visitid, fullvisitorid
   HAVING COUNT(*)> 1);
   
<br>DELETE FROM analytics
WHERE visitid
IN (SELECT visitid
   FROM analytics
   GROUP BY visitid
   HAVING COUNT(*)> 1);

<br>DELETE FROM analytics
WHERE fullvisitorid
IN (SELECT fullvisitorid
   FROM analytics
   GROUP BY fullvisitorid
   HAVING COUNT(*)> 1);
   
#Filling up the Null values   
<br>UPDATE analytics
SET 
units_sold = COALESCE(units_sold, 0),
revenue = COALESCE (revenue, 0)
WHERE units_sold IS NULL OR revenue IS NULL;


#Formatting the timeonsite
<br>SELECT  CAST (timeonsite AS integer), '00:00:00' +(interval '1 second' * timeonsite::integer) 
FROM analytics;

<br>UPDATE analytics
SET timeonsite =  time '00:00:00' +(interval '1 second' * timeonsite::integer);

#Formatting visitstarttime
<br>SELECT TO_TIMESTAMP(visitstarttime)as visitstart_time
FROM analytics;
 
<br>ALTER TABLE analytics ADD COLUMN visitstart_time TIMESTAMP;

<br>UPDATE analytics SET visitstart_time = timestamp 'epoch' + (interval '1 second' * visitstarttime::integer);
ALTER TABLE analytics DROP COLUMN "visitstarttime";
ALTER TABLE analytics
RENAME COLUMN visitstart_time TO visitstarttime;

#Cleaning products table

#Retrieved complete table

<br>SELECT *
FROM products;

#Checking for Duplicates

<br>SELECT sku
FROM products

<br>SELECT DISTINCT sku
FROM products;
#above queries returned same result; no duplicates

<br>SELECT *
FROM products
WHERE name IS NULL;
#Use above to check NULL values and none was returned

#Cleaning sales_by_sku
#Retrieved complete table

<br>SELECT *
FROM sales_by_sku;

<br>SELECT productsku
FROM sales_by_sku;

<br>SELECT productsku
FROM sales_by_sku;

#above queries returned zero duplicates and zero null value 


#Cleaning sales_report
#Retrieved complete table

<br>SELECT productsku, name
FROM sales_report;

<br>SELECT DISTINCT productsku, name
FROM sales_report;

#Above queries returned no duplicate values

<br>SELECT *
FROM sales_report
WHERE ratio IS NULL;
#Above query returned 78 NULL values

<br>UPDATE sales_report
SET 
ratio = COALESCE(ratio, 0)
WHERE ratio IS NULL;
