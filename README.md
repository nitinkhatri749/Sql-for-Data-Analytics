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
1) customer
2) sales
3) product

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

Let's start with queries -

1) Total Revenue(sales), average sales, minimum sales, maximum sales geneated till now-
```select sum(sales) as "Total Revenue", avg(sales) as "Avg Sales", min(sales) as "Min Sales", max(sales) as "Max sales" from sales```

2) Total profit, average, minimum, maximum profit till now-
Similar to above query with minor modifications as per need

3) Top 5 cities with Maximum No. of customers
```select city, count(customer_id) from customer group by city order by count(customer_id) desc limit 5```

4) Top 5 states, country with maximum No. of customers-
Similar to above query with minor modifications as per need
result- 
"New York City"
"Los Angeles"
"Philadelphia"
"San Francisco"
"Seattle"

5) Region wise Customers distribution-
```select region, count(customer_id) from customer group by region order by count(customer_id) desc```
From above we get, West region has maximum no. of customers and South has least.
Results-
"West" -	255
"East" -	220
"Central" -	184
"South" -	134

6) Segment wise Customer distribution-
```select segment, count(customer_id) from customer group by segment order by count(customer_id) desc```
Result-
"Consumer" -	409
"Corporate" -	236
"Home Office" -	148


7) Toral No. of orders, Total no. of products ordered

#### Note- To get this one thing we'll have to understand is that if we look into sales table, One customer can make an order(order_id) with multiple products(product_id) orderred.
```select count(order_line) as "Total no. of products ordered", count(distinct product_id) as "Total no. of orders made" from sales```
Results-
"Total no. of products ordered" - 9994, "Total no. of orders made" -	1862



