---
title: "Portfolio item number 4"
excerpt: "This project aims to conduct an exploratory analysis of a hotel and resort chain dataset. SQL Server will be used to store, clean and transform the data. 
The data will be further cleaned and transformed using Power Query and visualized through a Power BI report. <br/>
<img src='/images/Hotel-Main-2.png' width='800' height='480' frameborder='2'>"
collection: portfolio
---

Introduction
------
This project aims to conduct an exploratory analysis of a hotel and resort chain dataset. SQL Server will be used to store, clean and transform the data. The data will be further cleaned and transformed using Power Query and visualized through a Power BI report. This project will analyse the revenue, reservations, seasonality of the data and customers of the hotel and resort chain.

Data Collection
------
The data for this project is based on a fictional hotel and resort chain. The data that will be used for this project is found in three tables, one for each calendar year [1]. A table with a list of all the countries in the world along with their 3 letters' alphabetical codes from iban.com was also used to complete the data [2]. 

Data Cleaning and Transformation
------
SQL Server:

The data cleaning was done using both SQL Server and Power BI’s Power Query. The steps taken in each of the tools are given below:

- Append the three tables (each for a different year) into one table. 
  - Some of the data types had to be changed to accommodate the append. 
  - The column names and data types were cleaned and matched in all three to perform the append. 
  - The order of the columns was also maintained in all three tables. 
- Added Index column (order id) to have a primary key for the fact table. 
- Replaced ‘NULL’ values with 0 from the following columns:
  - Children
  - Babies
  - Company
  - Agent
