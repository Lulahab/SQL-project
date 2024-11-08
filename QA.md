What are your risk areas? Identify and describe them.
--duplicate
-- null values
--incorrect data eg negative
--inconsistent spelling
--data formats,
--capitalization,
-- data redunduncy



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

2.

--standardize data;find issues in the data and fix it:inconsistent data formats,capitalization,

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

3.

identifying and removing duplicates

SELECT DISTINCT* --1739308 FROM analytics

SELECT* -- 4301122 FROM analytics

the unique row number is < the orginal rows this means--- we have duplicates

--handling duplicates by creating a veiw table and assigning rows and then delete the rows>1

CREATE VIEW analytics_unique AS WITH DuplicateCheck AS ( SELECT *, ROW_NUMBER() OVER (PARTITION BY visitNumber, visitId, visitStartTime, date, fullvisitorId, userid, channelGrouping, socialEngagementType, units_sold,pageviews, timeonsite, bounces, revenue, unit_price ORDER BY fullvisitorId

) AS row_num
FROM analytics
) SELECT * FROM DuplicateCheck WHERE row_num =1;

-------After the CTE computes the row numbers, the main query selects only the rows where row_num = 1, effectively keeping the first occurrence of each unique combination of values in the columns specified in the PARTITION BY clause.


4.--check for incorrect data

select * from analytics where units_sold < 0--- 1 row units_sold = -89 --we can remove it permanently update analytics drop units_sold where units_sold is < 0
Describe your QA process and include the SQL queries used to execute it.
