# Final-Project-Transforming-and-Analyzing-Data-with-SQL
<br>Project/Goals
The project is an ecommerce site data  with 5 separate data sets/tables, 
namely: all sessions, analytics, sales report, sales by stock  keeping unit and products.

The project goal  is to clean, transform, and analyze the data to identify site visitors pattern and 
purchasing behaviours, draw conclusions and make data driven decisions.


## Process
<br>-Created a database in PostgreSQL named ecommerce
-Used queries create the 5 tables and their columns assigned specific data types
-Cleaned the datasets with following steps: identifying and removing duplicate values; 
identifying and replacing NULL values with relevant data where applicable; Converting date and time columns to date and time formats respectively and dividing the unit price/product price by 1000000 for easy manipulation and representation.
-Adding primary key constraints to the tables to create relationships

## Results

<br>-United States is the country with the highest transaction revenue followed by Canada : 
The table all sessions represents the fact table, having most of the information.  Used query to select the sum of transaction revenue,
cities and countries, from all sessions table where total transaction revenue and city is not null. Group by the country and city 
and order by total transaction revenue in descending order.

<br>- The data revealed the pattern of purchase by the visitors, which shows drinkwares and apparels were mostly orderd by site visitors.
To get this insight, two tables - All sessions (fact table) and Prodcts (dimension table)  were inner joined. Used Where clause to filter
out where product is '0' and city is null. Grouped my findings by country, city and product category and ordered by sum of the order quantity in descending order
## Challenges 
<br>-As this was my first ever data analysis/ SQL project I had a very hard time undestanding what is actually required of me, especially 
How to present my findings. 
-Had difficulty understanding the project context and making sense of the raw data
-Had so many questions to ask but does not know how to put it together for mentors to even help
-Not understanding how to upload my work in Github and wasted alot of time trying to figure it out and yet still struggling

## Future Goals
<br>To do a better job next time: 
-- Think outside the box and use order familiar tools to analyze my data
-- Work on improving my skills in using windows functions
-- Use charts to visualize my results during presentations
-- Put more efforts to understanding how Github works
