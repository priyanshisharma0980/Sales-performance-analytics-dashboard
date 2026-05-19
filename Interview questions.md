### what is partitioning and indexing and clustering   
### On which column partitioningis done - DATE column   
### What is output of left and inner join on these 2 tables   

### order for sql query execution

FROM / JOIN  
WHERE   
GROUP BY    
HAVING    
WINDOW FUNCTIONS    
SELECT    
DISTINCT
ORDER BY
LIMIT / OFFSET    


### How would you calculate a "Running Total" of revenue ordered by date?   
SUM(revenue) OVER(ORDER BY date)   

### COUNT(*) counts all rows including NULLs, while COUNT(column_name) only counts non-NULL values in that column.    

### Recursive CTE
A recursive Common Table Expression (CTE) is a special type of SQL query that references itself to handle data with hierarchical or repetitive relationships.    

Core Components-   
Anchor Member: The "seed" query that provides the initial base result set.    
Recursive Member: The part that references the CTE itself. It joins the previous results with the table to find the next "level" of data.    
Termination Condition: A WHERE clause or the point where the recursive member returns no more rows. This prevents infinite loops.     

When to Use It-      
Organizational Charts    
Bill of Materials     
Graph Traversal    
Generating Sequences    

### You are calculating the average of a column bonus. Some values are NULL. 
You want these NULL values to be treated as 0 so they lower the average. Which approach is correct?     
- AVG(COALESCE(bonus, 0))
Using COALESCE, you transform NULL into 0 before the AVG function processes the row, ensuring the denominator includes all rows.


### You want to calculate a running total of sales for each day. Which window function syntax correctly calculates a cumulative sum?    
SUM(amount) OVER (ORDER BY sale_date)    
When an ORDER BY is used inside OVER() without a frame specification, it defaults to ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW,     
which creates a running total.    

### What is the result of the following SQL statement?SELECT COALESCE(NULL, NULL, 'SQL', 'Python', NULL);   
'SQL'    
COALESCE evaluates the list of arguments from left to right and returns the first non-null value it encounters.     

###  List ALL employees, but only show department details for those in 'Accounting'.To achieve this, you must place the filter in the ON clause
SELECT    
    e.employee_name,     
    d.department_name   
FROM Employees e   
LEFT JOIN Departments d    
    ON e.dept_id = d.dept_id    
    AND d.department_name = 'Accounting';   


### difference between where and having clause in sql
The core difference between WHERE and HAVING in SQL is the stage at which they filter data: WHERE filters individual rows before data is grouped or aggregated,    while HAVING filters the summarized groups after a GROUP BY clause is applied.   


### Difference between DELETE, TRUNCATE, DROP
DELETE removes specific rows from a table using a condition, used with where         
TRUNCATE empties all rows while preserving the table structure,       
DROP completely deletes the entire table structure along with its data from the database.    
 
FASTEST - Truncate then drop then delete    

### JOINS - 6 types
INNER JOIN- Returns only the records that have matching values in both tables.   
LEFT JOIN- Returns all records from the left table and the matched records from the right table.    
RIGHT JOIN- Returns all records from the right table and the matched records from the left table.     
FULL JOIN- Returns all records when there is a match in either the left or the right table.    
CROSS JOIN- Returns the Cartesian product of the two tables (every possible combination of rows).    
SELF JOIN - A regular join, but the table is joined with itself.
Self join eg - In same table you need to find respective manager for each employee   
select * from table AS t1 join table AS t2 ON t1.emp_id = t2.manager_id     


### Group by
The SQL GROUP BY clause arranges identical data into groups, allowing you to perform calculations on each group using aggregate functions like COUNT(), SUM(),     AVG(), MAX(), or MIN().    

























