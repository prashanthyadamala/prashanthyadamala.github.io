---
title: "Analysis of LinkedIn Data Analysis Job Postings in Australia (Jun 2024 - Present)"
excerpt: "Analysed 400+ LinkedIn data analyst job postings in Australia using Python to collect data via web-scraping and Power BI visualizations to provide insights into the current job market.<br/><img src='/images/LinkedIn-image-map.png' width='800' height='480'>"
collection: portfolio
---

Introduction
------
The goal of this project is the analyse the data analyst job postings in Power BI to help me better understand the opportunities available to me during my job hunt. This project would give me an overview of the jobs in a Power BI report as well as narrow down jobs that I want to apply for immediately using filters. The data used in this project was scrapped from LinkedIn Australia  using Python in Jupyter Notebooks.

Data Collection
------
The data was collected from the LinkedIn Australia website’s job section with the help of the Python library 'requests', 'BeautifulSoup' and 'pandas' to scrape and store. It was collected using information that was available without the need to log in (publicly available data). The code used to extract and export the data into a CSV file can be found <a href="https://www.code.com/">here</a> on my GitHub along with comments explaining each section.

Data Cleaning
------
A majority of the data cleaning and transformation of the collected data took place in Power Query. This was done to maintain data integrity and make the data format suit the needs of the analysis of Power BI desktop. The steps of the data cleaning and transformation process are listed below:

1. Split the Location into three different columns (Suburb, State and Country).
  - The country's values are isolated by using separate by a delimiter and separating on the rightmost delimiter “, “.
  - Then the suburb and state values from the location column are separated by the “, “ delimiter.
2. Each of the columns is trimmed to eliminate the excess white spaces. 
3. Clean each column where the correct values have not been split automatically when using separate by a delimiter. 
  - The country column’s null values are replaced with the value Australia as all the job applications are scraped from Australian LinkedIn.
  - The “Australia” values in the suburb column are eliminated using the replace function. 
  - The missing state values in some of the rows have to be added based on the suburb that is listed in the row. This is done by filtering all the null values in this column and then filtering each suburb individually and then adding the state it belongs to.
4. Cleaned and formatted the “Time since the posting” column by grouping the postings in even time frames.
5. Cleaned and formatted the “No of applicants” column by grouping the postings into three even categories of no of applicants.
6. Changed the replaced Null values with ‘Not applicable’ in the ‘Industries’ column to align with the other unknown values.
7. Cleaned rows of the ‘Job Title’ column which had unexpected ‘?’ and ‘&amp’ in them.
8. Changed column names to improve readability where applicable.

Data Analysis and Visualization
------
The data was then aggregated and visualized using DAX measures and charts in the form of a professional Power BI report. The report can be seen below after logging into your Microsoft Power BI account. If you do not have a Power BI account, there will be images of the report along with visualizations with the following writeup.

<iframe title="LinkedIn_data_analyst_jobs" width="1140" height="541.25" src="https://app.powerbi.com/reportEmbed?reportId=d7a58fac-ec19-41f3-9b6f-4b77d538879c&autoAuth=true&ctid=a8e716d0-a9d2-4b2b-b1e2-3711505feb2e" frameborder="1" allowFullScreen="false"></iframe>

------
<iframe title="LinkedIn_data_analyst_jobs" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiN2EyOGM1YTctOWFiNS00ZTRmLWFlODMtZDkzMDI0MzlkNTY1IiwidCI6ImE4ZTcxNmQwLWE5ZDItNGIyYi1iMWUyLTM3MTE1MDVmZWIyZSJ9" frameborder="0" allowFullScreen="true"></iframe>
