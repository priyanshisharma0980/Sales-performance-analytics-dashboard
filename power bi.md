### Objective SQL power BI
Developed an enterprise-level Sales Analytics Dashboard to monitor business KPIs, regional sales performance, profitability trends, customer behavior,   
and operational efficiency across multiple business units.   
The dashboard analyzed ₹30Cr+ revenue data and supported management reporting and strategic decision-making.     

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
in Power BI is a powerful DAX mechanism where an active row context (evaluating data row-by-row) is converted into an    
equivalent filter context (applying those row values as filters to your data model)     
Can be achieved using - Placing CALCULATE function inside a row context.













































