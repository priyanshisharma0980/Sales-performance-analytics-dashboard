### Window functions   
row_number() over()   
rank() - skips the numbering   
dense_rank() - sticks to the ranking 1 then 2 then 3     

### Order of execution of query in SQL
from       -- chose the table   
where      -- filters the base data       
group by   -- aggreates the base data   
having     -- filters the data   
select     -- returns the final data  
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












































