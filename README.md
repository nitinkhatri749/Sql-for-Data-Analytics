# Tableau for Analysis
Dashboard can be found here which is made on top of attached data-
## [Dashboard](https://public.tableau.com/views/E-commerce_15899678474640/SalesTrend?:display_count=y&:toolbar=n&:origin=viz_share_link)





# Sql-for-Data-Analytics
SQL, Postgresql, Data Analysis

### Prerequisites - Good knowledge of SQL, Postgresql(or any RDBMS), Basic Analytical thinking


To copy data from tables into database tables(local)-
First create Table and then copy or any tools can be used.

```
Copy Table_name from 'Path of file' delimiter ',' csv header
```
Or if data has to be copied from a text file then (Make sure to remove header in text file before executing below query)-

```
Copy Table_name from 'Path of file' delimiter ','
```
Once all the tables have been copied or directly extracted into ypour Database using any tool, we can look into our data.

There are three tables namely- 
#### 1) customer
#### 2) sales
#### 3) product

Table customer have necessary details about a customer as- customer_id,  customer name, country, state, age, city, region, segment
sales includes - order_line, order_id, order_date, ship_date, ship_mode, customer_id, product_id, sales, quantity, discount, profit
product includes - product_id, category, sub_category, product_name


primary keys - 
customer_id (Table customer)
order_line (sales)
product_id (product)

foreign keys -
customeer_id(Table sales)
product_id(Table sales)

#### Let's start with queries -

1) Total Revenue(sales), average sales, minimum sales, maximum sales geneated till now-
```python
select sum(sales) as "Total Revenue", avg(sales) as "Avg Sales", min(sales) as "Min Sales", max(sales) as "Max sales" from sales
```

2) Total profit, average, minimum, maximum profit till now-
Similar to above query with minor modifications as per need

3) Top 5 cities with Maximum No. of customers
```python
select city, count(customer_id) from customer group by city order by count(customer_id) desc limit 5
```

4) Top 5 states, country with maximum No. of customers-
Similar to above query with minor modifications as per need
result- 
```python
"New York City",
"Los Angeles",
"Philadelphia",
"San Francisco",
"Seattle"
```
5) Region wise Customers distribution-
```python
select region, count(customer_id) from customer group by region order by count(customer_id) desc
```
From above we get, West region has maximum no. of customers and South has least.
Results-
```python
"West" -	255
"East" -	220
"Central" -	184
"South" -	134
```
6) Segment wise Customer distribution-
```python
select segment, count(customer_id) from customer group by segment order by count(customer_id) desc
```
Result-
```python
"Consumer" -	409
"Corporate" -	236
"Home Office" -	148
```

7) Toral No. of orders, Total no. of products ordered

#### Note- To get this one thing we'll have to understand is that if we look into sales table, One customer can make an order(order_id) with multiple products(product_id) orderred.
```python
select count(order_line) as "Total no. of products ordered", count(distinct product_id) as "Total no. of orders made" from sales
```
Results-
```python
"Total no. of products ordered" - 9994, "Total no. of orders made" -	1862
```

8) Above 7the query further can be asked by manager to get all the data(Total no. of products ordered) ,and (Total no. of orders made) from a particular date to a particular date-
from '2016-01-01' to '2016-12-01'- 
```python
select count(order_line) as "Total no. of products ordered", count(distinct product_id) as "Total no. of orders made" from sales where order_date between '2016-01-01' and '2016-12-01'
```


9) Number of Customers for age range between 31 and 40-
```python
select count(*) as "Total No. of customers" from customer where age between 31 and 40
```

10) Count No. of customers for each age-
```python
select age, count(age) as "Total No. of customers" from customer group by age order by age
```


11) TO get Total Expense, Total quantity of products sold, Total profit, Total Revenue for all the sales
```python
select (sum(sales)  - sum(profit))  as  "Total Expense",sum(quantity) as "Total quantity of products sold", sum(profit) "Total profit", sum(sales) as "Total Revenue" from sales
```


