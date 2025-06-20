# Exploring-Restaurant-Data-with-SQL
## 🍽️ Restaurant Data Analysis
This repository contains SQL scripts used to explore and analyze sales data from a restaurant. The aim of this project was to gain insights into menu performance, popular dishes, order trends, and customer spending habits.
---

## 📊 Project Structure

The SQL queries are organized into three main files, each focusing on a different aspect of the restaurant's operations:

Restaurant analysis 1.sql: Focuses on menu_items table, exploring menu details, pricing, and category breakdowns.

Restaurant analysis 2.sql: Focuses on the order_details table, examining order dates, quantities, and order sizes.

Restaurant analysis 3.sql: Combines menu_items and order_details to analyze popular items, sales performance, and high-value orders.

---

## 🚀 Key Questions Answered
This analysis addresses several key questions, providing valuable insights for the restaurant:

## Menu Insights (from Restaurant analysis 1.sql)
How many food items are on the menu? 🍔

What are the lowest and highest priced food items? 💰

How many Italian dishes are on the menu, and what are their price ranges? 🍝

How many dishes are in each category? 🍲

What is the average price per category? 📈

```sql
-- View menu table --
select * 
from menu_items;

-- How many food items do we have on our menu table--
select 
count(*) 
from menu_items;

-- what is the food item with the lowest price on our menu--
select * 
from menu_items
order by price;

-- what is the food item with the highest price on our menu--
select * 
from menu_items
order by price DESC;

-- what is the number of italian dishes sold on my menu---
select count(*) from menu_items 
WHERE category = 'Italian';

-- what is the least expensive italian dishes on my menu based on its price --
select * from menu_items 
WHERE category = 'Italian'
order by price;

-- what is the most expensive italian dishes on my menu based on its price --
select * 
from menu_items 
WHERE category = 'Italian'
order by price DESC;

-- How many dishes are in each category --
select category, count(menu_item_id) as num_items
from menu_items
group by category;

-- Average price per category --
select category, avg(price) AS avg_price
from menu_items
group by category;
```

## Order Details & Trends (from Restaurant analysis 2.sql)
What is the date range of the order data? 📅

How many unique orders were placed within this period? #️⃣

What is the total number of items ordered? 📦

Which orders had the most items? 🛒

How many orders contained more than 12 items? 🔢

```sql
-- View order details table--
select *
from order_details;

-- What is the date range of the table--
select *
from order_details
order by order_date;

-- what is the minimum and maximum order date--- 
select min(order_date), max(order_date)
from order_details;

-- How many orders were made within this date range--
select count(distinct order_id)
from order_details;

-- How many items were ordered within this date range--
select count(*)
from order_details;

-- which orders had the most number of items --
select order_id, count(item_id)as num_items
from order_details
group by order_id
order by num_items DESC;

-- how many orders have more than 12 items --
Select count(*) from
(select order_id, count(item_id)as num_items
from order_details
group by order_id
HAVING num_items > 12) AS num_orders;
```

## Sales Performance & Popularity (from Restaurant analysis 3.sql)
What were the least and most ordered items, and in which categories do they belong? 📉 ascends and ascends 🚀

What were the top 5 orders by total spending? 💸

What were the categories of items included in the highest-spending order (Order ID: 440)? 🌟

```sql
-- View information on the nemu items and the order details table--
select * from order_details
left join menu_items
on order_details.item_id = menu_items.menu_item_id;

-- what were the least and most ordered items? what categories were they in?
select item_name, CATEGORY, count(order_details_id) AS num_purchases
from order_details
left join menu_items
on order_details.item_id = menu_items.menu_item_id
group by item_name, CATEGORY
order by num_purchases;

select item_name, category, count(order_details_id) AS num_purchases
from order_details
left join menu_items
on order_details.item_id = menu_items.menu_item_id
group by item_name, category
order by num_purchases DESC;

-- what were the top 5 orders that spent the most money--
select  order_id, sum(price) as sum_price from order_details
left join menu_items
on order_details.item_id = menu_items.menu_item_id
group by order_id
order by sum_price DESC
limit 5;

-- View details of highest spend order--
select category, count(item_id) from order_details
left join menu_items
on order_details.item_id = menu_items.menu_item_id
where order_id = 440
group by category;
```

## 🛠️ Technologies Used
MySQL Workbench: For writing and executing SQL queries.

SQL: For data extraction and analysis.
