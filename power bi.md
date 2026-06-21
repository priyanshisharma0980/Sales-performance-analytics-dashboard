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




































 












































