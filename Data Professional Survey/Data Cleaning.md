# Data Overview
This project utilizes survey data collected by Alex Freberg from Data Professionals. The survey covered general demographics such as country, age, and gender, along with questions evaluating respondents' job satisfaction across various criteria.

The dataset, provided in .xlsx format, consists of 28 columns and 630 rows. You can view the dataset [here](https://github.com/AlexTheAnalyst/Power-BI/blob/main/Power%20BI%20-%20Final%20Project.xlsx).

# Data Cleaning
For the analysis, I performed the following data cleaning steps:

1) Deleted columns that were not relevant to my analysis.
2) In the "Current Role" column, I regrouped responses by consolidating all answers starting with "Other..." into a single value "Other" using the text-to-columns wizard.

![](/Data%20Professional%20Survey/screenshots/cleaning_1.png)


3) In the "Favorite Programming Language" column, I grouped all SQL-related responses into one value, combined all other responses into "Other," and categorized "don’t use" as "None".

![](/Data%20Professional%20Survey/screenshots/cleaning_2.png)

4) Calculated the average salary from the "Salary" column, converting ranges into a single average value for better visualization.
