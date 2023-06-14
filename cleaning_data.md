What issues will you address by cleaning the data?


--productprice convertion

SELECT productprice
FROM all_sessions;

SELECT CAST(productprice/1000000.0 AS DECIMAL(10, 2)) AS productprice, productprice
FROM all_sessions;



Queries:
Below, provide the SQL queries you used to clean your data.
