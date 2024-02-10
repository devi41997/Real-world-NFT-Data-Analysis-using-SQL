# Real-world-NFT-Data-Analysis-using-SQL
Analyzed sales data of Cryptopunks NFT project from January 1st, 2018, to December 31st, 2021, employing SQL queries to derive valuable insights such as transaction volumes, top transactions, average sale prices, buyer and seller behaviors, and estimating daily collection values.
use cryptopunkdata;
show tables from cryptopunkdata;
select * from pricedata;
SELECT COUNT(*) AS total_sales
FROM pricedata;
select name, usd_price, eth_price, event_date from pricedata order by usd_price desc limit 5;
select event_date, usd_price, avg(usd_price) over (order by event_date rows between 49 preceding and current row) as moving_average from pricedata;
select name , avg(usd_price) as average_price from pricedata group by name order by average_price desc;
select dayname(event_date) as day_of_week, count(*) as sales_count, avg(eth_price) as avg_eth_price from pricedata group by day_of_week order by sales_count asc;
select concat(name, 'sold for $', round(usd_price, 3),'to', buyer_address, 'from', seller_address, 'on', event_date) as summary from pricedata;
create view 1919_purchase as select * from pricedata where buyer_address = '0x1919db36ca2fa2e15f9000fd9cdc2edcf863e685';
select round(eth_price, -2) as eth_price_range, count(*) as frequency from pricedata group by eth_price_range;
select name,max(usd_price) as price, 'highest' as status from pricedata group by name union select name, min(usd_price) as price, 'lowest' as status from pricedata group by name order by name, status asc;
select extract(month from event_date) as month, extract(year from event_date) as year, name, count(*) as sales_count, max(usd_price) as max_price from pricedata group by year, month, name order by year, month;
select extract(month from event_date) as month, extract(year from event_date) as year, round(sum(usd_price),2) as total_volume from pricedata group by year, month;
select count(*) as count_of_transations from pricedata where buyer_address = '0x1919db36ca2fa2e15f9000fd9cdc2edcf863e685';
create temporary table temp_avg_price as select event_date, usd_price, avg(usd_price) over (partition by event_date) as daily_avg_price from pricedata;
select event_date, avg(usd_price) as estimated_value from temp_avg_price where usd_price >= 0.1 * daily_avg_price group by event_date;
select buyer_address, sum(usd_price) as total_spend, count(*) as total_transation from pricedata group by buyer_address order by total_spend desc;
