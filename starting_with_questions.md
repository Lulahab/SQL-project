Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**
-- Step 1: Select the top 10 cities and countries with the highest transaction revenue
```sql
SELECT
     country,
     city,
    SUM(transactionrevenue) AS total_revenue --Sum the transaction revenue for each city and country
FROM 
FROM all_sessions
WHERE 
    transactionrevenue IS NOT NULL
	and city not like 'not available in demo dataset' -- Exclude irrelevant cities and countries based on dataset notes
	and city not like 'not set'
	and country not like 'not available in demo dataset'
	and country not like '%not set%'
GROUP BY 
     city,
	 country
ORDER BY 
    total_revenue DESC
	limit 10
```
SQL Queries:



Answer:   country	     city	          total_revenue
         United States     Sunnyvale  	          200000000




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL QUERIES:
-- Step 1: Calculate the average number of products ordered per visitor for each city and country
SELECT 
    country,
    city,
   round( AVG(total_products_per_visitor),2) AS avg_products_per_visitor  --Calculate the average of total products ordered per visitor
--    -- Step 2: Calculate the total products ordered per visitor for each city and country  
FROM (

    SELECT 
        country,
        city,
        fullvisitorid,
        SUM(orderedquantity) AS total_products_per_visitor -- Sum the total products ordered by each visitor

    FROM all_sessions a
	join products p
	on a.productsku= p.sku
    WHERE 
        orderedquantity IS NOT NULL
        AND city NOT LIKE 'not available in demo dataset' ---- Exclude irrelevant cities and countries based on dataset notes
        AND city NOT LIKE '%not set%'
        AND country NOT LIKE 'not available in demo dataset'
        AND country NOT LIKE '%not set%'
    GROUP BY 
        country, 
        city, 
       fullvisitorid
) AS visitor_totals
GROUP BY 
    country, 
    city
ORDER BY 
    avg_products_per_visitor DESC;
	

Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
-- Step 1: Calculate the number of product categories per city and country

SELECT 
    country,
	city ,
	count(v2productcategory) as number_products --Counts the number of products in each category for each city and country
       
FROM all_sessions
WHERE 
	city not like 'not available in demo dataset'-- -- Exclude irrelevant cities and countries based on dataset notes
	and city not like '%not set%' 
	and country not like 'not available in demo dataset'
	and country not like 'not set'
group by country,
        city
order by number_products desc





Answer:This query will return the number of products available in each product category for each city and country, sorted by the number of products in descending order.





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

with product_sales as(SELECT 
      v2productname,city,country,
	  sum(units_sold) as total_product_sold
FROM analytics a
JOIN all_sessions al
ON a.fullvisitorid= al.fullvisitorid
WHERE 
    units_sold IS NOT NULL
	and city not like 'not available in demo dataset'
	and city not like '%not set%'
	and country not like 'not available in demo dataset'
	and country not like 'not set'
GROUP BY city,country, v2productname
)
select*
from (


SELECT city, country, v2productname,total_product_sold,
rank() over(partition by city,country order by total_product_sold desc) as rank_sales
FROM product_sales 
) as subq

where rank_sales<=1



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

Step 1: Calculate total revenue per city and countr
with revenue_of_city_country as(
        SELECT city, country,
        SUM(units_sold * a.unit_price/1000000) as total_revenue -- Sum of units sold multiplied by unit price, divided by 1,000,000 for scaling
	   FROM  all_sessions al
	   join  analytics a
	   on al.fullvisitorid = a.fullvisitorid
	  WHERE 
      a.unit_price IS NOT NULL and a.unit_price>0 --- Filter to include only valid unit prices and sold units
	  and units_sold is not null
	  and city not like 'not available in demo dataset' -- Exclude irrelevant cities and countries based on dataset notes
	  and city not like '%not set%'
	  and country not like 'not available in demo dataset'
	  and country not like 'not set'   
	  group by city, country)


-- Step 2: Calculate the revenue percentage for each city/country and rank them by total revenue
--round to round the percentage t0 2 decimal point only
--
SELECT city,country,total_revenue,-- Total revenue calculated in Step 1
    -- Calculate the percentage of total revenue for each city/country
ROUND(total_revenue/(SELECT SUM(total_revenue) FROM revenue_of_city_country)*100,2) as revenue_percentage,
RANK() over(order by total_revenue desc ) as rank_revenue
FROM revenue_of_city_country --Use the result of the CTE from Step 1
order by rank_revenue -- Order the final result by the revenue rank (highest revenue first)



Answer:







