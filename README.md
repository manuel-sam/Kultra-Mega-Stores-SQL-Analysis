# Kultra-Mega-Stores-SQL-Analysis
SQL-based business analysis project using transactional data from Kultra Mega Stores to uncover insights on sales, customer value, and shipping strategy (2009–2012).

## Project Overview
This project features a detailed analysis of four years' worth of sales and order data from Kultra Mega Stores, a Nigerian-based office supply and furniture retailer. Using SQL (MySQL Workbench), the project uncovers key business insights, including top-performing products, customer behaviors, shipping cost efficiency, and regional sales performance. The analysis supports strategic decision-making aimed at improving profitability, optimizing shipping logistics, and strengthening customer engagement across various segments.

## Project Scope
This project explores four years of sales and shipping data from Kultra Mega Stores using SQL. It focuses on finding patterns in customer behavior, product performance, and shipping efficiency. The goal is to uncover practical insights that can help the business improve sales, reduce costs, and serve customers better.

## Data Source

The dataset for this project was provided by DSA Hub as part of the Data Analytics Program. It was designed for learning and portfolio-building purposes and simulates a real-life business scenario for Kultra Mega Stores.

The data comes in Excel format and includes 19 fields capturing four years of order and customer activity. Below are the key columns and what they represent:

- **Row_ID** Unique row identifier

- **Order_ID** Unique identifier for each customer order

- **Order_Date** Date the order was placed

- **Order_Priority** Urgency of the order (e.g., Critical, High, Medium, Low)

- **Order_Quantity** Total items ordered

- **Sales** Total sales value for the order

- **Discount** Any discount applied

- **Ship_Mode** Method used to deliver the order (e.g., Express Air, Delivery Truck)

- **Profit** Net profit from the order

- **Unit_Price** Price per unit of product

- **Shipping_Cost** Cost of delivering the order

- **Customer_Name** Full name of the customer

- **Province** Customer's province

- **Region** Customer's region

- **Customer_Segment** Type of customer (Consumer, Corporate, Small Business)

- **Product_Category** Broad category of product (e.g., Technology, Furniture)

- **Product_Sub_category** More specific product type

- **Product_Name** Full name of the item sold

- **Product_Container** Packaging type of the product

- **Product_Base_Margin** Base margin used to calculate profitability

- **Shiping_Date** Date the order was shipped

In addition to the main dataset, a separate `Order_Status` table was later provided. This table contains the `Order_ID` and a new field called `Order_Status`, which tracks whether an item was returned. I used a SQL JOIN operation to merge this table with the main dataset to gain insights into returns by segment and customer. This additional data was essential for answering the question on customer returns and improved the overall depth of the analysis.

These variables work together to give a complete picture of Kultra Mega Stores’ operations—covering customer behavior, product trends, sales performance, logistics, and returns.


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

##  Case Scenario I – Operational Insights

``` SQL
-- 1. Highest Selling Product Category
SELECT Product_Category, SUM(sales) AS Total_Sales
FROM kms_table
GROUP BY Product_Category
ORDER BY Total_Sales DESC
LIMIT 1 ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/1.%20Category%20with%20Highest%20Sales.png)

```
```SQL
--  2. Top And Bottom 3 Region
--  Top 3 
SELECT region, SUM(Sales) AS Total_Sales
FROM kms_table
GROUP BY Region
ORDER BY hgh DESC
LIMIT 3 ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/2.%20Top%203%20Region.png) 

``` SQL
--  Bottom 3
SELECT region, SUM(Sales) AS Total_Sales
FROM kms_table
GROUP BY Region
ORDER BY hgh ASC
LIMIT 3 ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/3.%20Appliances%20Sales%20in%20Otario.png)

``` SQL
--  3. Appiance Sales in Ontario
SELECT Province, SUM(sales)
FROM kms_table
WHERE province = "Ontario" ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/3.%20Appliances%20Sales%20in%20Otario.png)

``` SQL
--  4. Bottom 10 Custmers
SELECT  Customer_Name, SUM(sales) AS Total_Sales
FROM kms_table
GROUP BY Customer_Name
ORDER BY Total_Sales ASC
LIMIT 10 ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/4.%20Bottom%2010%20Customers.png)

