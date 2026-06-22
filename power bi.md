### Objective SQL power BI
Developed an enterprise-level Sales Analytics Dashboard to monitor business KPIs, regional sales performance, profitability trends, customer behavior,   
and operational efficiency across multiple business units.   
The dashboard analyzed ₹30Cr+ revenue data and supported management reporting and strategic decision-making. 

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



### Revenue Growth % =   
DIVIDE(   
    [Current Revenue] - [Previous Revenue],   
    [Previous Revenue]    
)   

### Business Insights Delivered
Identified low-performing regions   
Detected high-return product categories   
Improved inventory planning   
Reduced reporting turnaround time   
Improved quarterly sales efficiency by 12%     

### CAlculated table/ Calculated column/ measure
dimdate= calendar(startdate, enddate)   - this will create calculated table     
calculated column -  a new column that you add to an existing table and it is calculated row by row   


### CALCULATE
evaluates a Data Analysis Expressions (DAX) expression under a modified filter context    
only function that can dynamically change, add, or override the data filters active in your report.     
CALCULATE(expression, filter1, filter2, ... )     
eg-
West Sales =    
CALCULATE(    
    SUM(Sales[Amount]),     
    Sales[Region] = "West"    
)    

### Overwrite with CALCULATE
If a user selects a specific year (like 2025) on a page slicer, but your formula explicitly specifies a different year, CALCULATE will override the slicer    election for that specific measure.     

### Filter Context
Filter Context is the set of filters that determines which rows are visible to a DAX calculation.    
Filters the entire dataset to a subset of data.    
Slicers, report filters, and visuals (charts, tables).    
Filter context is the set of filters applied to your data model before a DAX calculation is executed.    

### Row Context 
Row context in Power BI is a DAX concept that tells the formula to evaluate data one individual row at a time.      
When you create a Calculated Column, Power BI automatically creates a row context     
Used with iterator functions - SUMX, AVERAGEX, MAXX    

### Context transition 
When you use the CALCULATE function inside a calculated column, it triggers a context transition. This automatically    
transforms the row context into a filter context.    
in Power BI is a powerful DAX mechanism where an active row context (evaluating data row-by-row) is converted into an     
equivalent filter context (applying those row values as filters to your data model)     
Can be achieved using - Placing CALCULATE function inside a row context.

### SELECTEDVALUE(Product[ProductName]) 
returns a value only when exactly one product is selected. If two products are selected, it returns BLANK() unless an alternate     result is provided.     
SELECTEDVALUE( ColumnName, [AlternateResult] )     
Eg- Dynamic Titles: Change a chart title based on a user's slicer selection.     
Eg- Selected Year Title = SELECTEDVALUE('Date'[Year], "All Years Selected")     
Result A (if 2026 is selected in the slicer): "2026"     
Result B (if nothing or multiple years are selected): "All Years Selected"     


### Power BI Premium Per User (PPU), the maximum dataset size is 100 GB    
### Power BI Premium, the maximum dataset size is 10 GB     
### Power BI Pro: Maximum of 1 GB per dataset.     

### Row-Level Security (RLS) does in Power BI. 
It restricts which rows of data each user can see.     
Go to the Modeling tab and select Manage roles.    
Static RLS: Hardcoded rules (e.g., [Country] = "Germany").     
Dynamic RLS: Uses DAX functions like USERPRINCIPALNAME() to automatically filter data based on the logged-in user's email.     
RLS only applies to users with Viewer permissions.        
[EmployeeEmail] = USERPRINCIPALNAME()    

### 4 types of users-
Admin     
Member   
Contributor   
Viewer    

### Cardinaltiy
Relationships - 1 to many, 1-1, many to many    
Less unique values - less unique values   
Lower cardinality more optimisation     
Cardinality for optimisation -     
Low cardinality is the foundation of Power BI performance optimization. Keeping cardinality low allows Power BI's underlying engine   to compress data incredibly well, reducing the file size and accelerating report interactions   


### What would happen if your dataset grows from 500K rows to 50 million?
Use incremental refresh - Do not reload the entire dataset every time. Load only new or changed data.    
Instead of storing every transaction for reporting, we create summarized data. Eg - for each category grup it and then show the toal revenue instaed of     
individual Indexes help SQL Server find rows faster.     

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


### DATA/OUTPUT Mismatch 
we will check number of rows and their output in SQL then in power BI check with DAX     
Total Rows = COUNTROWS(Sales)      

Compare Record Counts at Every Stage, This is exactly what senior BI developers do- Create validation table.     
So a good power BI practise I we create a staging layer data (we duplicate it in power query and do transformations in power query)    

Check for many to many relationships     
Validate DAX measures    
Check filters     
Check the data types      
Reconcile Using Sample Records - January Revenue is incorrect. so we cross check only for January data    

Experienced BI teams maintain validation KPIs.    


### 3 types of Refresh Types in Power BI
Dataset refresh    
Visual refresh -- This happens automatically when a user interacts with filters or slicers.      
Increamental refresh -- Only new or updated data is loaded instead of refreshing the entire dataset.    

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

how did you load such big data into power bi with respect to this huge dataset?    
Instead of loading a long query directly into Power BI, I created a SQL View.    
To improve performance, I removed unnecessary columns.    
For example, raw tables might contain:   
internal system IDs   
audit columns    
metadata fields    

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

### 

### ALL()
Used inside caluclate, used to bypass filters and slicers    
Grand Total Sales =     
CALCULATE(     
    [Total Sales],     
    ALL(Products) // Clears all filters applied to the Products table     
)    


### datesinperiod
DATESINPERIOD(<dates>, <start_date>, <number_of_intervals>, <interval>)     

3 months sales-   
Last 3 Months Sales =     
CALCULATE (   
    [Total Sales],    
    DATESINPERIOD (    
        'Date'[Date],    
        MAX ( 'Date'[Date] ),    
        -3,   
        MONTH   
    )    
)    

### count(*)
COUNT(*) counts all rows     
COUNT(column_name) ignores NULL values    

### Merge and Append
Merge adds columns horizontally by matching rows based on a unique identifier,      
while Append adds rows vertically by stacking datasets with identical or similar column structures    
APPEND - Similar or identical schema / column headers    
MERGE- A matching "key" column in both tables    

### ALLEXCEPT: 
If your table has a Primary Key / Unique ID column, use ALLEXCEPT to strip out all filters except for that unique identifier.    




































 












































