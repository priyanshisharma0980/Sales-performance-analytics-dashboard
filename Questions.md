### OBJECTIVE
To analyze sales data and build an interactive Power BI dashboard that helps stakeholders monitor revenue, profit, 
and sales performance across products, regions, and time periods for better data-driven decision making.

### Grain of your fact table?
One row per sales transaction per product per order
This means each row represented a single product sold in a specific order on a specific date.

### How did you handle Slowly Changing Dimensions (SCD)?
In real companies, dimension data changes over time. 
Example: 
A sales rep moves from North region → West region. 

With SCD Type 2 
It keeps historical records.  

### What challenges did you face while creating the data model?
One challenge was ensuring correct relationships between tables.
dimension tables had unique primary keys
fact tables used those keys as foreign keys


### How did you optimize performance for large datasets?
• Implemented a star schema data model
• Removed unnecessary columns before loading data into Power BI
• Used DAX measures instead of calculated columns when possible
• Created SQL views in Azure SQL Database to pre-transform data
• Ensured relationships were single direction and one-to-many


### Why did you create measures instead of calculated columns?  
Measures are calculated dynamically during report execution, while calculated columns are computed during data load  
Using measures helps reduce memory usage and improves performance, especially when working with larger datasets.


### How did you handle duplicate records in your data?  
While exploring the dataset in SQL Server, I identified duplicate order records.
To resolve this, I used SQL techniques such as:
GROUP BY checks
ROW_NUMBER() window functions

### What business insights did your dashboard reveal?  
• A few products contributed to a large portion of total revenue
• Some regions generated high revenue but had lower profit margins
• Certain sales representatives consistently outperformed others


### If your Power BI revenue number doesn’t match the SQL result, how would you debug it?  
First, I would verify the SQL source data.
SELECT SUM(revenue)
FROM sales_orders
Then I would compare it with the Power BI measure:
Total Revenue = SUM(FactSales[Revenue])
If numbers don’t match, I check:
Filters applied in Power BI
Duplicate records in the model
Relationship issues
Aggregation logic


### How would you find the Top 10 products by revenue using SQL?
SELECT TOP 10
product_id,
SUM(revenue) AS total_revenue
FROM sales_orders
GROUP BY product_id
ORDER BY total_revenue DESC



### What is the difference between Import Mode and DirectQuery?
Import Mode
Data is loaded into Power BI memory.
Advantages:
• faster dashboards
• better performance

DirectQuery
Power BI queries the database every time.
Advantages:
• real-time data



### What happens if you create a many-to-many relationship?
Many-to-many relationships can cause:
• incorrect totals
• ambiguous filtering


### How would you calculate Profit Margin?
Profit Margin =
DIVIDE([Total Profit], [Total Revenue])


### The dashboard helped identify:
• regions generating the highest revenue
• products with the highest profit margins
• seasonal sales trends



### Daily refresh at 6 AM

### How EDA Was Performed in the Sales Analytics Project
EDA is the process where we understand the data, clean, visulaise.
The goal of EDA was to understand the structure of the data, check data quality, identify patterns, 
and ensure the dataset was suitable for building the analytics dashboard.

First, I explored the structure of the tables to understand what kind of data was available.  
SELECT TOP 100 *
FROM sales_orders
This helped me identify:
• column names
• data types
• primary keys and foreign keys
• important metrics such as revenue and quantity


Checking dataset size - 
SELECT COUNT(*)
FROM sales_orders

We checked for most previous and most latest date
SELECT MIN(order_date), MAX(order_date)
FROM sales_orders

Then check for missing values

### Analyzing Sales Trends
To understand how sales changed over time, I analyzed monthly sales trends.
SELECT
YEAR(order_date) AS year,
MONTH(order_date) AS month,
SUM(revenue) AS monthly_revenue
FROM sales_orders
GROUP BY YEAR(order_date), MONTH(order_date)
ORDER BY year, month
This helped identify:
• seasonal trends
• months with higher sales
• revenue growth patterns.




### 3 types of Refresh Types in Power BI
Dataset refresh
Visual refresh  -- This happens automatically when a user interacts with filters or slicers.
Increamental refresh  -- Only new or updated data is loaded instead of refreshing the entire dataset.


### How Refresh Was Configured
After building the dashboard in Power BI Desktop, I published it to Power BI Service.
Steps:
Publish report to Power BI workspace
Go to dataset settings
Configure scheduled refresh
Refresh Frequency: Daily
Time: 6 AM



### How was automation implemented in your project?
Automation in this project was primarily implemented through scheduled dataset refresh in Power BI Service. After publishing the dashboard, 
I configured a daily refresh that automatically pulls updated sales data from the Azure SQL Database. 
This ensured that new sales transactions were automatically reflected in the dashboard without manual intervention. 
Additionally, all business metrics such as revenue and profit margins were calculated using DAX measures, 
which automatically recompute whenever new data is refreshed, allowing stakeholders to always view the most up-to-date sales performance.