``` SQL
--  5. Most Costly Shipping Method
SELECT ship_mode, SUM(shipping_cost) AS Total_Shipping_Cost
FROM kms_table
GROUP BY Ship_Mode
ORDER BY Total_Shipping_Cost DESC
LIMIT 1 ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/5.%20Costly%20Shipping%20Method.png)

``` SQL
--  CASE SCENARIO II
--  6. Top 10 Customers and Their Product Preferences
SELECT Customer_Name, Product_Name, Order_Quantity, Sales,
ROW_NUMBER() OVER(ORDER BY sales DESC) AS position
FROM kms_table
LIMIT 10 ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/6.%20Valuable%20customers%20and%20their%20products.png)

``` SQL
--  7. Top Small Business Customer
SELECT Customer_Name, Customer_Segment, Sales,
ROW_NUMBER() OVER(ORDER BY sales DESC) AS position
FROM kms_table
WHERE Customer_Segment="Small Business"
LIMIT 1 ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/7.%20Small%20business%20with%20the%20highest%20sales.png)

``` SQL
--  8. Customer With The Most Number of Orders
SELECT customer_name, order_id, customer_segment, Order_Quantity,
ROW_NUMBER() OVER(ORDER BY Order_Quantity DESC) AS Position
FROM kms_table
WHERE Customer_Segment="Corporate"
LIMIT 66 ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/8.%20Corporate%20Customer%20with%20most%20no.%20of%20orders.png)

``` SQL
--  9. Most Profitable Customer
select * from kms_table;
SELECT customer_name, order_id, customer_segment, Profit,
ROW_NUMBER() OVER(ORDER BY Profit DESC) AS Position
FROM kms_table
LIMIT 1 ;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/9.%20Most%20Profitable%20Customer.png)

``` SQL
-- JOIN KMS_TABLE and ORDER_STATUS TOGETHER
-- 10. Customers with Returned Items
SELECT customer_name, order_id, customer_segment, Status,
ROW_NUMBER() OVER(ORDER BY `Order ID` DESC) AS Position
FROM kms_table AS KMS
JOIN order_status AS ORD
	ON KMS.Order_ID = ORD.`Order ID`;
```
![](https://github.com/manuel-sam/Kultra-Mega-Stores-SQL-Analysis/blob/main/10.%20Customers%20with%20Returned%20Items.png)

## Question 11: Did the company appropriately spend shipping costs based on Order Priority?

Based on the analysis of Kultra Mega Stores’ shipping methods and order priorities, it’s clear that there was a mismatch between how orders were prioritized and how they were shipped. Specifically, Express Air, which is the fastest and most expensive shipping method, was often used for orders marked with Low or Medium priority. This is concerning because those orders did not demand urgent delivery, yet they attracted significantly higher shipping costs.

A specific example of this can be seen in Order_ID 19583, placed by a consumer customer named Michael Johnson. The order was marked with Low Priority but was shipped via Express Air, incurring a high shipping cost of ₦89,980. There was no urgent need based on the priority tag, making this an inefficient logistics decision.

On the other hand, Delivery Truck, considered the most economical but slowest shipping method, turned out to have the highest overall cost. This may be due to volume, but it also points to a possible overuse or poor alignment with actual order needs. If the goal was to save cost on non-urgent deliveries, the execution didn’t fully align with that strategy.

To summarize, the data showed that shipping choices were not consistently aligned with the stated order priorities. Express Air should have been reserved for critical or high-priority orders only, but in practice, it was used more broadly. If KMS wants to manage costs better, especially in a competitive retail space, it should review its shipping policies and ensure logistics decisions reflect the actual urgency of each order.



### Findings:
Express Air was used even for low-priority orders, which led to cost inefficiency. Delivery Truck, though slower, had the highest total cost—indicating possible overuse or misalignment with priority.



| Area                | Recommendation                                  |
|---------------------|--------------------------------------------------|
| Shipping Strategy   | Align shipping methods with order priorities     |
| Low Sales Regions   | Focus on marketing and logistics improvements    |
| Customer Segments   | Upsell to bottom-tier customers                  |
| High-Value Clients  | Implement loyalty programs                       |
| Returns Management  | Include return tracking in future datasets       |

## Conclusion
By implementing the recommendations and aligning shipping strategies with actual order priorities, Kultra Mega Stores can reduce unnecessary logistics expenses, improve customer satisfaction, and make more informed operational decisions.

I’m currently seeking an internship or junior data analyst role where I can apply my SQL and data interpretation skills, continue learning, and contribute meaningfully to a data-driven team.

Feel free to connect with me on LinkedIn.

Thank you for reading!
