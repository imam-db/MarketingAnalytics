## Introduction to common marketing metrics

### 1. Introduction to common marketing metrics

In this lesson, we're going to discuss common marketing terms and metrics and how these metrics are typically measured.

### 2. Was the campaign successful?

As a data professional, you will often find yourself translating business questions into measurable outcomes. One of the most common questions you might be asked is, "Was the campaign successful?". At first, this might feel like an intimidating question. There are so many ways one could measure campaign success! That said, there are a few kinds of metrics you will find yourself using over and over again. For a marketing team, campaign success is typically measured by conversion rate. This is to say, of all the people who came into contact with your marketing campaign, how many bought the product? Depending on the business, this could mean that a person made a purchase or subscribed to your service. In addition, many subscription businesses care about retention. Once a user has signed up for a subscription, are they still a subscriber one, three or twelve months in? This metric can be particularly difficult to measure because it requires patience. We can't know 90-day retention rates until 90 days have passed since a user initially subscribed.

### 3. Conversion rate

Conversion rate is the percentage of people that we market to who ultimately convert to our product. In this course, we will be focusing on a subscription service and will be talking about conversion in terms of subscriptions.

### 4. Calculating conversion rate using pandas

To calculate the total number of people who converted, we slice the DataFrame to include only the rows where "converted" equals True and then count the unique user_ids using nunique(). Next, we calculate the total number of people we marketed to. We can do this by counting all the unique user IDs in our dataset. Now that we have the total number of users and users who subscribed, we can calculate conversion rate by dividing subscribers by total.

### 5. Retention rate

Retention rate is the percentage of people that remain subscribed after a certain period of time. In this course, we will focus on 1-month retention.

### 6. Calculating retention rate

To determine retention rate, we first calculate the total number of users who remain subscribers after one month. This information is available in the column "is_retained". We now slice the DataFrame to include only rows where the user was retained, that is, where is_retained equals True and count the unique user_ids. We can reuse our "subscribers" calculation from the conversion rate calculation, as the number of total users who originally subscribed remains the same. Finally, we divide the number of users who were retained by the number of users who subscribed to calculate the retention rate.

### 7. Let's practice!

Now that you understand the basics of how to determine campaign effectiveness let's practice! 


## Customer segmentation

### 1. Customer segmentation

In this lesson, we will focus on customer segmentation.

### 2. Common ways to segment audiences

In addition to high-level metrics, it's important to segment customers by who you're marketing to. Segmenting means breaking down metrics by specific characteristics. For instance, in addition to looking at conversion rates overall, you might also want to look at conversion rate by age group. It's possible that a campaign had a low conversion rate overall, but was super effective for users who were 55 years and up. Rather than calling the campaign a failure, your team has learned a new way to market to older individuals! That's great news! You could use these results to conduct a campaign where users 55 years and up receive a different marketing technique than everyone else.

### 3. Segmenting using pandas

One way to segment is by subscribing channel. Let's check the retention rate for users who converted by clicking on a House Ad. To do this, we first subset the DataFrame to include data only for House Ads, that is, where subscribing_channel equals House Ads. Using the house_ads DataFrame, you can calculate retention rate like before, dividing the total number of users retained by the number of subscribers who originally subscribed through a House Ad. That's great! But how do you know if this retention rate is good or bad? Ideally, you will compare retention rates across all channels to determine whether some channels perform better than others.

### 4. There must be an easier way to segment!

The previous method is great if you only care about some of the sub-segments that are in your dataset. However, recalculating retention rate can get tedious if you want to compare across all channels.

### 5. Segmenting using pandas - groupby()

This is when the flexibility of pandas comes in handy! You can use the groupby() method to analyze and calculate statistics for multiple sub-segments in your data. Here we first subset the data to include only the customers who were retained and then group by subscribing_channel. Then, we count the number of unique user ids to find the total number of retained customers per channel.

### 6. Segmenting using pandas - groupby()

Similarly, we subset the data to include the customers who subscribed, group by subscribing_channel and count the number of unique user ids to find the total number of subscribers for each channel.

### 7. Segmenting results

Finally, you can divide the number of retained customers by the total number of subscribers to find the retention rate for each channel. And here are our results. It appears email has the highest retention rate among our marketing channels.

### 8. Let's practice!

It's time for you to calculate metrics for different segments in the data. 

## Plotting campaign results (I)

