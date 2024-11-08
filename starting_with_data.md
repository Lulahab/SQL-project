Question 1: 

-What are the total monthly sales for the year 2017.

SQL Queries:
-- CTE (Common Table Expression) to calculate sales per unit
```sql
with tot_sales as (
  select fullvisitorid,
        (unit_price/1000000) as unit_price, --Converting unit price to a more readable format (dividing by 1,000,000)
      coalesce(units_sold,0), --Handling null values in units_sold, replacing them with 0
	  (units_sold*(unit_price/1000000))as sales --Calculating total sales (units sold * unit price)
	  from analytics
	  where units_sold > 0 and --Filter to only include records where units were sold (positive sales)
	    unit_price is not null )
-- Main query to calculate monthly sales
select
    sum(sales) as monthly_sales, -- Summing total sales for each month
	extract(month from to_date(date::text,'yyyymmdd')) as month, ---- Extracting the month from the date field
	extract(year from to_date(date::text,'yyyymmdd'))as year
from  tot_sales -- refering to the cte above
join all_sessions --Joining the sessions table on the visitor ID to bring in the session data
using (fullvisitorid)
where extract(year from to_date(date::text,'yyyymmdd')) = '2017' --Filter to only include records from 2017

group by extract(month from to_date(date::text,'yyyymmdd')) ,
          extract(year from to_date(date::text,'yyyymmdd'))
order by month 

```


Answer: 



Question 2:  What are the total sales by product type.

SQL Queries:
```sql
with total_sales as
    (select v2productname,-- Product name for identification
        productquantity, -- Quantity of each product sold
       (productprice/1000000)as productprice,
	   productquantity * (productprice/1000000)as productsales
 from all_sessions
 where productquantity >= 0 and
      productprice > 0
	  )
---- Main query to aggregate total sales per product

select v2productname as productname, 
       sum(productsales) as product_sales  -- Summing up total sales for each product
from total_sales -- Using the calculated sales data from the CTE
group by  v2productname -- Grouping the data by product name to get total sales per product
order by product_sales desc
```	  



Answer:



Question 3:  which channelgrouping generates the highest revenue and how many products are ordered within
   -- each channel.


```sql
-- CTE to calculate the total revenue (sales) for each visitor

with total_revenue as ( 
    select  fullvisitorid,  -- Unique identifier for each visitor
	(unit_price/1000000 * coalesce(units_sold,0))as sales
	  from analytics
	  where unit_price > 0 
	  order by sales desc
)
-- Main query to aggregate total sales and product count by channel grouping
select channelgrouping, -- The channel (e.g., direct, organic, etc.) through which the product was sold
       sum(units_sold) as product_count,-- Total units sold for each channel
      sum(sales) as total_sales -- Total revenue (sales) for each channel
	   from analytics
	   join total_revenue using (fullvisitorid) -- Joining with the CTE to get the total sales (revenue) for each visitor
	   where channelgrouping not in ('(Other)') -- Excluding the '(Other)' channel grouping (presumably invalid or unspecified data)
	   group by channelgrouping
	   order by product_count desc
       


SQL Queries:

Answer:



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
