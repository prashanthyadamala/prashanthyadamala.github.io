---
title: "Analysis of LinkedIn Data Analysis Job Postings in Australia (Jun 2024 - Present)"
excerpt: "Analysed 400+ LinkedIn data analyst job postings in Australia using Python to collect data via web-scraping and Power BI visualizations to provide insights into the current job market.<br/><img src='/images/LinkedIn-image-map.png' width='800' height='480'>"
collection: portfolio
---

Introduction
------
The goal of this project is the analyse the data analyst job postings in Power BI to help me better understand the opportunities available to me during my job hunt. This project would give me an overview of the jobs in a Power BI report as well as narrow down jobs that I want to apply for immediately using filters. The data used in this project was scrapped from LinkedIn Australia using Python in Jupyter Notebooks.

Data Collection
------
The data was collected from the LinkedIn Australia website’s job section with the help of the Python library 'requests', 'BeautifulSoup' and 'pandas' to scrape and store. It was collected using information that was available without the need to log in (publicly available data). The code used to extract and export the data into a CSV file can be found <a href="https://www.code.com/">here</a> on my GitHub along with comments explaining each section.

Data Cleaning
------
A majority of the data cleaning and transformation of the collected data took place in Power Query. This was done to maintain data integrity and make the data format suit the needs of the analysis on Power BI desktop. The steps of the data cleaning and transformation process are listed below:

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

<iframe title="LinkedIn_data_analyst_jobs" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiN2EyOGM1YTctOWFiNS00ZTRmLWFlODMtZDkzMDI0MzlkNTY1IiwidCI6ImE4ZTcxNmQwLWE5ZDItNGIyYi1iMWUyLTM3MTE1MDVmZWIyZSJ9" frameborder="1" allowFullScreen="true"></iframe>
<br/>
The first page contains an overview of the job listings from LinkedIn Australia. The Azure map visual shows the distribution of data analyst job listings across the different states of Australia with the number of listings signified by the size of the bubble. Each bubble also shows the segments of different seniority-level requirements of that particular state. This visualization cannot be viewed in the embedded Power BI report above thus a copy of it is given below.

<img src='/images/LinkedIn-image-map.png' width='800' height='480'>

The seniority level required is an ideal metric for me to see which state has a better proportion of jobs for entry-level data analysts. The map visualization gives a good idea of where most of the jobs are and also if I qualify for them.

There is also a ‘Jobs by State’ treemap visualization of the number of job listings in each state as it acutely shows the data in more comparable segments. The ‘Jobs by Week of Posting’ column chart helps me show the number of jobs that have been posted recently and focus on applying to them as older job postings tend to have a lower response rate. The last chart is the ‘Jobs by No. of Applicants’ pie chart. This shows the breakdown of applicants for the jobs. Ideally, I would want to apply for the jobs that fall in the < 25 applicants range. 

This page also has filters that can be applied to all of the visualizations on the page along with the metrics (card visualizations) to generate further insights into the data analyst job market.

On page 2 of the report is a table with columns describing the job. This page is used to narrow down a job to apply for. The list of jobs can be narrowed down using the filters on the right-hand side of the page. Once a job is identified copy the job ID and then go to the LinkedIn jobs page and replace the ID in the URL to end up on the application page. 

Recommendations to Implement in the Future
------
Currently, the data scraped on when the job was posted is ‘Days since posting’. It would be ideal to find a way to find the actual date on when it was posted so that it can give better insights and narrow down which jobs to apply for immediately. With the date formatted, the scraping code can be rerun regularly to get a growing database of job postings. 

Learn how to implement Neuro-linguistic programming (NLP) to find key skills in job descriptions to also be added to the database of job listings. This can help me further pinpoint the jobs that match the skill set that I have. It can also give me an understanding of popular skills that are in demand in the job market that I can start learning. 

If I can automate the web scraping and complete the other two recommendations, then I could sometime in the future make a cool line chart that tracks the popularity of different skills required for data analysts over time.