- Cleaned the ‘reservation_status_dates’ column (some values had dates that were outside the time range, thus they were converted to dates of the year of the dataset as this was an error due to a bug.
- Values in the child and babies column which had a high number of children were replaced after finding out that they were typos so “10” was converted to 1 and 9 to 0 due to the proximity of the keys to each other.
- Could add a country dimension to have full forms of the names that exist in the factsheet.

Power BI:

Data Extraction:
- Connected to SQL server database where the Hotel database is located. 
- Connected to the web to get a list of names of counties along with their Alpha-3 code. This is to analyse the Hotel data based on the country locations. 

Data Cleaning:
- Changed the data types in the native query from float to integer to improve performance by decreasing query size. 
- Also the fact table by Order No. in the native query in ascending order.
- Removed columns that were not necessary for the analysis.
- Changed the column names to a more readable format.
- Importing the dimensions tables Market Segment and Meal Cost.
- Changed the summarization setting to not summarize on columns that are order numbers or contain binary data. 

A date table was created using DAX’s CALENDER function to encompass all the dates present in the Cohort Table. The year, quarter, month name, month number, week number, weekday, day of week and day of month columns were added as well.

Data Modeling
------
The schema used for this dataset was the star schema. Data from the main hotel and resort table was normalized thus creating the ‘Market Segment’ and ‘Meal Cost’ tables as dimension tables. 

The Country table was added and connected to the Hotel table (Fact Table) on the 3-letter country codes. This is because the countries in the original table are only present as 3-letter alphabetical codes. 

The Date table’s Date column is connected to the Arrival Date column on the fact table. All the relationships to the fact table have a one-to-many cardinality and a single cross-filter direction. 

<img src='/images/Model_Hotel_Data.png' width='800' height='480'>

Data Visualization and Analysis
------
For the Power BI report of the hotel and resort data analysis, four pages were modelled for each of the following four analyses:
- Revenue
- Reservations
- Seasonality of the data
- Customers

Revenue Analysis

<iframe title="Hotel_Data_Analysis_Project" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiYzU5N2IzZTYtZjZiMy00NjVlLWIwNTMtMWNhNDA2NTQ4Y2Q5IiwidCI6ImE4ZTcxNmQwLWE5ZDItNGIyYi1iMWUyLTM3MTE1MDVmZWIyZSJ9&pageName=ReportSectionabdde79d77ca07a7c59d" frameborder="1" allowFullScreen="true"></iframe>

The revenue for the hotel and resort chain comes mainly from two areas, hotel reservations and food. The revenue from the hotel reservations is calculated based on the factors below along with the measure to calculate it:
The ‘average daily rate’ (agr) for each reservation, thus the customer is charged ‘agr’ times the number of days they stay.
The ‘agr’ is the same whether it is a weekend or a weekday.
Revenue is only generated if the reservation is not cancelled.
The total reservation billing is discounted by the discount associated with the market segment.

Hotel Booking Revenue = <br/>
&emsp;	CALCULATE(<br/>
&emsp;&emsp;		SUMX('Hotel', 'Hotel'[Average Daily Rate]*<br/>
&emsp;&emsp;&emsp;('Hotel'[Week   Nights]+'Hotel'[Weekend Nights])*<br/>
&emsp;&emsp;&emsp;(1 - RELATED('Market Segment'[Discount]))), <br/>
&emsp;&emsp;&emsp;'Hotel'[Order Status Cancelled] = "0"<br/>
&emsp;)<br/>
<br/>

The meal revenue, on the other hand, isn't given directly, there is information on the meal type chosen for the stay and the price of the meal. Some assumptions about how the billing is done for each customer are given below:
The meal price given is the data per person per day.
Adults and children on the reservation are charged the same price per day. (This can be changed in the future to be a fraction of the full price for children.)
Babies are not charged.
Revenue is only calculated on non-cancelled reservations.

The measure used to calculate the meal revenue is given below:

Meal Revenue = <br/>
&emsp;CALCULATE(<br/>
&emsp;&emsp;SUMX('Hotel', ('Hotel'[Adults] + Hotel[Children])*<br/>
&emsp;&emsp;&emsp;('Hotel'[Week Nights]+'Hotel'[Weekend Nights])*<br/>
&emsp;&emsp;&emsp;RELATED('Meal Cost'[Cost])), 'Hotel'[Order Status Cancelled] = "0"<br/>
&emsp;)<br/>
<br/>

The top two visualisations compare the revenues generated by the reservations and meals from the hotel chain. In the line chart, we can see the comparison over time. The revenues from the reservations and meals follow similar variations but the reservations have steeper variations. It can be seen from the bar chart revenue from the hotel and resort reservations is over twice that of the meal revenue. 

The ribbon chart and bar chart and the bottom show the revenue compared to the previous year. It can be seen from the bar chart that the revenue in the current year is about 30% higher than the previous year. The ribbon chart confirms that most of the year the current year outperforms the previous year but the months of July and August. This can be attributed to the rise of COVID-19 which hit the hospitality industry very hard in mid-2020. The measure used to calculate the previous revenue is shown below:

Revenue Previous Year = <br/>
&emsp;	CALCULATE(Hotel[Total Revenue], SAMEPERIODLASTYEAR('Date'[Date]))<br/>
<br/>

Reservations

<iframe title="Hotel_Data_Analysis_Project" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiYzU5N2IzZTYtZjZiMy00NjVlLWIwNTMtMWNhNDA2NTQ4Y2Q5IiwidCI6ImE4ZTcxNmQwLWE5ZDItNGIyYi1iMWUyLTM3MTE1MDVmZWIyZSJ9&pageName=ReportSection" frameborder="1" allowFullScreen="true"></iframe>

The page analysed the data of the reservations based on the reservation status (successful / cancelled), customer type, market segment and hotel type. 
The top two charts show the comparison of the reservations based on their status. It can be seen that 37.22% of all the orders are cancelled. The filled area chart shows the distribution of successful vs cancelled over time. They follow a similar pattern of ups and downs.

The Customer Type doughnut chart, market segment treemap, and the Hotel Type Donut chart are self-explanatory. 

Seasonality of the data

<iframe title="Hotel_Data_Analysis_Project" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiYzU5N2IzZTYtZjZiMy00NjVlLWIwNTMtMWNhNDA2NTQ4Y2Q5IiwidCI6ImE4ZTcxNmQwLWE5ZDItNGIyYi1iMWUyLTM3MTE1MDVmZWIyZSJ9&pageName=ReportSection5f2343edd550c445e261" frameborder="1" allowFullScreen="true"></iframe>

The trends of the reservations over time can be seen on this report page. It can be seen in the bar chart and the line chart that reservations are based on the season. It is highest in the summer and early autumn months with the highest being in August. The reservations by weekday shows that reservations are many mostly for the weekdays.

Note: Change the Reservations by year to something different as it is misleading since the years 2018 and 2020 aren’t complete. 


Customers

<iframe title="Hotel_Data_Analysis_Project" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiYzU5N2IzZTYtZjZiMy00NjVlLWIwNTMtMWNhNDA2NTQ4Y2Q5IiwidCI6ImE4ZTcxNmQwLWE5ZDItNGIyYi1iMWUyLTM3MTE1MDVmZWIyZSJ9&pageName=ReportSectionc0ded03944da80eb1685" frameborder="1" allowFullScreen="true"></iframe>

This report page analyses the customer data of the dataset. There is a huge limitation with the dataset as there are no Customer ID columns. This limits the scope of the customer analysis. However, there is data regarding whether a customer is new or returning. With these limitations in mind, the analysis is conducted based on the reservations made.

The ‘New and Returning Customers by Month-Year’, the clustered bar chart shows the contrast of these two types of customers. It can be seen that a majority of reservations are made by new customers through all the months that data has been collected. 

The pie chart shows that a majority of the reservations are made by customers with no children. Measures could be taken to attract customers with children. The customers with and without children were calculated using the two measures below:

Customers with Children =<br/>
&emsp;CALCULATE(<br/>
&emsp;&emsp;COUNT(Hotel[Order No.]), <br/>
&emsp;&emsp;&emsp;OR(Hotel[Babies] > 0, Hotel[Children] > 0)<br/>
&emsp;)<br/>
<br/>

Customers without Children =<br/>
&emsp;CALCULATE(<br/>
&emsp;&emsp;COUNT(Hotel[Order No.]), <br/>
&emsp;&emsp;&emsp;AND(Hotel[Babies] = 0, Hotel[Children] = 0)<br/>
&emsp;)<br/>
<br/>

The map visualization shows the countries where the customers are with the majority of them coming from Western Europe or Brazil. The Donut chart shows the fraction of customers who do not get the room they booked. The map visualization cannot be seen in the embedded Power BI report above thus an image of the map visual can be seen below:

<img src='/images/hotel-map.png' width='800' height='480'> 

Limitations of the Data
------
One of the biggest limitations of the data is that reservations are not linked to a Customer ID, this decreases the scope of the customer analysis that can be conducted.

Recommendations for the Hotel and Resort Chain:
------
Data related
- Add a Customer ID linked to each reservation. The personal information can be protected by only providing data with the ID numbers.
- Have a main fact table that contains a row for each day that a room is occupied with their respective reservation number and customer ID.
- Add a premade list of choosable options when customers choose to cancel a reservation. This can give insights into improvements that can be made to decrease future cancellations.

Hotel-related
- Provide incentives for customers to return as it is seen that most of the reservations are made by new customers. It is a huge missed opportunity if this is not capitalized upon.
- Promotions for periods of low reservation rates by offering discounted rates. 
- The percentage of customers with children accounts for only about 8% of all the reservations. This can be a market to explore.
- Provide information on the capacity of each room type to check the fill rates over time. 

References:
------
1. List of country codes by alpha-2, alpha-3 code (ISO 3166). (n.d.). https://www.iban.com/country-codes
2. Holland, G. (2023, December 29). Where to find data? AbsentData. https://absentdata.com/data-analysis/where-to-find-data/
