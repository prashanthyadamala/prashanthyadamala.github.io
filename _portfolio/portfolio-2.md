---
title: "Basket Analysis of Adventure Works"
excerpt: "This project aims to identify the items bought most frequently together at the Adventure Works store. <br/><img src='/images/Basket-Main.jpg' width='800' height='480' frameborder="2">"
collection: portfolio
---

Introduction
------
This project aims to identify the items bought most frequently together at the Adventure Works store. This will give a better understanding of what items to bundle together during promotional sales, item placement in stores, and item suggestions when an item is placed in the cart while shopping online. SQL Server will be used to store, clean and transform the data. The data will be further cleaned and transformed using Power Query and visualized through a Power BI report.

Data Collection
------
The data for this project is based on a fictional bicycle store and has been used to practise my skills. The Adventure Works database is vast and hence for this project only the SalesOrderDetail, Product, Sub-category and Category tables were selected. The Sub-category and Category tables are options and are only used as filters for the product. 

Data Cleaning and Transformation
------
SalesorderDetail and Product tables are imported into SQL Server and their column names are changed to improve readability. The rows that contain null values in the columns SalesOrderID column and the Product Name are removed.

The data is then imported into Power BI. In Power Query the SalesOrderDetail table, the SalesOrderID column and the Product Name are the only columns that are required. From the Product table, only the Name column is required. The remaining columns from each of the tables are deleted to decrease the size data and improve loading performance with the query folding feature.

To perform the market basket analysis, the data has to be transformed to have a table with two columns containing all possible combinations of the products. This is achieved using the following DAX code:

Basket Analysis =<br/>
FILTER(<br/>
&emsp;    CROSSJOIN(<br/>
&emsp;&emsp;        SELECTCOLUMNS(VALUES('Product'[Name]), "Product 1", 'Product'[Name]),<br/>
&emsp;&emsp;        SELECTCOLUMNS(VALUES('Product'[Name]), "Product 2", 'Product'[Name])<br/>
&emsp;        ), [Product 1] <> [Product 2]<br/>
)<br/>


After the two columns are generated using the Product table’s Name column, the next step is to create a column with a combination of both names as the primary key to this table. This is done using the following DAX code that just concatenated the two names: 

Basket Combination = 'Basket Analysis'[Product 1] & " - " & 'Basket Analysis'[Product 2]

The most important thing is to create the number of occasions that the two products are bought together. This is done using the following DAX code below. It creates two temporary tables for each row of products and then uses the INTERSECT function to find the number of orders where both products are bought together.

Prod 1&2 Purchases =<br/>
var prod1 = 'Basket Analysis'[Product 1]<br/>
var prod2 = 'Basket Analysis'[Product 2]<br/>
<br/>
var prod1Trasactions =<br/>
&emsp;    SELECTCOLUMNS(
&emsp;&emsp;        FILTER(SalesOrderDetail, SalesOrderDetail[Product Name] = prod1),<br/>
&emsp;&emsp;        "SalesOrderID", SalesOrderDetail[SalesOrderID]<br/>
&emsp;    )<br/>
<br/>
var prod2Trasactions =<br/>
&emsp;    SELECTCOLUMNS(<br/>
&emsp;&emsp;        FILTER(SalesOrderDetail, SalesOrderDetail[Product Name] = prod2),<br/>
&emsp;&emsp;        "SalesOrderID", SalesOrderDetail[SalesOrderID]<br/>
&emsp;   )<br/>
<br/>
var combinedTrasactions =<br/>
&emsp;    INTERSECT(prod1Trasactions, prod2Trasactions)<br/>
<br/>
RETURN<br/>
&emsp;    COALESCE(COUNTROWS(combinedTrasactions), 0)<br/>
<br/>

Data Modeling
------
A relationship was created between the SalesOrderDetail and the Product table on the product name of each of the tables with a many-to-one cardinality and single cross-filter direction. The Basket Analysis table doesn’t have any relationship with the rest of the tables. Its relationships are established using DAX code when creating it. The graphical representation of the data model is shown below:


<img src='/images/Basket-Analysis-Model.png' width='800' height='480'>


Data Visualization and Analysis
------
The data was then aggregated and visualized using DAX measures and charts in the form of a professional Power BI report that can be seen below:

<iframe title="AdventureWorks_Basket_Analysis" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiNDI2OTU2YTYtYzQyZS00ZjBlLWFmYjktYTQ4ZGM3ZTY0NDNmIiwidCI6ImE4ZTcxNmQwLWE5ZDItNGIyYi1iMWUyLTM3MTE1MDVmZWIyZSJ9" frameborder="1" allowFullScreen="true"></iframe>

The report shown has two main visualizations. The left visualization is a bar chart that shows the top 10 best-selling products at Adventure Works. It isn’t affected by any of the filters on the page.

The visualization on the right is a table visualization that shows the top 10 products that are bought together. Product 1 of the visualization can be controlled using the Products drop-down filter on the top right. Choosing a particular product on the filter will show the top 10 other products that are purchased along with it. The number of transactions in which these products are purchased together is shown in the ‘No. of Transactions’ column.

This helps in identifying which items to bundle together during promotional sales, item placement in stores, and item suggestions when an item is placed in the cart while shopping online. This information can be used by Adventure Works in both their retail and online stores to help boost sales.

Reference:
------
Loshin, D., & Reifer, A. (2013). Customer Data Analytics. In Elsevier eBooks (pp. 68–78). https://doi.org/10.1016/b978-0-12-410543-0.00009-3 (Image)



