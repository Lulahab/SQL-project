# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
The goal of this project was to load, clean, transform, and analyze data using SQL to gain valuable business insights. Specifically, this project aimed to answer both predefined questions and self-formulated ones to assess data trends and performance metrics.

## Process

step 1:Data Loading

Data Source: CSV files

Method: Used the COPY command in PostgreSQL to load the data into the database.


step 2: Data Cleaning

Initial Checks: Identified missing and inconsistent data using basic SQL queries,checked for duplicates,null

Cleaning Actions:

replaced null values with 0 whenever necessary,

cast data type into desired datatype

Adjusted negative quantity values to 0 to ensure data accuracy.

step:3  Answering Questions

Provided Question Example: Found the highest-city country generating revenue ?

Step 4: Creating Self-Formulated Questions

Analyzed  total monthly sales for the year 

step 5:Finally, I performed a QA step to validate the integrity of the data, ensuring accuracy and consistency throughout the analysis.


## Results
analyzed transaction revenues to identify the cities and countries generating the highest income, explored popular products and product categories by region, and found patterns in top-selling items across different locations, with notable revenue contributions from the United States.



## Challenges 
Data Quality: Encountered missing and negative values that required cleaning.

no unique identifier

data redundency

## Future Goals
 Implement strategies to populate NULL values and missing data by calculating averages or using data from related tables to fill gaps. 
Further Analysis:  Explore predictive modeling to forecast future trends.

