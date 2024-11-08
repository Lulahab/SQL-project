What are your risk areas? Identify and describe them.



QA Process:

1. identifying null values and handling it

select * 
from all_sessions
where transactions is null --15053
-- there are 15053 raws with nulls 
-- we can check every column using that method

--Query for handling nulls

select
  
        coalesce(totaltransactionrevenue,0) as totaltransactionrevenue,
	coalesce(transactions,0) as transactions,
	coalesce(pageviews,0) as pageviews,
	coalesce(sessionqualitydim,0) as sessionqualitydim ,
	 coalesce(productrefundamount,0) as productrefundamount
from all_sessions



Describe your QA process and include the SQL queries used to execute it.
