Project title: Retail sales analysis
database: project
This project is designed to demonstrated SQL skill and technics typicaly used by data analyst to explore, clean and analyze retail sales data. The Project involves setting up a retail sales database , performong EDA and answering specific business problem through SQL queries.
Objectives:  i)database creation, data cleaning, EDA, BUSINESS QUERIES SOLUTION;

use projects
select * from dbo.Retail_sales

select count(*) as total_count from Retail_sales

select * from Retail_sales
where transactions_id is null

--data cleaning--

select * from Retail_sales
where sale_date is null
or
sale_time is null
or
customer_id is null
or
gender is null
or
age is null
or
category is null
or
quantiy is null
or
price_per_unit is null
or
cogs is null
or
total_sale is null;

delete from dbo.Retail_sales
where 
sale_date is null
or
sale_time is null
or
customer_id is null
or
gender is null
or
age is null
or
category is null
or
quantiy is null
or
price_per_unit is null
or
cogs is null
or
total_sale is null;


----data exploration--
--how many sales we have--
select count(*) from Retail_sales

---how many customer we have--
select count(customer_id) from Retail_sales
--category count--
select count(distinct(category)) from Retail_sales

--data analysis and business key problems---

--1.WTQ TO RETRIEVE ALL COLUMNS FOR THE SALE MADE ON -2022-11-05--
select * from Retail_sales
where sale_date='2022-11-05'

--2.WTQ TO RETRIEVE ALL TRANSACTIONS WHERE THE CATEGORY IS 'CLOTHING' AND QTY IS SOLD MORE THAN 4 FOR THE MONTH OF NOV-2022
select * from Retail_sales
where category='clothing'
and sale_date BETWEEN '2022-11-01' AND '2022-11-30' 
and quantiy>=4






3. -- total sales for each category--

SELECT category,sum(total_sale) as total_sales,count(*) as total_orders from Retail_sales
group by category

--check-
select category,count(transactions_id) from Retail_sales
group by category

  ---4.  average age of customers who purchashed items for the beauty category--

  select avg(age)  avg_age from Retail_sales
  where category='Beauty'

  --5. WTQ to find all transactions where the total_sales is greater than 1000--
  select transactions_id, total_sale from Retail_sales
  where total_sale>1000

  --6. WTQ to find the total number of transactions	made by each gender in each category--
  select  gender, category,count(*)as count_ from Retail_sales
  group by gender, category
  order by 1

  7.-- CALCULATE AVG SALES FOR EACH MONTH, BEST SELLING MONTH

  select 
  year_,month_,avg_sales
  from
  
  (select 
  year(sale_date) as year_,
 month (sale_date) as month_,
  avg(total_sale) as avg_sales, 
  rank() over(partition by year(sale_date) order by avg(total_sale) desc) as rank
 from Retail_sales
  group by year(sale_date),month (sale_date))

 as t1
  where rank=1


  --8.--top 5 customer based on total sales--
  select top 5 customer_id,sum(total_sale) as sales_ from Retail_sales
  group by customer_id 
  order by sum(total_sale) desc

  --9--wtq unique customer whicjh purchased from each category
  select category,count(distinct(customer_id)) as count_ from Retail_sales
  group by category

  ---10-create each shift and no of orders(morning , evening,noon)
  select *,
		case
		when datepart(hour,sale_time)<12 then 'morning'
		when datepart(hour,sale_time) between 12 and 17 then 'afernoon'
		else 'evening'
		end as shift_

   from Retail_sales


with hourly_sales as 
(
select *,
		case
		when datepart(hour,sale_time)<12 then 'morning'
		when datepart(hour,sale_time) between 12 and 17 then 'afernoon'
		else 'evening'
		end as shift_

   from Retail_sales)

   SELECT shift_,COUNT(*) AS TOTAL_ORDRES FROM hourly_sales
   group by  shift_



