In my previous project, the finance team had to prepare monthly financial reports for different departments like Marketing, 
Sales, HR, and Operations.
The data came from an ERP system export and was provided as Excel files every month. 
The finance team used to manually clean the data, combine multiple files, and create pivot tables to analyze 
department expenses and budgets.

### AUTOMATION
Power Query automatically:
imports all monthly transaction files from a folder
cleans and standardizes the data
removes duplicates
fixes department names
creates additional columns like month and year

### Who were the stakeholders for this report?
Department managers
Senior managment

### Data Used
| Column           | Meaning                          |
| ---------------- | -------------------------------- |
| Posting Date     | Date of transaction              |
| Department       | Which department spent the money |
| Expense Category | Type of expense                  |
| Amount           | Money spent                      |
| Cost Center      | Internal cost tracking ID        |



Each table had 10k around rows

### Pivot Tables used for
summarizing department expenses
analyzing category spending
monthly trend analysis

### Data Cleaning using Power Query
Import Data
Remove unnecessary columns
Standardize department names
Convert data types
Create calculated columns
Load Clean Data to Excel

### KPIs
KPI 1 — Total Expense
Formula:
=SUMIFS(Amount,TransactionType,"Expense")
KPI 2 — Budget vs Actual
Actual Spend = SUMIFS(Amount,Department,A2)
Budget = VLOOKUP(A2,BudgetTable,2,FALSE)
Variance = Budget - Actual
KPI 3 — Variance %
Variance % = (Actual - Budget)/Budget


### CHARTS
Department Spend Chart
Bar chart showing:
Department vs Expense
3 Budget vs Actual Chart
Column chart:
Budget vs Actual
4 Monthly Trend
Line chart:
Month vs Expense

Slicers added for:
Department
Month
Category


### Data we received
Finance Reports Folder

Jan_Transactions.xlsx
Feb_Transactions.xlsx
Mar_Transactions.xlsx
Budget.xlsx
Department_Mapping.xlsx


### SCHEDULE REFRESH in EXCEL
Steps
Go to Data tab
Click Queries & Connections
Right-click your query
Click Properties
Enable:
✅ Refresh data when opening the file


