12) To get age and respective total profit of all time. This gives - Age which orders maximum no. of orders(can also be shown on histogram for visualization).
```python
select age, sum(profit) as "Total Profit" from customer as a left join sales as b on a.customer_id = b.customer_id group by age order by sum(profit) desc
```


13) TO get total profit and average profit for given condition-
age group divided into - 10-20, 20-30, 30-40, 40-50, 50-60, 60-70
```python
select age_dif, sum(profit) as "Total Profit", avg(profit) as "Average profit" from 
(select case when age between 10 and 20 then '10-20' when age between 20 and 30 then '20-30' when age between 30 and 40 then '30-40'
when age between 40 and 50 then '40-50' when age between 50 and 60 then '50-60' else '60-70' end as age_dif , a.age, b.profit from customer as a 
right join sales b on a.customer_id = b.customer_id )as U
group by age_dif order by sum(profit) desc
```
Result-
```python
age_dif Total Profit        	Average profit
"60-70"	76092.14320000002	36.39031238641799
"20-30"	54759.84040000009  	33.24823339404984
"40-50"	52922.588	        26.905230299949164
"50-60"	41789.160299999974	24.127690704387977
"30-40"	41340.23890000004	22.190144337090736
"10-20"	19493.050900000027	28.087969596541825
```

14) To get the total No. of products ordered grouped with age_dif-
```python
select age_dif, count(order_line) as "Total number of ordered products" from 
(select case when age between 10 and 20 then '10-20' when age between 20 and 30 then '20-30' when age between 30 and 40 then '30-40'
when age between 40 and 50 then '40-50' when age between 50 and 60 then '50-60' else '60-70' end as age_dif , a.age, b.order_line from customer as a 
roght join sales b on a.customer_id = b.customer_id ) as X
group by age_dif order by count(order_line) desc
```
Results-
```python
age_dif Total number of ordered products
"60-70"	2091
"40-50"	1967
"30-40"	1863
"50-60"	1732
"20-30"	1647
```

15) Region with maximum profit-
```python
select region, sum(profit) from customer as a right join sales as b on a.customer_id = b.customer_id group by region order by sum(profit) desc
```
Results-
```python
Region    	Total Profit
"West"	  	98008.22490000006
"East"	  	94604.31210000008
"Central"	63609.34900000003
"South"	  	30175.13570000007
```

16) Average time, Max time, and Min time taken by Shipping Modes-
```python
select ship_mode, avg(age(ship_date, order_date)) as "Average Time Taken", max(age(ship_date, order_date)) as "Max Time Taken", min(age(ship_date, order_date)) as "Min Time Taken"  from sales group by ship_mode order by avg(age(ship_date, order_date))
```
Results-
```python
ship_mode		Average Time Taken		Max Time Taken	Min Time Taken
"Same Day"		"01:03:38.78453"		"1 day"		"00:00:00"
"First Class"		"2 days 04:23:05.695709"	"4 days"	"1 day"
"Second Class"		"3 days 05:42:47.197943"	"5 days"	"1 day"
"Standard Class"	"5 days 00:09:24.61126"		"7 days"	"3 days"
```

17) To get all the data into single table unsing joins, with age_diff, month column which can be used for Full analysis/Dashboard-
```python
create table final as (select ROW_NUMBER() OVER (ORDER BY 1) AS id, 
					  substring(to_char(order_date,'yyyy-mm-dd'),6,2) as month ,
					  segment, country, city, state, region, category, sub_category, product_name, order_date, ship_date, ship_mode, sales, quantity, discount, profit,
					  case when age between 10 and 20 then '10-20' when age between 20 and 30 then '20-30' when age between 30 and 40 then '30-40'
	when age between 40 and 50 then '40-50' when age between 50 and 60 then '50-60' else '60-70' end as age_cat  from customer as a left join sales as b on a.customer_id = b.customer_id left join product as c on b.product_id = c.product_id)
```
```python
select * from final
```
The above query will be used for getting all the information together in single table named final.






