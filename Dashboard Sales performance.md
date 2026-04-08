### OBJECTIVE
To analyze sales data and build an interactive Power BI dashboard that helps stakeholders monitor revenue, profit,   
and sales performance across products, regions, and time periods for better data-driven decision making.  
The objective of the project was to build a Sales Performance Analytics Dashboard that allows business stakeholders to monitor key metrics such as revenue,   profit, regional performance, and product sales trends. The dashboard helps management identify high-performing products and regions and make data-driven   business decisions.  

### What were the main tables used in your dataset?
Fact Table- fact_sales   
Dimension Tables  - dim_product, dim_region, dim_date, dim_customer  

### How did you handle NULL values in your project?
For numeric fields like revenue or quantity, I replaced NULL with 0 using ISNULL().  
For critical fields like OrderID or ProductID, records with NULL values were removed because they would break relationships.  
For text fields like region names, I replaced NULL with default values like "Unknown".  

SELECT ISNULL(Revenue,0) AS Revenue   
FROM sales_data;  

### Why did you create a SQL view for the dataset?
A SQL view was created to simplify the Power BI data model and centralize business logic in the database.  
Instead of loading multiple tables into Power BI, the dashboard connects to a single analytical view, which improves maintainability and performance.   


### What is the difference between a fact table and a dimension table?
Fact tables contain measurable business metrics such as revenue, quantity, and profit.  
Dimension tables contain descriptive attributes such as product name, region, and customer details.   

### How did you check data quality in the project?
Data quality checks included:  
checking duplicates  
checking NULL values  
verifying row counts  
validating relationships between tables   

### What window functions did you use?
I used window functions such as:  
ROW_NUMBER() for duplicate detection  
RANK() for ranking products by revenue   
SELECT  
RANK() OVER(ORDER BY SUM(Revenue) DESC) AS Rank  
FROM sales_data  
GROUP BY ProductID;    


### CTE to remove duplicate-  
A CTE (Common Table Expression) is a temporary result set used to simplify complex queries.   
It improves readability and query structure.   

### What is the difference between UNION and UNION ALL?
UNION removes duplicate rows.  
UNION ALL keeps duplicates and is faster.  

### What INDEX did you use to optimize performance?
Indexes were created on key columns used in joins and filters.  
Indexes improve query performance by reducing table scans.   
An index improves query speed.  
CREATE INDEX idx_orderid    
ON EmpSalary1(salary);   
EXEC sp_helpindex 'EmpSalary1';   

INDEX on NON_PRIMARY KEY columns are called unclustered indexes
Use database   
GO  
Create NONCLUSTERED Index IX_empname   
ON Employee([name])
GO  



### How did you identify top-selling products?
I grouped sales by product and calculated total revenue.  
SELECT  
ProductID,  
SUM(Revenue) AS TotalRevenue  
FROM sales_data  
GROUP BY ProductID  
ORDER BY TotalRevenue DESC;   


### What business insights did the dashboard provide?
top-performing products    
regions generating the most revenue  
yearly sales trends   


### How do you QUERY OPTIMISATION?
alway choose specific columns   
use top/limit       
use index  

IN POWER BI - Use unable load   

EXECUTION PLAN -  
When query runs the execution plan decides if we do full table scans or check for indexes  
then it checks if joins are possible or not  
and then retirve data for select statement  
and stores the result of execution plan to CACHE  

Reads data from right to left  
Now use TABLE SCAN   click click - Properties   
check number of rows that are read  
An index improves query speed.  
CREATE INDEX idx_orderid    
ON EmpSalary1(salary);   
EXEC sp_helpindex 'EmpSalary1';   


### How do you handle slowly changing dimensions?
3 types SCD 1,2,3    
SCD 1- the new data replaces the old data  
SCD 2 - new data adds in, without deleting anything. and all the historical records are kept in  
SCD 3 - It stores only the current value and the immediately preceding value side-by-side in the same record.   
SCD 2 - when new data comes, the status of old data = 0 and new data = 1  
Merge employee as target  
using stagingemployee as source  
on tagert.empid=source.empid    
when matched and status =1   
set status=0   
Now insert data from staging database to original database using-  
insert into employee
select * from stagingemployee


