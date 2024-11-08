What issues will you address by cleaning the data?

-remove duplicates
--standardize data;find issues in the data and fix it:inconsistent data formats,capitalization,
--null or blank value,try to populate or remove if no u

cleaning process:

select*from all_sessions

select distinct channelgrouping
from all_sessions
select productrevenue
from all_sessions
where  productrevenue is not null
create view as
select distinct fullvisitorid , 
       trim(both from INITCAP (channelgrouping)) AS channelgrouping,
	   (time * interval'1 second')::time as time,
	   lower(country) as country, 
	   lower(city) as city,
	   coalesce(totaltransactionrevenue,0) as totaltransactionrevenue,
	    coalesce(transactions,0) as transactions,
		timeonsite,
		coalesce(pageviews,0) as pageviews,
		coalesce(sessionqualitydim,0) as sessionqualitydim ,
		 to_date(date ::text,'yyyymmdd') AS date,
		 visitid,
		 type,
		 coalesce(productrefundamount,0) as productrefundamount,
		 productquantity,
		 productprice/1000000 as productprice,
		 coalesce(productrevenue,0),
		 trim(both from  (productsku)) as productsku,
		 trim(both from INITCAP (v2productname)) as productname,
		 trim(both from INTICAP (v2productcategory)) as v2productcategory,
		 trim(both from (productvariant)) as productvariant,
		 UPPER(currencycode) as currencycode,
		 coalesce(itemquantity,0) as itemquantity,
		 coalesce(itemrevenue,0) as itemrevenue,
		 coalesce(transactionrevenue,0) as transactionrevenue,
		 transactionid,
		 trim(both from (pagetitle)) as pagetitle,
		 searchkeyword,
		 trim(both from (pagepathlevel1)) as pagepathlevel1,
		 ecommerceaction_type,
		 ecommerceaction_step,
		 ecommerceaction_option
from all_sessions




DESCRIBE your QA process and query






