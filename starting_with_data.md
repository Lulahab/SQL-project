Question 1: 

-What are the total monthly sales for the year 2017.

SQL Queries:

with tot_sales as (
  select fullvisitorid,
        (unit_price/1000000) as unit_price, 
      coalesce(units_sold,0),
	  (units_sold*(unit_price/1000000))as sales
	  from analytics
	  where units_sold > 0 and
	    unit_price is not null )
select
    sum(sales) as monthly_sales,
	extract(month from to_date(date::text,'yyyymmdd')) as month,
	extract(year from to_date(date::text,'yyyymmdd'))as year
from  tot_sales
join all_sessions
using (fullvisitorid)
where extract(year from to_date(date::text,'yyyymmdd')) = '2017'
group by extract(month from to_date(date::text,'yyyymmdd')) ,
          extract(year from to_date(date::text,'yyyymmdd'))
order by month 




Answer: 



Question 2:  What are the total sales by product type.

SQL Queries:

with total_sales as
    (select v2productname,
        productquantity, 
       (productprice/1000000)as productprice,
	   productquantity * (productprice/1000000)as productsales
 from all_sessions
 where productquantity >= 0 and
      productprice > 0
	  )

select v2productname as productname, 
       sum(productsales) as product_sales
from total_sales
group by  v2productname
order by product_sales desc
	  



Answer:



Question 3:  which channelgrouping generates the highest revenue and how many products are ordered within
   -- each channel.



with total_revenue as ( 
    select  fullvisitorid,
	(unit_price/1000000 * coalesce(units_sold,0))as sales
	  from analytics
	  where unit_price > 0 
	  order by sales desc
)

select channelgrouping, 
       sum(units_sold) as product_count,
      sum(sales) as total_sales
	   from analytics
	   join total_revenue using (fullvisitorid)
	   where channelgrouping not in ('(Other)')
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
