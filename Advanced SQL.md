### Window functions   
row_number() over()   
rank() - skips the numbering   
dense_rank() - sticks to the ranking 1 then 2 then 3     

### Order of execution of query in SQL
from       -- chose the table  
Join       
where      -- filters the base data       
group by   -- aggreates the base data   
having     -- filters the data   
select     -- returns the final data     
Distinct      
order by   -- sorts the final data  
limit      -- returns the limit of data   


### how many categories have max count 15
select count(category) from    
(select category, max (price) as max_price from table    
group by category) AS mp   
where max_price < 15;   


### write this as CTE
with mp as(select category, max (price) as max_price from table    
group by category)   

select count(*) from mp    

### CTEs are more powerful than subqueries

### Having and Group by
having comes right after group by      
filter at aggreagtion level    

where - when there are conditions  
used before group by    

Eg - there are customers A and B, with multiple sales
👉 Show customers whose total sales > 200 --- we use HAVING (need to aggregate for total sales)   
👉 Filter rows where Amount > 100  --------- where      


### which is faster having or where?
WHERE


### Partition in SQL
 dividing large tables into smaller, more manageable segments called partitions.    
 It doesn’t magically “make queries faster” on its own; it works by reducing how much data the database needs to touch.     
 splitting one big table into smaller pieces (partitions)…but still treating it like one table logically     
 
 1. PArtition function - based on partition key eg- DATE    
 Create partiton function parititonbyyear(date)     
 AS range LEFT for values('2022-12-31', '2023-12-31')    

 2. FILE Group -
It creates files for each different year to store them seperately    
add the file groups    
alter database databasename ADD filegroup year_2024;   
    
TYPES -   
1. Range - based on date    
2. LIST - based on region/country (specific values)   
3. Hash - divides data equally

Partition Pruning -	Reads only relevant partitions         
Parallel Processing -	Multiple partitions processed simultaneously     

Does Partitioning Take More Space?   
👉 Slightly yes   
Metadata per partition    

### Partition pruning 
is a database performance optimization technique that boosts query speed by scanning only relevant data partitions, skipping unnecessary ones.    

### What is sliding window partitioning?
👉 Used in real production systems  
Add new partition monthly   
Drop old partition   


### Indexing?
👉 Index = a separate data structure that helps the database find rows quickly without scanning the whole table    
B-TREE Structure:   
Root node → entry point   
Intermediate nodes → navigation    
Leaf nodes → actual data or pointers    

Types of Indexes (Core)-   
Clustered Index	- Data stored in sorted order   
Non-Clustered Index	- Separate structure with pointers    


Clustered vs Non-Clustered Index (Deep Comparison)   
Clustered Index = actual table data is stored in sorted order   
Non-Clustered Index = separate structure → points to data    
 

Difference between scan and seek?
Seek = direct access
Scan = sequential read       

### INDEXing takes more space than partitioning   

### What is index fragmentation?
👉 Index becomes disorganized → slower performance   

### Is indexing automatically created on primary key? Why?
👉 Yes (in most DBs like SQL Server)    
Primary key must be unique + fast lookup      
DB creates clustered index by default (unless specified otherwise)    



### What is an Execution Plan 
👉 Execution plan = the exact strategy the database chooses to run your query   
which index, which join, how much data, in what order, and at what cost   
EXECUTION PLAN -   
When query runs the execution plan decides if we do full table scans or check for indexes   
then it checks if joins are possible or not    
and then retirve data for select statement   
and stores the result of execution plan to CACHE   

Reads data from right to left    
Now use TABLE SCAN click click - Properties    
check number of rows that are read   
An index improves query speed.    
CREATE INDEX idx_orderid    
ON EmpSalary1(salary);    
EXEC sp_helpindex 'EmpSalary1';     


### JOINS
1. INNER JOIN   
👉 Returns only matching rows    

2. LEFT JOIN (LEFT OUTER JOIN)   
✔ All rows from LEFT table   
✔ Matching from RIGHT   
✔ Non-matching → NULL

3. RIGHT JOIN   
✔ All rows from RIGHT table   
✔ Matching from LEFT    
✔ Non-matching → NULL

FULL OUTER JOIN  
👉 Combines LEFT + RIGHT   
✔ All rows from both tables   
✔ Non-matching → NULL   


### DATA DEDUPLICATION
remove duplicates using-
row_number   
having count(*)>1   
select DISTINCT   
Intermediate Table (CTAS Pattern)- 
For massive tables, performing an "in-place" delete can be slow and lock the table.   
Instead, use the Create Table As Select (CTAS) pattern:Create a new table with unique rows using DISTINCT or ROW_NUMBER().    
Drop the original table.Rename the new table to the original name.   



### What are LAG and LEAD?
These are window functions   
👉 LAG() → gets value from previous row   
👉 LEAD() → gets value from next row    

SELECT   
    Date,   
    Sales,   
    LAG(Sales) OVER (ORDER BY Date) AS Previous_Day_Sales    
FROM Sales;    


### SQL SERVER AGENT
in sql server - got o sql server agents - jobs - new jobs



### B. COUNT(*) counts all rows including NULLs, while COUNT(column_name) only counts non-NULL values in that column.  


### Find Emp without any projects in 2 tables, emp and project
select e.emp_id, e.emp_name      
from emp e left join project p     
ON e.emp_id = p.emp_id     
where p.emp_id IS NULL      


### Second highest age
WITH ranked_salaries AS (           
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank             
    FROM employees          
)         
SELECT DISTINCT salary       
FROM ranked_salaries          
WHERE salary_rank = N;           


### Find Duplicate Records id in a Table
select id, count(*) as duplicate    
from employee    
group by id         
having count(*)>1;     

### Delete Duplicate Rows While Keeping One Copy
with cte as(    
select id, row_number() over(partition by id order by id) as rnk    
from employees)
delete from cte where rnk>1;     

### Find Employees Earning More Than Their Managers from details table, emp_name also contains manager names, then thereis emp_id and manager_id 
self join     
select e.emp as employee, m.emp as manager    
from employee e 
JOIN employee m ON e.managerid = m.empid      
where e.salary > m.salary;           





























































































