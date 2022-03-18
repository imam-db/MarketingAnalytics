## Processing Twitter text

### 1. Processing Twitter text

In the first chapter, we collected Twitter data. In this chapter, we're going to analyze the text of the tweets themselves.

### 2. Text in Twitter JSON

In the prior lesson , we saw that there are many parts to the Twitter JSON. Arguably, the most important part of the tweet is the text. Recall that the primary location of the text in the Twitter JSON can be accessed via the `text` key. However, that's not the only place where we can extract meaningful text-based elements.

### 3. More than 140 characters

Twitter has made it possible to use more than 140 characters. While this is great for self-expression and fun emoji games, to accommodate for this, Twitter added a child JSON to the Twitter JSON object. For tweets which are over 140 characters, we can access the text through the `extended_tweet` child JSON object and the `full_text` field.

### 4. Retweets and quoted tweets

Also recall, to get those objects in retweets and quoted tweets, we have to access the `retweeted_status` or `quoted_status` child JSON objects. Once we parse the JSON, we can access these elements by chaining together dictionaries.

### 5. Textual user information

Parts of the user JSON object may have informative textual elements as well. We can gain insight about a user's communities, partisan behavior, or geographical location through their user profile. To extract these, we access the `description` and `location` fields in the user child JSON.

### 6. Flattening Twitter JSON

So far we've only deal with individual tweets. To analyze tweets at scale, it's helpful to put everything into a pandas data frame. This allows us to apply certain analysis methods across all rows and multiple columns. However, with the multiple JSON children, we can't access values in those children easily in columns. To do this, we'll flatten the JSON by storing the children values in top-level keys. For convenience, we'll separate the original keys with a dash. This is how we would flatten a single 280 character tweet, for instance.

### 7. Flattening Twitter JSON

To put these all into a DataFrame, we will have to loop through all of the tweets individually. We'll open a JSON file full of tweets -- in this case `all_tweets.json`. We'll then split the JSON by the newline character. Then, for each tweet, we'll parse the JSON with `json.loads`, which converts JSON to Python. We'll then check if the tweet contains a field of interest, such as a 280 character tweet in `extended_tweet`. If it does, we'll create a top-level field for it in the tweet dictionary. Here, we are creating a new field, `extended_tweet-full_text`. We'll then add this tweet object to a list of tweets. This is now a list of dictionaries which represent tweets. Because this is a list of dictionaries, we can pass it as an argument to the pandas data frame constructor, which will convert this JSON file full of tweets to a data frame.

### 8. Let's practice!

In the exercises, we're going to write a function which flattens multiple fields which we can use for the rest of this course. 

## Counting Words

### 1. Counting words

Great job! Now that we're able to analyze tweets at scale, it's time to begin processing tweet text. One of the most basic analyses we can perform is counting the number of times a word or a phrase has appeared in text, and how many times it has appeared compared to other keywords in the text.

### 2. Why count words?

Why should we count words? Counting words is the most basic step we can take in automating the analysis of text. It allows us to convert words into numbers. In practice, counting words can tell us how many times a company, product, or hashtag is mentioned. In the exercises, we'll look at how the R and Python hashtags compare to each other. While this won't settle the question of which programming language is better, it can give us an idea about whether there's more talk about one hashtag compared to the other.

### 3. Counting with str.contains

To count the frequency of a particular keyword, we'll use `str.contains`. This is a string method for the pandas Series object which tells us whether or not a row contains the keyword in question. In other words, `str.contains` will return a boolean `Series` object, that is, it will contain only True/False values. It also takes the keyword argument case. Setting `case equal to False` will make it case insensitive.

### 4. Companies dataset

We'll first look in the `text` column of our Twitter data frame. Say we have a dataset of tweets mentioning three companies: Apple, Facebook, and Google. We want to know what proportion of tweets mention Apple. First, we'll flatten the tweets and load them into a data frame. Then, we'll use `str.contains` on the `text` column. We'll then use `numpy.sum` to add up all the True values, since they are numerically equal to one. Lastly, we'll divide that by the number of total items to get the proportion of tweets which mention 'apple'. Here, Apple is mentioned 11.2 percent of the time.

### 5. Counting in multiple text fields

However, a single tweet contains multiple places where relevant keywords can appear. Remember that a tweet may contain a retweet, a quoted tweet, and text over 140 characters. Furthermore, we also may want to search in user locations and user descriptions. You can search these fields separately, or you can loop through these fields and use the logical `or` operator to connect them together. Recall that a logical `or` will evaluate to True if at least one of the values is true. In this example, we first evaluate the 'text' field. We then loop through the extended tweet and retweet text fields to find instances of the keyword. We `or` them with each other using the pipe operator. This searches the text and extended text of both the original tweet and the retweet. Lastly, we print out the proportion of tweets which contain the keyword. We see that the proportion has gone up from 11.2 percent to 12.8 percent.

### 6. Let's practice!

Now it's your turn to count keywords in some Twitter data. 

## Time Series

### 1. Time series

Now you know how to check how many tweets mention a word or phrase in a Twitter dataset. We're going to build on this by illustrating how mentions of keywords change over time.

