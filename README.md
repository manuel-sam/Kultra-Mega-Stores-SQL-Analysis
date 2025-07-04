# Kultra-Mega-Stores-SQL-Analysis
SQL-based business analysis project using transactional data from Kultra Mega Stores to uncover insights on sales, customer value, and shipping strategy (2009–2012).

## Project Overview
Kultra Mega Stores is a major supplier of office supplies and furniture in Nigeria, with a focus on serving both B2B and B2C customers. This project focuses on the Abuja division's order data from 2009 to 2012. 2009 to 2012.

### The dataset contains transactional data such as:
- Customer and segment information
- Product categories and sub-categories
- Order and shipping details
- Revenue, profit, and shipping cost

## Objectives
The analysis is split into two case scenarios:
**Case Scenario I:** Operational Insights
1. Identify the highest-selling product category
2. Find the top 3 and bottom 3 regions by total sales
3. Calculate total appliance sales in Ontario
4. Analyze the bottom 10 customers by revenue
5. Determine the most costly shipping method

**Case Scenario II:** Customer & Shipping Strategy
1. Identify the most valuable customers and what they buy
7. Find the top small business customer by sales
8. Determine which corporate customer made the most orders
9.  Find the most profitable consumer customer
10. Check for any product returns and associated segments
11. Evaluate if shipping costs align with order priority

## Understanding Ship Mode
Ship Mode refers to how an order is delivered. Each method has different implications for speed, cost, and customer experience.

| Ship Mode         | Description                                                             | Speed       | Cost           | Ideal For                   |
|------------------|--------------------------------------------------------------------------|-------------|----------------|-----------------------------|
| Express Air       | Fastest air transport, often overnight shipping                          | Fastest     | Highest         | Critical/Urgent orders      |
| Regular Air       | Standard air delivery for typical orders                                 | Moderate    | Medium          | Medium or High priority     |
| Delivery Truck    | Ground transport used for local or less urgent deliveries                | Slowest     | Varies          | Low-priority, local orders  |

Using fast and expensive shipping methods like Express Air for low-priority orders increases operational costs. Delivery Truck is generally economical when used for local, non-urgent bulk deliveries. However, if it is used inefficiently—for small, long-distance, or urgent shipments—it becomes costly.

## Tools Used
- MySQL Workbench for SQL queries
- Excel for data cleaning

##  Case Scenario I – Operational Insights

``` SQL
-- 1. Highest Selling Product Category
SELECT Product_Category, SUM(sales) AS Total_Sales
FROM kms_table
GROUP BY Product_Category
ORDER BY Total_Sales DESC
LIMIT 1 ;
RESULT: Technology (5984253)
```
```SQL
--  2. Top And Bottom 3 Region
--  Top 3 
SELECT region, SUM(Sales) AS Total_Sales
FROM kms_table
GROUP BY Region
ORDER BY hgh DESC
LIMIT 3 ;  

--  Bottom 3
SELECT region, SUM(Sales) AS Total_Sales
FROM kms_table
GROUP BY Region
ORDER BY hgh ASC
LIMIT 3 ;
```
``` SQL
--  3. Appiance Sales in Ontario
SELECT Province, SUM(sales)
FROM kms_table
WHERE province = "Ontario"
;
```
``` SQL
--  4. Bottom 10 Custmers
SELECT  Customer_Name, SUM(sales) AS Total_Sales
FROM kms_table
GROUP BY Customer_Name
ORDER BY Total_Sales ASC
LIMIT 10
;
```
``` SQL
--  5. Most Costly Shipping Method
SELECT ship_mode, SUM(shipping_cost) AS Total_Shipping_Cost
FROM kms_table
GROUP BY Ship_Mode
ORDER BY Total_Shipping_Cost DESC
LIMIT 1 ;
```
``` sql
--  CASE SCENARIO II
--  6. Top 10 Customers and Their Product Preferences
SELECT Customer_Name, Product_Name, Order_Quantity, Sales,
ROW_NUMBER() OVER(ORDER BY sales DESC) AS position
FROM kms_table
LIMIT 10 ;
```
``` sql
--  7. Top Small Business Customer
SELECT Customer_Name, Customer_Segment, Sales,
ROW_NUMBER() OVER(ORDER BY sales DESC) AS position
FROM kms_table
WHERE Customer_Segment="Small Business"
LIMIT 1 ;
```
``` sql
--  8. Customer With The Most Number of Orders
SELECT customer_name, order_id, customer_segment, Order_Quantity,
ROW_NUMBER() OVER(ORDER BY Order_Quantity DESC) AS Position
FROM kms_table
WHERE Customer_Segment="Corporate"
LIMIT 66 ;
```
``` sql
--  9. Most Profitable Customer
select * from kms_table;
SELECT customer_name, order_id, customer_segment, Profit,
ROW_NUMBER() OVER(ORDER BY Profit DESC) AS Position
FROM kms_table
LIMIT 1 ;
```

### Findings:
Express Air was used even for low-priority orders, which led to cost inefficiency. Delivery Truck, though slower, had the highest total cost—indicating possible overuse or misalignment with priority.



| Area                | Recommendation                                  |
|---------------------|--------------------------------------------------|
| Shipping Strategy   | Align shipping methods with order priorities     |
| Low Sales Regions   | Focus on marketing and logistics improvements    |
| Customer Segments   | Upsell to bottom-tier customers                  |
| High-Value Clients  | Implement loyalty programs                       |
| Returns Management  | Include return tracking in future datasets       |
