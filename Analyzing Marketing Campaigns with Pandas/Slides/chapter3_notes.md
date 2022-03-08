## Building functions to automate analysis

### 1. Building functions to automate analysis

In the previous chapter, you may have noticed that you were doing a lot of similar, repetitive tasks. Anytime you notice repetition in your work, you should think about how you can automate it.

### 2. Why build a function?

One of the best ways to make this kind of analysis faster is to create functions. For example, remember when you analyzed the quality of subscribers by day and conversion channel in the last chapter? You might want to conduct this kind of analysis repeatedly for different subsegments of the customer base.

### 3. Print daily_retention_rate

Rather than copy-paste the code snippet to make the appropriate edits, which can lead to typos and make it difficult to correct bugs as they arise, it is better to write a function.

### 4. Building a retention function

So we define a function, retention_rate() that allows users to input a DataFrame and a list of column names. This function follows the same steps to calculate the total number of users who converted and retained. First, it calculates the total number of retained users, then the total number of subscribers, and finally divides them to obtain the retention rate. However, this time in the groupby() method, we include the user-inputted column names.

### 5. Retention rate by channel

Now that we’ve defined the function, all we need to do to reproduce the retention rates from the previous chapter is call retention_rate() with the marketing DataFrame, and pass the date_subscribed and subscribing_channel columns as a list of strings. After unstacking, we have the same results as before.

### 6. Plotting daily retention by channel

Next, we follow the same steps as before to plot our results and...

### 7. Messy daily retention rate chart

Here's the resulting plot with much less effort than in the previous chapter due to our function! However, as you can see, this is a crowded chart that is nearly impossible to read. I recommend looking at channels one-by-one to identify trends.

### 8. Plotting function

Again, instead of writing similar code over and over to plot the columns one at a time, we will create a function: this function will create several plots, one for each column. The function uses a for loop to go through each column in the DataFrame and plot each column individually. Note that here we are using matplotlib's plot() function to keep things simple. Since the dates are still located in the DataFrame's index, we use the index attribute to display dates on the x-axis, and the column values go on the y-axis. We can now call this function on the same daily_channel_retention DataFrame.

### 9. Email plot

Which will then display a plot for each column in the DataFrame -- in this case a plot for each channel by date. This is the plot for email. You can see email has big spikes that often go down to 0. This is common because emails are typically sent in bulk leading users to subscribe on the same set of limited days. When retention rate is 0, this means no one subscribed on those days.

### 10. Let's practice!

Now it's your turn to practice building your own functions. 

## Identifying inconsistencies

### 1. Identifying inconsistencies

Now that we have a few functions to work with, we can slice our data in a variety of ways and calculate retention rates with minimal effort. Let's leverage our new functions and make the kinds of reports stakeholders might not know to ask, but will be happy you took a look.

### 2. Day of week trends

One of the most common reasons for fluctuating metrics is due to differences in how customers behave on different days of the week. For example, some businesses consistently perform better during weekdays than weekends. We pass the DoW column created in the first chapter to the retention_rate() function from the previous lesson.
### 3. Plotting the results

and plot the results.

### 4. Retention not affected by day of week user subscribes

As you can see, there does appear to be some relationship where retention is lower if users subscribe later in the week, but this difference is small and is likely indicative of something else, such as sending more emails on the weekend which converts lower intent users. This is all to say, weekday fluctuations are common and do not necessarily merit raising a red flag even if you see a consistent pattern, but it might mean modifying when you attempt to market to customers most heavily.

### 5. Real data can be messy and confusing

Sometimes with real data, variations in metrics may be due to random chance and have no explanation. Or we might not have access to the kind of data that we would need to identify the cause of a change. Other times, it requires a bit of creative thinking to find the underlying reason. When you find concerning variations in the data, you must be creative and comfortable with ambiguity. When you get stuck, it's a good time to brainstorm with coworkers. While this can be frustrating at first, over time, you will develop an intuitive sense of what kinds of problems can arise and how to identify these problems.

### 6. Let's practice!

Now, let's think creatively and try to find the root cause of House Ads’ conversion rate suddenly declining. 

## Resolving inconsistencies

### 1. Resolving inconsistencies

In the previous exercise, you identified a dip in conversion rates for House Ads. It appears that the problem was that users were seeing ads in languages other than there preferred language. In this lesson, we'll assess the impact of this mistake.

### 2. Assessing impact

While you cannot ignore data related to errors in the campaign, you can estimate what conversion might have looked like if there had been no issues. One way to assess impact is to index all other languages' conversion rates to English during the period where the ads were running in the correct language for each user. We begin by slicing the house_ads DataFrame to include the rows where the date_served is prior to when the language bug arose. Using our conversion_rate() function, we calculate conversion rate for each language during that period.

### 3. Assessing impact

We then divide the conversion rate of all other languages by the conversion rate of English in order to understand the relative relationship of how well our marketing assets typically convert users for each language compared to English.

### 4. Interpreting Indexes

What these indexes mean is that Spanish-speaking users typically convert 1.7 times the rate of English-speakers and Arabic and German speakers convert at about 4-5 times the rate compared to English-speakers.

### 5. Daily conversion

Next, we calculate the total number of users and actual conversions on each day. First, we group the DataFrame by date_served and language_preferred. Next, we do something different. We use the agg() method since we wish to calculate multiple statistics. We pass a dictionary to this method where the key is the column name, and the value is the method we want to apply on the column. Thus, we calculate the total number of unique user ids and the total number of users who converted.

### 6. Daily conversion

Finally, we unstack our result with level equals one to make it easier to manipulate in future steps. The result is a DataFrame with the number of users who should have seen ads in each language and how many of those users converted each day.

### 7. Create English conversion rate column

Since the conversion_rate() function puts the date_served in the DataFrame's index, we can use the loc accessor to slice our DataFrame and retrieve columns only from the period where the language bug was a problem. Our DataFrame has multi-level column names, so we can access the total number of people who converted for the English language by putting the two names in parentheses as a set, first writing "converted" because that is the first level of the column structure and then the relevant language. In this case, converted comma English.

### 8. Calculating daily expected conversion rate

Next, we can multiply the actual English conversion rate during this time by the language indexes we created earlier to determine what the expected conversion rates for these languages would have been for each day.

### 9. Calculating daily expected conversions

Then, we multiply the daily expected conversion rate of each language by the number of users who should have seen ads in that language. This gives us how many subscribers we would have expected if the language bug had not occurred.

### 10. Determining the number of lost subscribers

To calculate the overall impact, limit the expected conversion dataset to only the days when the bug occurred. Next, sum the number of expected and actual subscribers during that period, individually. Finally, we take the difference in the number of subscribers we expected and the subscribers we received, which gives us an estimate of how many subscribers we lost due to the language error.

### 11. Let's practice!

Now it's your turn. Let's assess the impact of this bug! 