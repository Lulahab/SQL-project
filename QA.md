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

2.--standardize data;find issues in the data and fix it:inconsistent data formats,capitalization,

--query for handling captialztion

select

lower(country) as country, 
lower(city) as city, trim(both from  (productsku)) as productsku,
trim(both from INITCAP (v2productname)) as productname,
trim(both from INITCAP (v2productcategory)) as v2productcategory,
trim(both from (productvariant)) as productvariant,
UPPER(currencycode) as currencycode
from all_sessions

--identifying inconsistent data types and handling them

--cast date from integer to date

query

SELECT to_date(date ::text,'yyyymmdd') AS date_new FROM all_sessions

UPDATE all_sessions SET date = to_date(date::text, 'YYYYMMDD');



Describe your QA process and include the SQL queries used to execute it.
