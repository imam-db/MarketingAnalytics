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