### 2. Time series data

Tweets about companies, products, and political issues vary by the day, hour, minute, and even to the second. We want to be able to capture that variation over time. When data is tagged with a date and time, this is known as "time series" or "over time" data. The structure of time series data typically contains a date or datetime, and some type of numerical measure. In the data frame above, we have a datetime, a count of mentions of a political candidate in time period, and the name of that political candidate.

### 3. Converting datetimes

We can convert the date information from a string to a `datetime` type. Pandas is smart enough to convert the Twitter format for the date -- stored in `created_at` -- to a datetime type with the `to_datetime` method. Next, we set the index of the DataFrame to the `created_at` column using the `set_index` method. This allows us to access a number of useful time series methods.

### 4. Keywords as time series metrics

Now that we have our data frame in a time series format, we need to produce a metric which can be graphed over time. Our function `check_word_in_tweet` returns a boolean Series which indicates which rows contain our keyword and which do not. Remember that the boolean value True is the same as the numerical value one. Therefore, we can produce a column for each keyword we're interested in and understand its prevalence over time. If we sum up this column, we get an overall count of how many times that keyword appears.

### 5. Generating keyword means

Now that we have a metric, we can now begin plotting the keyword over time. We first generate a summary statistic over the metric we're interested in. We can use Series method `resample` for this purpose. `resample` allows us to summarize over a time window of our choice and apply a function to it. We'll use `resample` with the `mean` method to generate averages over one-minute windows. The averages are measured as a proportion of all the tweets within the window.

### 6. Plotting keyword means

Lastly, we'll plot those keyword means over time. We import `matplotlib.pyplot`, then we use `plt.plot` to create the plot. On the x-axis, we'll use the minute index and on the y-axis, we'll use the generated mean. We'll color Facebook blue and Google green. In this dataset, we see that mentions of Facebook seem generally higher over time compared to mentions of Google.

### 7. Let's practice!

Now it's your turn. In the following exercises, you'll be plotting keyword prevalence across time. 

## Sentiment analysis

### 1. Sentiment analysis

Excellent! You've been able to detect the presence of words in tweets and plot their relative prevalence across time. This is a small step in the direction of understanding meaning in text. In this lesson, we're going to focus on a method for deriving meaning from text called sentiment analysis.

### 2. Understanding sentiment analysis

Sentiment analysis is a type of natural language processing method which determines whether a word, sentence, paragraph, or document is positive or negative. The idea behind sentiment analysis is that we count the words which are positive and negative as a proportion of the words in the rest of the document. Each document then gets a positive and negative score. Sentiment analysis can be useful in gauging reactions to a company, product, politician, or policy.

### 3. Sentiment analysis tools

We'll use the VADER SentimentIntensityAnalyzer included with the Natural Language Toolkit or `nltk`. The VADER toolkit handles short text documents like tweets very well because it measures sentiment not only with particular words, but also for emoji and different types of capitalization. For instance, there's a qualitative difference in 'Nice' in lowercase letters versus 'NICE' in all caps.

### 4. Implementing sentiment analysis

To use VADER, we first import it from `nltk`. Next, we instantiate a SentimentIntensityAnalyzer. Lastly, we can generate sentiment scores with the polarity_scores function and the Series method, `apply`.

### 5. Interpreting sentiment scores

A critical part of any type of natural language processing involves reading the text and assessing whether the method makes sense compared to a human reading. If we're attempting to replicate meaning with computational methods, then we have to make sure that meaning has face validity. Face validity means that the metric matches the concept we're trying to measure. In this case, we want to be sure the sentiment score matches our idea of what it means for a tweet to be positive or negative.

### 6. Interpreting sentiment scores

Here, we have two examples -- a positive tweet and a negative tweet. Each sentiment score from the VADER analyzer provides four values: negative, neutral, positive, and compound. Positive and negative are self-explanatory, while neutral measures words that don't contribute to the sentiment. Compound, however, is a combination of the positive and the negative; it's an overall assessment which ranges between negative 1 and positive 1. Below 0 is negative, and above 0 is positive. The first tweet presented here reads to human eyes as positive and the compound score is rather high -- about 0.9. The second tweet reads as negative but the compound score is only slightly below zero, around -0.07.

### 7. Generating sentiment averages

We generate sentiment averages over time in the same way we generate average prevalence measures. We'll extract only the 'compound' field from the sentiment scores. Next, we'll separate sentiments for each company with our `check_word_in_tweet` function. Lastly, we'll generate an average value for our time window of one minute.

### 8. Plotting sentiment scores

We can plot sentiment scores in the same way we plotted the prevalence of keywords. We'll set the x-axis to time, and the y-axis to our sentiment score. This plot indicates that sentiment towards Google is slightly higher than that of Facebook over time. This is despite Facebook being mentioned more, which we saw in the last lesson. This underlines the importance of extracting meaning from text, not just performing keyword counts.

### 9. Let's practice!

Now that you know what sentiment analysis is, let's revisit the data science hashtag dataset and analyze the sentiment of those tweets. 