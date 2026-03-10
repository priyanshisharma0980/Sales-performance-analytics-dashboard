### Find which customers spend the mostand understand their behavior
We want to understand customer purchasing behavior
“We will analyze transaction data and segment customers.”

Who are our high-value customers?
Which customers purchase frequently?

### METRICS
| Metric              | Description                    |
| ------------------- | ------------------------------ |
| Total Spend         | Total money spent per customer |
| Purchase Frequency  | Number of orders               |
| Average Order Value | Spend per order                |


TABLES
| Column           | Description        |
| ---------------- | ------------------ |
| customer_id      | unique user        |
| order_id         | order number       |
| product_category | product type       |
| order_value      | transaction amount |
| purchase_date    | purchase time      |


### Setup a connection between SQL server and Python
import pandas as pd
import pyodbc
conn = pyodbc.connect(
    "DRIVER={SQL Server};"
    "SERVER=companyserver.database.windows.net;"
    "DATABASE=salesdb;"
    "UID=username;"
    "PWD=password"
)

df = pd.read_sql(query, conn)

### Understand the data
df.shape    --- gives rows and columns
df.info()   
It gives column names
data types
missing values


### Fixing Data Types
df['purchase_date'] = pd.to_datetime(df['purchase_date'])


### REMOVE DUPLICATES
df = df.drop_duplicates()

### Handling Missing Data
Option 1 remove rows
df = df.dropna()

Option 2 fill values
df['price'] = df['price'].fillna(df['price'].median())


### Creating Business Metrics
Total spending per customer
customer_spend = df.groupby('customer_id')['price'].sum()


### Creating Customer Behavior Features
frequency = df.groupby('customer_id')['order_id'].count()

### HEAT MAPS CORR
Do frequent buyers spend more money?
sns.heatmap(df.corr())

### Identifying High Value Customers

Segment	Spend
High value	>10000
Medium	3000–10000
Low	<3000

df['segment'] = pd.cut(
df['total_spend'],
bins=[0,3000,10000,50000],
labels=['Low','Medium','High']
)

Count High Value Customers
high_value_customers = customer_data[customer_data['segment']=='High']

Calculate Revenue From High Value Customers
high_value_revenue = high_value_customers['total_spend'].sum()

### Most purchases between ₹500–₹3000
import seaborn as sns
import matplotlib.pyplot as plt

sns.histplot(df['order_value'])
plt.show()


### Revenue by product category
category_sales = df.groupby('product_category')['order_value'].sum()
















































