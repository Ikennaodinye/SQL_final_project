What are your risk areas? Identify and describe them.
<br>#1 This project being my first data analytics project posed a great challenge for me and trying to do it alone was enough risk. 
Therefore, I found it most difficult reaching out to mentors because I don't even know the right questions to ask. 
However, I believe I have learned alot

<br>#2 The datasets have duplicates values that if not identified and removed would skew the whole analytical process

<br>#3 Some columns have Null values that might affect other values and columns if removed entirely



QA Process:
Describe your QA process and include the SQL queries used to execute it.
# Uniqueness: ensured all duplicates were removed

#Checking duplicates in analytics table

#identifying distinct values
<br>SELECT COUNT (DISTINCT visitid) 
FROM analytics;

#Identifying the total count
<br>SELECT COUNT (visitid) 
FROM analytics;

#Identifying duplicates
<br>SELECT visitid FROM analytics
WHERE visitid
IN (SELECT visitid
   FROM analytics
   GROUP BY visitid
   HAVING COUNT(*)> 1);
   
#Deleting duplicates
<br>DELETE visitid FROM analytics
WHERE visitid
IN (SELECT visitid
   FROM analytics
   GROUP BY visitid
   HAVING COUNT(*)> 1);   
   
#Checking for duplicates in allsessions

#identifying distinct values
<br>SELECT COUNT (DISTINCT fullvisitorid) 
FROM allsessions;

#Identifying the total count
<br>SELECT COUNT (fullvisitorid) 
FROM allsessions;

#Identifying duplicates
<br>SELECT fullvisitorid FROM allsessions
WHERE fullvisitorid
IN (SELECT fullvisitorid
   FROM allsessions
   GROUP BY fullvisitorid
   HAVING COUNT(*)> 1);
   
#Deleting duplicates
<br>DELETE fullvisitorid FROM allsessions
WHERE fullvisitorid
IN (SELECT fullvisitorid
   FROM allsessions
   GROUP BY fullvisitorid
   HAVING COUNT(*)> 1);   



#Consistency:  ensured numerical type data are in the same decimal and date and time are in correct formats 

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






#Completeness : Fill up Null values where necessary and excluded them if need be

#Changing inconsistent value to NULL
<br>UPDATE allsessions
SET city = ''
WHERE city = 'not available in demo dataset';