### 1. Plotting campaign results (I)

Now that you've practiced aggregating and segmenting the results, it's time to learn how best to visualize these results in order to increase ease of interpretability, especially for people on our team who might be less comfortable with data.

### 2. Comparing language conversion rates

Let's use the language_conversion_rate series you created in the previous lesson. First, we'll import matplotlib's pyplot module as plt. Next, we call the plot() method on language_conversion_rate and specify the kind argument as bar indicating that we want to create a bar chart. Bar charts enable us to visually compare the conversion rates. Next, we add a title and axis labels to this plot. Finally, we show the plot using plt dot show().

### 3. Retention by language chart

And here's our plot! As we saw in the last lesson, Arabic and German have the highest conversion rates. However, instead of looking at a bunch of numbers, you can now quickly interpret the results by looking at the plot.

### 4. Calculating subscriber quality

In the previous lesson, we looked at how subscriber quality varied over time by looking at retention rates for each day. This was the code you used to calculate the daily retention rate. When looking at retention rates over time based on when users subscribed, we are doing a simple form of cohort analysis that helps us evaluate the quality of subscribers we're bringing in each day. If we see this metric trend up over time, that could mean we're getting better at converting users who are genuinely interested in our product or that we're improving our onboarding process once users subscribe.

### 5. Preparing data to be plotted over time

Before we plot this data, let's first convert the pandas series into a DataFrame by calling the reset_index() method on the series and wrapping the output in a call to pd dot DataFrame. Then we rename the columns using the columns attribute. Make sure you provide column names in the correct order and that there's a label for every column.

### 6. Visualizing data trended over time

Again, we call the plot() method on the daily_retention_rate DataFrame and pass the x and y arguments, that is, date_subscribed and retention_rate, respectively. Next, we add a title and x and y labels like in the previous chart. One thing to remember here is that matplotlib tries to be smart about the scale of the axes by rescaling it from the lowest to the highest values. While this is useful for dates on the x-axis, it has a negative side-effect for the values on the y-axis. The plot would show misleadingly large spikes in retention rates due to the range of the y-axis. Therefore, we use the ylim() function to explicitly set the y-axis to begin at 0.

### 7. Daily subscriber quality chart

And here's our final plot showing how subscriber quality changed over time.

### 8. Let's practice!

As you can imagine, there are endless ways to customize your charts to be more useful, interactive, and aesthetically pleasing. I encourage you to check out other plotting resources if this topic interests you. Now, let's practice! 

## 

### 1. Plotting campaign results (II)

In this lesson, we will combine all the concepts covered so far in this chapter by grouping multiple columns and plotting the results.

### 2. Grouping by multiple columns

You will often want to group the data by multiple dimensions. For instance, say you want to count the number of users for each preferred language on each date. In order to do this, you need to group the data by multiple columns; thus we pass a list of columns, date_served and language_preferred to the groupby() method and count the number of users. The result is a series with multiple indices and the number of users.

### 3. Unstacking after groupby

Sometimes it can be easier to manipulate the data when we have a DataFrame. We use the unstack() method to transform our data such that each preferred language becomes a column. Since preferred_language is the second index, we set the level argument to 1, indicating that we want to unstack the second index. Remember, the first index is represented with 0, and the second with 1.

### 4. Plotting preferred language over time

Plotting these results is very similar to what we've done in previous lessons. Since the index is a date, if you call the plot() method on the DataFrame, pandas will automatically draw a line plot. Since there's one line for each language, it's crucial to include a legend to ensure you know which language each line represents. We can use the legend() function to add a legend. The loc argument determines the location of the legend and to get the correct labels; we set the labels argument to the column names. The column names can be obtained by chaining the columns and values attributes.

### 5. Daily language preferences plot

And here's our daily language preference chart. As we can see, by far the most popular language is English.

### 6. Creating grouped bar charts

Let's say that we followed the same groupby process as before, but this time, we group by age group and preferred language to count the number of users. The code looks very similar as you can see here.

### 7. Plotting language preferences by age group

In this case, a line plot will no longer be the right way to show this data. Instead, when we plot, we set the kind argument to 'bar'. Once again, we must remember to include a legend using the column names.

### 8. Language preferences by age group

And here's the plot. See how easy it is to slice and dice your data in order to obtain various insights?

### 9. Let's practice!

Now, it's time for you to group by multiple columns. 