### how did you load such big data into power bi with respect to this huge dataset?
Instead of loading a long query directly into Power BI, I created a SQL View.
To improve performance, I removed unnecessary columns.
For example, raw tables might contain:
internal system IDs
audit columns
metadata fields

Next, I connected Power BI to the Azure SQL Database.
Steps:
Power BI Desktop → Get Data → SQL Server
Connection details:
Server name
Database name
Then I selected the vw_sales_analytics view instead of raw tables.

Data Transformation in Power Query
Once the data was loaded into Power BI, I used Power Query for additional transformations.
Examples:
converting date formats
renaming columns
creating derived fields
removing null values



### Instead of loading a long query directly into Power BI, I created a SQL View.
A SQL View is basically a saved SQL query stored inside the database.

CREATE VIEW vw_sales_analytics AS
SELECT
o.order_id,
o.order_date,
p.product_name,
r.region_name,
o.revenue,
p.cost,
(o.revenue - p.cost) AS profit
FROM sales_orders o
JOIN products p
ON o.product_id = p.product_id
JOIN regions r
ON o.region_id = r.region_id

select * from vm_sales_Analytics



### how to find duplicate values?
Suppose you want to check if OrderID is duplicated.

SELECT order_id, COUNT(*) AS duplicate_count
FROM sales_orders
GROUP BY order_id
HAVING COUNT(*) > 1

GROUP BY order_id → groups all rows by order ID
COUNT(*) → counts how many times each order appears
HAVING COUNT(*) > 1 → returns only duplicated order IDs


OR

ROWNUMBER


For handling duplicates, I used the ROW_NUMBER() window function partitioned by order_id to identify duplicate entries and retain only the first valid record.

A window function performs calculations across a group of rows but without collapsing them into a single row.
Window functions add extra information to each row without removing rows.
ROW_NUMBER() assigns a sequential number to rows inside each group.

This method is used when you want to identify and remove duplicates.
SELECT *,
ROW_NUMBER() OVER(PARTITION BY order_id ORDER BY order_date) AS rn
FROM sales_orders

PARTITION BY order_id
→ groups rows by order ID
ROW_NUMBER()
→ assigns a number to each row
rn = 1 → keep
rn > 1 → duplicate


CTE to remove duplicate-

WITH CTE AS
(
SELECT *,
ROW_NUMBER() OVER(PARTITION BY order_id ORDER BY order_date) AS rn
FROM sales_orders
)
DELETE FROM CTE
WHERE rn > 1



### how to handle null values

First Step – Identify NULL Values
SELECT *
FROM sales_orders
WHERE revenue IS NULL


For numeric columns such as revenue or cost, NULL values were replaced with 0.
SELECT
order_id,
ISNULL(revenue,0) AS revenue
FROM sales_orders

During the data preparation phase, I first identified NULL values in key columns using SQL queries in SSMS. For numeric fields such as revenue and cost, 
I replaced NULL values with zero using functions like ISNULL or COALESCE to ensure calculations remained accurate. 
For critical fields such as product_id and order_date, rows with missing values were filtered out because they could not be used for meaningful analysis.


### calculate the top 10 order Id with highest revenue
SELECT TOP 10 
    order_id,
    SUM(revenue) AS total_revenue
FROM sales_orders
GROUP BY order_id
ORDER BY total_revenue DESC;


### HANDLE Inconsistent DATA
Inconsistent data occurs when the same information is represented in different formats or values across the dataset.
Eg- North, north, nrth
First Step – Detect Inconsistent Data
SELECT DISTINCT region_name
FROM regions

then-
SELECT
CASE
    WHEN region_name IN ('North','NORTH','N. Region') THEN 'North'
    WHEN region_name IN ('South','SOUTH') THEN 'South'
    ELSE region_name
END AS region_clean
FROM regions

Handling Inconsistent Product Names
SELECT
CASE
    WHEN product_name LIKE 'Laptop%' THEN 'Laptop'
    ELSE product_name
END AS product_category
FROM products



### DAX used
Total Revenue =
SUM(FactSales[Revenue])

Profit = Revenue - Cost (calculated in sql)
Total Profit =
SUM(FactSales[Profit])

Profit Margin =
DIVIDE([Total Profit], [Total Revenue], 0)

Total Orders =
COUNT(FactSales[OrderID])

Average Order Value =
DIVIDE([Total Revenue], [Total Orders], 0)

Revenue by Region =
SUM(FactSales[Revenue])

TOP N - Finding the top performing items based on a metric.
Examples in your project: Top 10 products with highest revenue
Product Rank =
RANKX(
ALL(DimProduct[ProductName]),
[Total Revenue],
,
DESC
)

Top 10 Products =
IF([Product Rank] <= 10, [Total Revenue])











































































































































































