### What would happen if your dataset grows from 500K rows to 50 million?
Use incremental refresh - Do not reload the entire dataset every time. Load only new or changed data.   
Instead of storing every transaction for reporting, we create summarized data. Eg - for each category grup it and then show the toal revenue instaed of individual 
Indexes help SQL Server find rows faster.  


### Find the second highest salary from a table.
SELECT MAX(Salary)  
FROM Employees  
WHERE Salary < (SELECT MAX(Salary) FROM Employees);  

### Find Duplicate Records
SELECT OrderID, COUNT(*)  
FROM sales_orders  
GROUP BY OrderID  
HAVING COUNT(*) > 1;   



-----------------------------------------------------------------------------------------------------------------------------------------------------------     

## POWER BI

### Revenue Growth =   
DIVIDE(  
    [Current Revenue] - [Previous Revenue],   
    [Previous Revenue]    
)    

### YTD 
YTD Revenue =  
TOTALYTD(  
    [Total Revenue],  
    DimDate[Date]  
)  

### YoY %
Revenue Last Year =    
CALCULATE(  
    [Total Revenue],  
    SAMEPERIODLASTYEAR(DimDate[Date])   
)  
This will create a new measure table column for revenue last year    


### YoY Growth % =  
DIVIDE(  
    [Total Revenue] - [Revenue Last Year],   
    [Revenue Last Year]  
)  
(to calculate revenue last year


### TOP N - 
rankx() - used for Ranking items based on a measure, such as total sales, allowing for easy identification of top and bottom performers.   
Product Rank =  
RANKX(  
    ALL(DimProduct[ProductName]),   
    [Total Revenue],  
    ,  
    DESC   
)  

used with ALL - ALL product names, based on total_revenue   
Then applied filter → Top 10 products.   

### CHARTS
Line charts show trends over time clearly.  
Bar charts allow easy comparison between categories. Eg- Revenue by region and Revenue by product   
Pie chart Shows percentage contribution. Eg- Revenue share by product category   


### Roles in Power BI Services
Admin	        full control  
Member	      edit reports  
Contributor	  publish reports  
Viewer	      view dashboards  


---------------------------------------------------------------------------------------------------------------------    


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
Disabling "Enable Load" in Power Query prevents a table from loading into the Power BI report view and data model,    
reducing model size, improving performance, and cleaning up the field list.    


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

checking number of rows and columns  
DESCRIBE table_name;   

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


For handling duplicates, I used the ROW_NUMBER() window function partitioned by order_id to identify duplicate entries and   retain only the first valid record.  

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
A CTE (Common Table Expression) is a temporary result set used to simplify complex queries.  
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

During the data preparation phase, I first identified NULL values in key columns using SQL queries in SSMS. For numeric   fields such as revenue and cost,   
I replaced NULL values with zero using functions like ISNULL or COALESCE to ensure calculations remained accurate.   
For critical fields such as product_id and order_date, rows with missing values were filtered out because they could not be  used for meaningful analysis.  


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



### TABLES USed and COLUMNS used
TABLES -  
FactSales  
DimProduct  
DimRegion  
DimCustomer  
DimDate  

FACTSALES - 
OrderID	Unique order identifier
OrderDate	  
ProductID	  
CustomerID	  
RegionID	  
SalesRepID	  
Quantity	  
Revenue	  
Cost	  
Profit


DimProduct-  
ProductID	Primary key  
ProductName  	
Category  
Brand	  
Cost  

DimRegion
RegionID	 
RegionName	  
Country   

DimCustomer
CustomerID  	
CustomerName	  
CustomerSegment   


### DIfferent source dataset 
Now these tables are in different data sources  
we use JOIN and VIEW  

### WHOLE PROCESS  
STEP 1 -  
Companies usually store this raw data in staging tables.  
Example staging tables:  
stg_sales_orders  
stg_products  
stg_regions  


STEP 2-  
Data Profiling  
Before transforming data, analysts explore the data  
Check number of rows - SELECT COUNT(*) FROM stg_sales_orders;  

Check duplicates:  
SELECT OrderID, COUNT(*)  
FROM stg_sales_orders  
GROUP BY OrderID  
HAVING COUNT(*) > 1;  

Check NULL values:  
SELECT *  
FROM stg_sales_details  
WHERE Revenue IS NULL;   


Step 3 —   
Cleaning Data Using CTE  
CTEs are widely used to clean data.
Example: Removing duplicate orders.
WITH CTE AS   
(   
SELECT *,   
ROW_NUMBER() OVER(PARTITION BY order_id ORDER BY order_date) AS rn   
FROM sales_orders   
)   
DELETE FROM CTE   
WHERE rn > 1   

SELECT *
FROM sales_orders
WHERE rn = 1;

 Step 4 -  
 Handling NULL data  
SELECT  
OrderID,  
ProductID,  
Quantity,  
ISNULL(Revenue,0) AS Revenue  
FROM stg_sales_details;  


 Step 5 —   JOINS and Derivied columns
 Combining Tables Using JOIN   and   
SELECT
d.OrderID,  
p.ProductName,  
(d.Revenue - p.Cost) AS Profit  
FROM stg_sales_details d  
JOIN stg_products p  
ON d.ProductID = p.ProductID;    

Step 7-  
Stored Procedure for Automation  
Companies automate data preparation using stored procedures.  


Step 8 — Final Analytical View  
Finally, analysts create a view for Power BI.
CREATE VIEW vw_sales_analytics  
AS  
SELECT  
OrderID,  
OrderDate,  
ProductName,  
RegionName,  
Quantity,  
Revenue,  
Profit  
FROM fact_sales;   


### DRILL DOWN  
when we add, eg - product and store in x axis   
y axis will be price  
legend - store  

we want in x axis to show details for each product under each store    
we are using the bar chart there is a down arrow, click on it, it is drill down   
if I click on each store then it will show for each product under each store    



### Query folding  
Process of pushing data transformation steps back to data source   
used while implementing increamental refresh   
supported for - removing, renaming, numeric calculations, joins etc    

Does not support - customer columns,  merging, appending from different tables   
Does not support - csv, excel files   

in M language - value.nativequery(tablename, select * from table,[enablefolding=True])   



### Row level security
Modelling - manage roles - give roles eg- country - then select table, right click on add filter   
in power BI services - Data set - 3 dots - security - enter IDs   

Dynamic RSL   
create a measure -    
username = userprincipalname()    
shared the email of accounts that have logged in   
now got ot manage roles and in value add filter - use email and value = username (this is the measure we just created)   


### Incremental refresh
In power BI desktop go to table, then right click - Incremental refresh   
In power query we need to set up parameters - Power query - New parameters   
rangestart and rangeend    
Type - date/time   
current value = give a date    
-- in your table you go to that table in power query and search for date column then right click   
-- filter date/time - custom filter - equals - now select parameter - and the parameter name   
In model view, select the table right click - incremental refresh    
select the data and the time   

### Forecast
Line chart - then right click - Forecast   
we can go in format table and then format the forcast line colour   


### RUNNING TOTAL
Running Total =    
CALCULATE (   
    [Total Sales],   
    FILTER (   
        ALLSELECTED ( 'Date' ),   
        'Date'[Date] <= MAX ( 'Date'[Date] )    
    )   
)   

OR     
right click thable - new quick measure- running total    
base value - numerical   



### ALL and ALLSELECTED 
in Power BI remove filters to calculate totals, typically for percentages.   
Use ALL to ignore all filters (e.g., grand total of all time/regions). Use ALLSELECTED to ignore    
only filters inside the visual but respect external filters like slicers (e.g., total of only the selected year).     





















































































































































