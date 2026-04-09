### Array is of same datatype
List - Mutable   
Tuple - Immutable   

Numpy is better than list for numerical calculations   
x = np.array(list1)
type(x) - to check the type it will give numpy.ndarray    

x.shape(x) OR np.shape(x)     
Reverse an array arr1[::-1]    
For Slicing 2 D array  -  
arr2d[1:,2:]   

Reshape   
arr1.reshape(2,5)    

FLATTEN/ ravel       
np.ndarray.flatten(arr2d)    
np.ravel(arr2d)   

### Concat 2d array  
axis 0 = Rows
axis 1 = columns

np.concatenate((arr1, arr2), axis=0)


### usecols = used to specify the columns we want to read
df = pd.read_csv('data.csv', usecols=['team', 'rebounds'])    

np.count_nonzero(df)   

you may need to use a raw string by prepending an r (e.g., r"C:\path\to\file.csv") to avoid Python interpreting backslashes as escape characters.    
Using forward slashes works on all operating systems.     


### PANDAS
has 2 types of data   
1. Series - Stores in form of index and data
2. Datafaream - in form of columns and rows

series1 = pd.Series(data=data_list, index=index_list)      
Series must start with uppercase S   
if we want the data pass the key series1[[1,2,3]]    

### loc and iloc
loc - used to retrieve data based on rows and columns   
iloc - used to retrieve data based on rows and columns based on INDEX   (columns as index)    

head and tail() - default display value is 5   

### df.dtypes - returns datatype of each columns    
df['serial_num].astype(float) - astype converts datatype    

### drop
df.drop(column='serial_num', inplace='True')    

### datetime
df['euro_dates'] = pd.to_datetime(df['date_strings'], format='%d/%m/%Y')     

### unique
df['country'].unique()   

### Group by
df.groupby('Category')['Sales'].sum()     

### LAMBDA 
numbers = [1, 2, 3, 4]     
squared = list(map(lambda x: x**2, numbers))    
 Lambdas allow you to write simple functions in a single line, reducing the number of lines of code compared to a full def function definition.    



 ### PIVOT TABLE
 data: The DataFrame you want to use.    
values: The column(s) you want to aggregate. This can be a single column name or a list.   
index: The column(s) whose unique values will become the row labels of the new table.   
columns: The column(s) whose unique values will become the new column headers.    
aggfunc: The function used for aggregation (e.g., 'mean', 'sum', 'count', 'min', 'max'). By default, it uses the mean. You can pass a single function,     
a list of functions, or a dictionary to apply different functions to different value columns   

pivot_df = df.pivot_table(values='Temp', index='Date', columns='City', aggfunc='mean')    


### df.isnull()
checkes the values that are NULL.    
If not null returns FALSE    


df.dropna(axis=0) -- will drop from rows     

df.fillna('UNKNOWN')    

### Fill column 'A' NaNs with 0, and column 'B' NaNs with 'missing'
df_col_specific = df.fillna({'A': 0, 'B': 'missing'})    

### ffill
df_filled = df.ffill()    
The missing values are filled with the value from the immediately preceding row in the same column.    


### Window function
rolling - provide a rolling option where you can decide window size(period) and apply various aggregation functions like mean sum count    
used to perform moving window calculations on sequential data, such as time series    
rolling_mean = df['value'].rolling(window=3).mean()     

expanding_sum = df['value'].expanding().sum()     
it will return the sum of each value and sum of values above it    


### Vectorization
s=pd.Series([cat,mat,none, rat])   
s.str.startswith('c') all values except cat will be FALSE and none    
srt is string accessor   

String functions   
upper(), lower(), Capitalize(), max(), len(), strip() - removes spaces form both end     

Eg- if we have a full name seeprated with , and want first and last name  
df['firstname']= df['name'].str.split(',').str.get(0)   


### Timestamp
particular moment of time 20 july 2022 10 AM  

pd.timestamp('2023/01/09')   


pd.date_range(start= , end=, freq = W-THU) this will give a column of the dates between start and end date   

to get year with to_datetime   
pd.to_datetime(df).dt.year



--------------------------------------------------------------------------------------------------------------------------------------------------------     


### Regression and Correlation  
Correlation in data analysis is a statistical technique used to measure the strength and direction of a relationship between two quantitative variables.     Ranging from -1 to +1, a coefficient of +1 indicates a perfect positive correlation, -1 a perfect negative correlation, and 0 no correlation.    

The measure of correlation is calculated by corelation coefficent     
+ve correlation - x increase with y, x decrease with y   (eg - price vs demand)    
-ve correlation - x increases y decreases, x decreases and y increases    


Regression - used to predict one variable with another   





























 



















