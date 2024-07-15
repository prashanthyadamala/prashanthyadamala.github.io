---
title: "Retention Rate of Customers using Cohort Analysis"
excerpt: "This project aims to find the retention rate of customers of the Adventure Works store using a cohort analysis. 
This will give a better understanding of how long customers stick around to explore new ways of engaging customers to keep them longer. <br/>
<img src='/images/Cohort-Main.png' width='800' height='480'>"
collection: portfolio
---

Introduction
------
This project aims to find the retention rate of customers of the Adventure Works store using a cohort analysis. This will give a better understanding of how long customers stick around to explore new ways of engaging customers to keep them longer. The project also aims to find the breakdown of engagement and revenue contribution of new vs old customers over time. SQL Server will be used to store the data. The data will be cleaned, formatted and transformed using Power Query and visualized through a Power BI report.

Data Collection
------
The data for this project is based on a fictional bicycle store and has been used to practise my skills. The Adventure Works database is vast and hence for this project only the SalesOrderHeader table was selected. The Customer ID and the Order Date columns are the only ones that are required for this project as both exist within the imported table. 

Data Cleaning and Transformation
------
As mentioned in the data collected only the Customer ID and the Order Date columns are required, the rest of the columns are deleted in Power Query before loading the table. The table was renamed as the ’Cohort Table’. Another crucial column that is required is the ‘First Order Month’ column which is the month and year when the customer placed their first order. This can be found by using the Customer ID and order date to find the earliest order date for each customer. The following DAX code was used to calculate the column:

FirstOrderMonth =<br/>
&emsp;var CurrentCustomer = 'Cohort Table'[CustomerID]<br/>
<br/>
return<br/>
&emsp;  CALCULATE(EOMONTH(MIN('Cohort Table'[OrderDate]), 0),<br/>
&emsp;&emsp;    FILTER(<br/>
&emsp;&emsp;&emsp;        'Cohort Table',<br/>
&emsp;&emsp;&emsp;        CurrentCustomer = 'Cohort Table'[CustomerID]<br/>
&emsp;&emsp;    )<br/>
&emsp;)<br/>
<br/>

A date table was created using DAX’s CALENDER function to encompass all the dates present in the Cohort Table. The year, quarter, month name and month number columns were added as well.

Another table was created with a single column named Months which contained the number going from 0 to 12 in increments of 1. This will be used in the cohort analysis to signify the months after the first order month.

Data Modeling
------
A relationship was created between the Cohort Table and the Date table on the dates column for each of the tables with a many-to-one cardinality and single cross-filter direction. The simple data model is shown below:

<img src='/images/Cohort-Model.png' width='800' height='480'>


Data Visualization and Analysis
------
The data was then aggregated and visualized using DAX measures and charts in the form of a professional Power BI report that can be seen below:

<iframe title="Adventure_Works_Cohort_Analysis" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiNTAyM2M4ZmUtODQ2OC00NzY2LWI5MjQtMTg5ODgwZmQ0NzE0IiwidCI6ImE4ZTcxNmQwLWE5ZDItNGIyYi1iMWUyLTM3MTE1MDVmZWIyZSJ9" frameborder="1" allowFullScreen="true"></iframe>

The main visualization of the report is the matrix visualization that contains the cohort analysis of customer retention in the 12 months after their first purchase. It is constructed by:
- The ‘first order month’ in the rows section. 
- The series generates a month column (0 -12) in the columns section.
- The measure to calculate the number of first-time or returning customers in the values section.

The measure used to calculate the values of the first-time or returning customers in the values section is given below:

Cohort Customer Numbers =<br/>
&emsp;    VAR MonthAfterJoining = SELECTEDVALUE(Months[Value])<br/>
&emsp;    VAR MonthOfJoining = SELECTEDVALUE('Cohort Table'[FirstOrderMonth])<br/>
<br/>
&emsp;    RETURN<br/>
&emsp;&emsp;    CALCULATE(DISTINCTCOUNT('Cohort Table'[CustomerID]),<br/>
&emsp;&emsp;&emsp;        EOMONTH('Date'[Date], 0) = EOMONTH(MonthOfJoining, MonthAfterJoining<br/>
&emsp;&emsp;&emsp;    )<br/>
&emsp;&emsp;    )<br/>
<br/>  

The two variables assigned at the start of the code are the row and column values of the matrix visualization. The rest of the code is used to calculate the distinct count of customers based on the values of the two variables.

An option to change the values of the matrix visualization to the percentage of returning customers can be toggled at the bottom right corner of the visualization.

It can be seen from the matrix visualization that the percentage and number of returning customers in the following months is a small proportion of first-time customers. This shows that there is a lot of customer churn 

The two visualizations on the right show the breakdown of the new versus the old (returning) customers. As can be seen at the start of the business there were a lot of new customers as time went on there was a gradual increase until a sharp climb (that could be attributed to something like the marketing campaign). The total number of customers per month continues to gradually rise (ignore the dip in last month as the complete month wasn’t recorded).

Recommendations
------
Include repairs and maintenance department so customers have a reason to come back to the store and thus view other products or upgrades.
Subscription-based model for the repairs or to rent out cycles.

Future Updates
------
Add another segment to the customer, the recovered customers. This is a customer that has purchased for the store for a set amount of time (i.e. 6 months) and has thus been recovered as a customer.

Add the breakdown of revenue generated by new customers and returning customers.
