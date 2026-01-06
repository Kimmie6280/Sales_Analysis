
<h1>Sales Analysis (SQL + Tableau)</h1>
<h5>Project Overview</h5>

<p>
This project analyzes bike sales transaction data using SQL and Tableau to identify trends in revenue, profitability, customer demographics, and product performance. SQL was used to clean, aggregate, and analyze over 113,000 sales records, while Tableau was used to create an interactive dashboard that allows users to explore sales performance across time, geography, and customer segments.</p>
<hr>
Dataset:

<p>The dataset contains sales transactions with the following key attributes:</p>
<ol>
	<li>Order date (day, month, year)</li>
	<li>Customer demographics (age, age group, gender)</li>
	<li>Geography (country, state)</li>
	<li>Product information (category, sub-category, product)</li>
	<li>Financial metrics (order quantity, cost, revenue, profit</li>
</ol>

<hr>
<p>Business Questions</p>
<ol>
	<li>What are the total revenue, total profit, total orders, and total products sold?</li>
	<li>How does revenue change over time by month?</li>
	<li>How does profit change over time by month?</li>
	<li>Which product categories generate the most revenue and profit?</li>
	<li>Which age groups contribute most to sales?</li>
</ol>
<hr>
<h3>Dashboard Highlights</h3>
<h3>4 KPI cards showing:</h3>
<ol>
	<li>Total Revenue</li>
	<li>Total Profit</li>
	<li>Total Orders</li>
	<li>Total Products Sold</li>
</ol>
<h3>Monthly trend charts for:</h3>
<ol>
	<li>Revenue</li>
	<li>Profit</li>
</ol>
<h3>Category breakdowns by:</h3>
<ol>
	<li>Product Category</li>
	<li>Age Group</li>
</ol>
<h3>Fully interactive filters for:</h3>
<ol>
	<li>Year</li>
	<li>Gender</li>
	<li>Sub-Category</li>
</ol>
<hr>
<h3>Key Insights</h3>

Revenue and profit peak during specific months, indicating clear seasonality.

Adult and Young Adult age groups generate the highest revenue.

A small number of product categories drive the majority of sales.

Sales performance varies significantly by country and gender.
<hr>
<h3>Tools Used</h3>
<ul>
	<li>SQL (PostgreSQL)</li>
	<li>Excel (data cleaning)</li>
	<li>Tableau Public (dashboard & visualization)</li>
	<li>GitHub (project documentation)</li>
</ul>

--Sales Project:

```create database bike_sales;```

```
Create table
drop table if exists bikeSales;
CREATE TABLE bikeSales (
    Date_ DATE,
    Day_ INT,
    Month_ VARCHAR(20),
    Year_ INT,
    Customer_Age INT,
    Age_Group VARCHAR(30),
    Customer_Gender VARCHAR(1),
    Country VARCHAR(50),
    State_ VARCHAR(100),
    Product_Category VARCHAR(50),
    Sub_Category VARCHAR(50),
    Product VARCHAR(100),
    Order_Quantity INT,
    Unit_Cost INT,
    Unit_Price INT,
    Profit INT,
    Cost_ INT,
    Revenue INT
);
```
select all from bikeSales
```select * from bikeSales;```

-- How many Items are in the table 113036

```select count(*) from bikeSales;```

how many countries are in the table "France" "United States" "Australia" "Germany" "United Kingdom" "Canada"

``select distinct(country) from bikeSales;```

what are the age groups: "Youth (<25)" "Seniors (64+)" "Young Adults (25-34)" "Adults (35-64)"

```select distinct(Age_Group) from bikeSales;```

what are the distinct states: alot (53)

```select count(distinct(State_)) from bikeSales;```

What is the total revenue, total cost, and total profit? 30214112 ,85271008, 32221100

```select sum(Unit_Cost) from bikeSales;```
```select sum(Revenue) from bikeSales;```
```select sum(Profit) from bikeSales;```

What is the average order quantity per purchase? 11.9016596482536537

```select avg(Order_Quantity) from bikeSales```

 What are the top 10 bestselling product sub-categories?
```
select Sub_Category, sum(Order_Quantity)  AS total_quantity_sold from bikeSales group by 1 order by 2 DESC
LIMIT 10;
```
Which countries/states generated the highest revenue?

```select country , sum(Revenue) from bikeSales group by 1 order by 2 DESC;```

```select State_ , sum(Revenue) from bikeSales group by 1 order by 2 DESC;```

6. What months show the highest bike sales?

```select month_,sum(revenue) from bikeSales group by 1 order by 2 desc; ```

7. What is the year-over-year revenue growth? Compare total revenue by year.
select year_, sum(revenue), from bikeSales group by 1 order by 1;
```
select 
	sum(revenue), 
	year_, 
	lag(sum(revenue)) over (order by year_) as yoy,
	sum(revenue) - LAG(sum(revenue)) OVER(ORDER BY year_) AS yearChange
	from bikeSales group by year_;
```
8. What day of the week has the most orders?

```select * from bikeSales```

```select 
	 EXTRACT(DOW FROM date_) AS day_of_week,
	count(order_quantity)
	from bikeSales
	group by 1
	order by 2
```

9. Is there a seasonal trend (e.g., summer vs winter)?

```select distinct(sub_category) from bikeSales```

```select distinct(month_),sum(order_quantity) from bikeSales where year_ = 2014 group by 1 order by 2 ```

10. Which age group spends the most money?

```select age_group, sum(revenue) from bikeSales group by 1 order by 2 desc```

11. What is the average revenue per customer age group?

```select age_group, avg(revenue) from bikeSales group by 1 order by 2 desc```

12. Do men or women buy more?: Compare quantity, revenue, profit.

```select * from bikeSales```

```select customer_gender, sum(order_quantity), sum(revenue), sum(profit) from bikeSales group by 1```

13. Which age group purchases each product category the most?

```
select distinct(product_category) from bikeSales
select age_group,  sum(order_quantity)from bikeSales group by 1
```
14. What country/state has the oldest vs youngest customers?

```
select country, min(customer_age) as young, max(customer_age) as old from bikeSales group by 1
select distinct(customer_age) from bikeSales
```

15. Which product categories generate the highest: Revenue Profit Quantity sold

```
select product_category, sum(revenue), sum(profit), sum(order_quantity)  from bikeSales group by 1
```

16. Which sub-categories have the highest profit margin?

```select sub_category ,sum(revenue), sum(profit) from bikeSales group by 1```

17. What sub-category is most popular for each gender?
```
select sub_category , customer_gender , sum(order_quantity),
	Row_Number() over (  PARTITION BY customer_gender order by 3 desc) as most_popular_by_gender from bikeSales group by 1 ,2 
```
	
18. What is the most profitable product by state?
```select * from bikeSales;```

```select state_ , sum(profit) from bikeSales group by 1```

```select product , count(product) , sum(profit) from bikeSales group by 1 order by 2 desc;```
```
select state_ ,product,  sum(profit),
	Row_Number() over (PARTITION BY state_ order by 3 desc) as profit_by_state  from bikeSales group by 1,2
```
19. What is the average profit per order?

```select avg(profit) from bikeSales
select product, sum(order_quantity), avg(profit)  from bikeSales group by 1
select product, avg(profit) from bikeSales group by 1;```

