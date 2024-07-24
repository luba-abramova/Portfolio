# Accenture virtual experience - top 5 content categories
This project is based on [Forage](https://www.theforage.com/simulations) job simulation program for [Accenture](https://www.theforage.com/simulations/accenture-nam/data-analytics-mmlb).

**Task**: apply your data analytics & visualization skills to advise a social media client on their content creation strategy as a Data Analyst at Accenture.

This project is completed using **Microsoft Excel** for data cleaning, modeling, and visualization, and **Power Point** for sharing the results.

## Project Understanding
Client name: Social Buzz 

Client industry: Social media & content creation 

Social Buzz emphasizes content by keeping all users anonymous, only tracking user reactions on every piece of content. There are over 100 ways that users can react to content, spanning beyond the traditional reactions of likes, dislikes, and comments. This ensures that trending content, as opposed to individual users, is at the forefront of user 
feeds. 

Over the past 5 years, Social Buzz has reached over 500 million active users each month. They have scaled quicker than anticipated and need the help of an advisory firm to oversee their scaling process effectively. 

To start our engagement with Social Buzz, we are running a 3 month initial project in order 
to prove to them that we are the best firm to work with. They are expecting the following: 
- An audit of their big data practice.
- Recommendations for a successful IPO.
- An analysis of their content categories that highlights the top 5 categories with the largest aggregate popularity **(my task as a Data Analyst).**

## Data Cleaning & Modeling

The client has sent 7 data sets. For our analysis to find the top 5 categories with the largest aggregate popularity, I used three of them: Content, Reaction and ReactioType.

![](/Accenture%20case/screenshots/data_model.png)

### I performed data cleaning as follows:

**Content** table:
- Changed the column name “Type” to “Content Type”.
- Deleted the User ID column, as it was not needed for the analysis.
- Addressed duplicates in the "Category" column: some names were with quotation marks and some were not. Replaced those with quotation marks using the Find and Replace function.

![](/Accenture%20case/screenshots/quotation_marks.png)


**Reactions** table:
- Changed column name “Type” to “Reaction Type”.
- Deleted User ID column, as we won’t work with this data.
- Deleted null values in Reaction Type column.

![](/Accenture%20case/screenshots/blank_types.png)

**ReactionTypes** table:
- Changed column name “Type” to “Reaction Type”.


### Data Modelling
1. Created a final data set by importing CSV files to Excel and merging three tables together. Used the Reaction table as a base table, then first joined the relevant columns from Content data set, and then the Reaction Types data set.

2. To merge "Content" with "Reactions", and "ReactionTypes" with "Reactions" used VLOOKUP formulas:

![](/Accenture%20case/screenshots/vlookup.png)

3. Copied and pasted the new columns "Content Type", "Category", "Sentiment", and "Score" as values.

4. Renamed this sheet as "Merged Data", deleted "Content" and "ReactionType" sheets.

5. Created a new sheet "Categories". Copied categories there, and used SUMIF function to calculate the total score, then sorted and highlighted the top 5 categories. Saved as values.

![](/Accenture%20case/screenshots/sumif.png)

6. Saved top 5 categories on a new sheet.

## Data Visualization & Storytelling

In this project, I chose quick and simple Excel charts. I adjusted the color and text size directly in the presentation for a better visual appeal.

The Power Point presentation is available [here](/Accenture%20case/Social%20Buzz%20Top%20Categories.pdf) (formatted as a PDF). The presentation template was provided in the task.

![](/Accenture%20case/excel_charts.png)

