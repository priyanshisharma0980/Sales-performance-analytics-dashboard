### Array is of same datatype
List - Mutable , starts with [],  list is technically heterogeneous        
When you modify a list, you change its contents directly. The location of the list in memory (its ID) remains exactly the same.      

Tuple - Immutable  ()  Tuple is technically heterogeneous too        
You can combine two tuples together     
Dictonary- {} - key value pairs - Mutable       


### df.describe() 
Generate summary statistics to help you quickly understand the distribution, central tendency, and dispersion of your dataset.    
has count, mean, mode, median

### df.shape
Used to calculate number of rows and columns (2 x 3)                

### df.size -
The total number of cells (rows × columns= 6)             

### df.info() 
method provides a concise summary of the DataFrame, including the column names, data types, and the number of non-null values in each column         

### how to handle missing values
df.dropna()         
Drop rows only if missing values are in specific columns         
df_cleaned_subset = df.dropna(subset=['Age', 'Salary'])        

Count missing values in each column -  print(df.isna().sum())            

Fill missing numeric values with the column mean-          
df['Age'] = df['Age'].fillna(df['Age'].mean())       

Forward fill (propagates the last valid value forward)      
df_filled = df.fillna(method='ffill')          

Linear Interpolation (estimates intermediate points based on data trends)          
df['Temperature'] = df['Temperature'].interpolate(method='linear')             


### DUPLICATE
To delete duplicate values from particular columns      
df.drop_duplicates(subset=['Name', 'Age'])        

print(df[df.duplicated()])      
It extracts and displays the actual rows that are duplicates, hiding the unique ones.       

df.drop_duplicates(inplace=True)      
It permanently deletes the duplicate rows directly from your original variable df.       

print(df.duplicated().sum())      
It calculates and prints the exact total number of duplicate rows in the dataset.

### LAMBDA function
lambda function is a small, anonymous function that is defined without a name using the lambda keyword.          
lambda arguments: expression          
lambda x: x * 2           

Eg- Clean text: Capitalize all names          
df['Name'] = df['Name'].apply(lambda x: x.capitalize())       

### Merge
Joining 2 tables based on common column     
df.merge() - innerjoin    
merged_df = df1.merge(df2, on='Emp_ID', how='inner')        
df.concat- outer join      
df.join - left join        


### Generators and decorators
generators handle memory-efficient data streaming, while decorators modify or extend the behavior of code without changing its source.     
Generator-  Processes data streams one item at a time without filling up RAM.
Uses the yield keyword to pause execution and save state.      
Used in Processing large files, infinite data streams, and data pipelines.      
When a generator yields a value, its execution state is paused and saved until the next value is requested.        

Decorator- used to extend or alter the behavior of a function or class without permanently modifying its source code.       
A decorator is a function that takes another function as an argument, adds some functionality to it, and returns a modified version of it        
Logging, authentication, execution timing, and caching.      
def my_decorator(func):        
    def wrapper():       

### MATPLOTLIB and Seaborn
Line Plot: plt.plot() — Connects data points with lines.     
Scatter Plot: plt.scatter() — Plots individual data points to look for relationships.       
Vertical Bar Chart: plt.bar() — Displays rectangular bars for categorical comparisons.     
Horizontal Bar Chart: plt.barh()        
plt.hist() — Bins data to show distribution frequencies.     
2D Histogram: plt.hist2d() — A binning frequency plot for two variables.     
Box and Whisker Plot: plt.boxplot() — Shows median, quartiles, and outliers.      
Violin Plot: plt.violinplot() — Displays the combination of a boxplot and a kernel density layout.      
Pie Chart: plt.pie()          


### Overfitting
occurs when a model learns the training data including its random noise and outliers, causing it to perform poorly on new, unseen data.          
Instead of recognizing the broad underlying pattern, the model essentially "memorizes" the specific dataset.       

     
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
Uses 0-based integer positions.     
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


Regression - used to predict one variable with respect another   

Regression with Excel - Add in - Tool PAK - Data Analysis  
Select whole table in input range   
in output range select any blank cell    
then Okay, it will give a score between -1 to +1    

or in excel we can use correl - =CORREL(array1, array2)

For Correlation-   
IN Power BI - Use scatter plot   
give x = product , y = sales   
Now HOME - New quick measure - Calculation select correlation coefficent   
select that measure and put in gauze chart   
create a new measure and write - max = 1  now in options of gauze chart in maximum value select this measure   

In regression we use LINEST function and create a DAX measure, and we calculate slope and intercept    
intercept + slope * column




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




### ML in Python project
K-Means clustering → “Which customers are similar to each other?”  - Unsupervised Machine Learning           
This is unsupervised learning because you don't have a pre-existing answer/label.              
    
Logistic Regression / Random Forest / XGBoost → “Will this customer perform a particular action?”      
This is supervised learning because you give the model historical examples where the outcome is already known.        


### What is RFM?
R = Recency   How recently did the customer purchase?             
F = Frequency  How often does the customer purchase?           
M = Monetary   How much money has the customer spent?         

### How do we calculate RFM in Python?
df["Revenue"] = df["Quantity"] * df["UnitPrice"]      
Then convert the date- df["InvoiceDate"] = pd.to_datetime(df["InvoiceDate"])         
Now determine the analysis date - analysis_date = df["InvoiceDate"].max() + pd.Timedelta(days=1)  - "How many days has it been since this customer's last      purchase?"      

Now calculate RFM-
rfm = df.groupby("CustomerID").agg(          
    Recency=("InvoiceDate",         
             lambda x: (analysis_date - x.max()).days),       

    Frequency=("InvoiceID", "nunique"),        

    Monetary=("Revenue", "sum")        
).reset_index()       

After doing this we get a table with columns - Customerid, recency, frequency and monetary


### Choosing K
KMeans(n_clusters=4)      
Two common approaches are:       
1. Elbow Method         
2. Silhouette Score
If you increase K, inertia will almost always decrease.

### What is K MEANS clustering
K-Means is an unsupervised clustering algorithm that partitions data into K clusters by assigning observations to the nearest centroid and iteratively     updating the centroids to minimize within-cluster squared distances.         

### How did you choose K?
I evaluated multiple values of K using the Elbow Method and Silhouette Score, then considered cluster interpretability from a business perspective.         

### What does inertia mean?
Inertia measures how close the customers are to the centroid of their cluster.     
Inertia is the sum of squared distances between observations and their assigned cluster centroids. Lower inertia indicates tighter clusters, although it      always tends to decrease as K increases.             

### How you used K means clustering
Engineered customer-level RFM features and applied K-Means clustering after feature scaling, using the Elbow Method and Silhouette Score to determine an         appropriate cluster count. Profiled resulting clusters based on Recency, Frequency, and Monetary behaviour to create actionable customer segments.    

### Silhouette Score asks:
"Are the customers within a cluster similar to each other and sufficiently different from customers in other clusters?"      
### Elbow asks:
How much does adding another cluster improve compactness?       


### What to say for K means
I used K-Means because the customer segments were not predefined. I wanted to discover natural groups based on customer behaviour rather than manually     defining the segments.      
"K-Means identified four behavioural clusters, which I then profiled using average RFM values and mapped to business segments such as High-Value, Loyal,     Potential, and At-Risk."    
































 



















