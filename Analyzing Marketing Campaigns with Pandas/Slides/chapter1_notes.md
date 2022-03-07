## Introduction to pandas for marketing

### 1. Introduction to pandas for marketing

Welcome to the course! My name is Jill Rosok, and in this course, you will learn about how Data Science techniques are used to understand the impact of marketing campaigns.
### 2. What does a data scientist on a marketing team do?

My hope is that this course will not only help to reinforce your Python and pandas abilities but also help understand what kinds of problems data scientists on marketing teams might encounter. While the possibilities are endless, there are a few types of projects that will almost certainly come up in a marketing team. You will likely be asked how a marketing campaign performed. Marketing campaigns mean anything that required the marketing team to put in work to promote your product. It could be a new creative direction, a discounted product, targeting a specific demographic or a multitude of other options. Another common question is how different marketing channels are performing. For example, when you send out an email how many new users subscribe? Given current conversion rates and revenue, should you continue investing in this channel and how much should you spend? Another common practice in marketing is running experiments, or A/B tests, to try to understand the impact of a particular change. All of these types of questions can intersect. You could analyze a marketing campaign by channel based on A/B test results, or you could tackle any one of these types of questions individually.
### 3. What is pandas, again?

First, let me give you a quick refresher on pandas. Hopefully, you've completed DataCamp's foundational pandas courses, but as a reminder, pandas makes data analysis and transformation in Python much easier by formatting the data into a table-like structure similar to an Excel spreadsheet. Pandas makes it easy to import and export common data formats. Once your data is imported, you can adapt your dataset to work for your analysis, including aggregations, merging multiple datasets, and selecting subsets of data that fit specific criteria.
### 4. Importing data using pandas

To use pandas, first import pandas using the alias pd. To import a CSV file, you can use the read_csv() function and pass the name of the file you want to import.
### 5. Inspecting data

Once you've imported your data, it is a good practice to examine its contents using the head() method. This will return the first five rows of the DataFrame.
### 6. Summary statistics

Use the describe() method to print the statistics of all columns in your dataset. You can inspect the output to find some obvious errors. For example, if you see negative values in a date column, this might indicate an error. In addition, pay careful attention to the minimum and maximum values. If the maximum is much larger than the median, it might be an outlier and merit further investigation.
### 7. Missing values & data types

Finally, you can identify the data types and the number of non-missing values in your DataFrame using the info() method. The result includes all columns and their data types.
### 8. Let's Practice!

Now that you have a high-level understanding of pandas and data science in marketing let's practice combining these two skills! 


## Data types and data merging

### 1. Data types and data merging

In this lesson, we will talk about various techniques to manipulate data using Pandas.

### 2. Common data types

Each column in a pandas DataFrame has a specific data type. Some of the common data types are strings (which are represented as objects), numbers, boolean values (which are True/False) and dates.

### 3. Data type of a column

You can use the dtype attribute if you are interested in knowing the data type of a single column.

### 4. Changing the data type of a column

To change the data type of a column, you can use the astype() method. For example, you saw on the earlier slide that the converted column is stored as an object. It contains True and False values, so it's more appropriate to store it as a boolean. You can use the astype() method along with the argument 'bool' as shown here to change its data type. If you check the data type of the 'converted' column again, you will see that it's now 'bool'.

### 5. Creating new boolean columns

The marketing_channel column captures the channel a user saw a marketing asset on. Say you want to have a column that identifies if a particular marketing asset was a house ad or not. You can use numpy's where() function to create a new boolean column to establish this. The first argument is an expression that checks whether the value in the marketing_channel column is 'House Ads', the second argument is the value you want to assign if the expression is true, and the third argument is the value you want to assign if the expression is false.

### 6. Mapping values to existing columns

Due to the way pandas stores data, in a large dataset, it can be computationally inefficient to store columns of strings. In such cases, it can speed things up to instead store these values as numbers. To create a column with channel codes, build a dictionary that maps the channels to numerical codes. Then, use the map() method on the channel column along with this dictionary, as shown here.

### 7. Date columns

Often, you will have date columns that are improperly read as objects by pandas. However, as you will see in the following lessons, having date columns properly imported as the datetime datatype has several advantages. You have two options to ensure that certain columns are treated as dates. First, when importing the dataset using the read_csv() function, you can pass a list of column names to the parse_dates argument to ensure that these columns are correctly interpreted as date columns.

### 8. Date columns

Another option is to use the pandas' datetime() function to convert a specific column.

### 9. Date columns

Once the dates in the column are properly imported, you can use various date attributes to extract relevant information. For example, to obtain the day of the week, you can use the dayofweek attribute along with the dt accessor on the date column. This will result in a numerical value where 0 maps to Monday, 1 to Tuesday, and so on.

### 10. Let's Practice!

It's time for you to practice these concepts. 

## Initial exploratory analysis

### 1. Initial exploratory analysis

Now that you've imported the marketing dataset and are familiar with it let's do some initial exploratory analysis.

### 2. How many users see marketing assets?

To begin, let's get a sense of how many unique users see our marketing assets each day. We can use the groupby() method on the marketing DataFrame. To group by date, we pass 'date_served' as the argument to groupby(). Next, we select the user_id column outside of the groupby() and use nunique() method to count the number of unique users each day. Looks like about 300 users each day see our ads.

### 3. Visualizing results

As you saw on the previous slide, it's not easy to interpret results when they're printed in a table. It's much easier to notice fluctuations in our metrics when we plot them. We first import matplotlib dot pyplot as plt. Then, we plot the series daily_users. It's good practice to always add title and labels to your plot in order to clearly convey what information the chart contains. You can add a title using plt dot title(), and add x and y labels using plt dot xlabel() and plt dot ylabel() functions, respectively. We also rotate the xticks, in this case, the date labels, by 45 degrees to increase legibility. Finally, don't forget to include a call to plt dot show() to display the plot.

### 4. Daily users plot

As you can see, while the first half of the month sticks around 300 users per day, there's a huge spike in the middle of the month. This may be because we sent out a big marketing email, which reached many users who are not daily visitors of the site. These are the kinds of fluctuations we want to be aware of before diving in and calculating metrics.

### 5. Let's practice!

Now it's your turn to analyze this data. 