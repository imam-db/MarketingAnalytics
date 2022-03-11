## Analyzing Twitter Data

### 1. Analyzing Twitter Data

Welcome to analyzing social media data with Python. I'm Alex Hanna and I work at Google. In this class, we're going to analyze Twitter data using Python. There are millions of tweets created every day from across the entire world, in many different languages. In this class, we're going to learn how to collect Twitter data, how to process Twitter text, how to analyze Twitter networks, and how to map Twitter data geographically. Let's get started!

### 2. Why Analyze Twitter Data?

Twitter is one of the largest social networking sites in operation right now. While Twitter is far from a comprehensive record of the public conversation, it can help to provide insight into popular trends and important cultural and political moments. If you're a data scientist in industry, analyzing Twitter can be used for tasks such as marketing or product analysis.

### 3. Why Analyze Twitter Data?

If you're a computational social scientist, Twitter can be used as a measure of public opinion on important political or social topics. Twitter data has been used to analyze political polarization, public opinion of world leaders, and the spread of protest movements.

### 4. What you can't analyze

Of course, you can't access all of what happens on Twitter. It may seem obvious to say, but you can only collect information on what people say, not who is watching passively. Twitter collects data on this internally but doesn't release it for analysis. If you're looking to use free tools for doing your analysis, you're also limited in two important ways. First, you can't collect data from the past. If you wanted data on your company from one year ago and you didn't collect it at the time, you're out of luck. Second, Twitter only offers a sample of their data for free, what they say is a 1% sample.

### 5. What you can analyze

However, a 1% sample of Twitter is still on the order of a few million tweets a day. In each of those tweets, you get a lot of information: the text of the tweet, user profile information -- like the number of followers and followees the user has -- geolocation, and other extras. You also get all of that information for retweets and quoted tweets.

### 6. Let's review!

We'll get into how to access all of that information in a second. Let's review what we can do with Twitter. 

## Collecting data through the Twitter API

### 1. Collecting data through the Twitter API

Now that you have a good idea of what kind of insights can be gained through Twitter data, let's look at how to collect some Twitter data ourselves through the Twitter API.

### 2. Twitter API

APIs, or Application Programming Interfaces, are methods of accessing data from a business or government organization. Many, if not most, social media companies have some kind of API which are made available to third-party developers and researchers. Twitter has multiple APIs which can be used for different purposes. These change from time-to-time and Twitter sometimes adds or removes APIs. These include the Search API, which allows access to tweets from the past week, the Ads API, which focuses on Twitter ads, and the Streaming API. We'll focus on the Streaming API for this course.

### 3. Streaming API

The Streaming API allows us to collect a sample of tweets in real-time based on keywords, user IDs, and locations. The connection stays open until you close it. The Streaming API has two endpoints, filter and sample. With the filter endpoint, you can request data on a few hundred keywords, a few thousand usernames, and 25 location ranges. With the sample endpoint, Twitter will return a 1% sample of all of Twitter.

### 4. Using tweepy to collect data

To collect data from the Streaming API, we're going to use a package called `tweepy`. `tweepy` abstracts away much of the work we need to set up a stable Twitter Streaming API connection. When you do this in practice, you're going to have to set up your own Twitter account and API keys for authentication. For now, we'll simulate having API keys with us.

### 5. SListener

`tweepy` requires an object called `SListener` which tells it how to handle incoming data. We've given you the code for this object, so you won't need to write your own. We'll show you the constructor here to give you an idea of what it does. Our `SListener` object inherits from a general `StreamListener` class included with `tweepy`. It opens a new timestamped file in which to store tweets and takes an optional API argument.

### 6. tweepy authentication

We first must authenticate with Twitter. OAuthentication, the authentication protocol which the Twitter API uses, requires four tokens which we obtain from the Twitter developer site: the consumer key and consumer secret, and the access token and access token secret. We pass the `OAuthHandler` our consumer key and consumer secret. Then we set the access token and the access token secret. Finally, we pass the auth object to the tweepy API object.

### 7. Collecting data with tweepy

Now we can get to collecting data. If we were going to take a random sample of all of Twitter, we would use the `sample` endpoint. First, we instantiate an `SListener` object. Then, we instantiate a `stream` object. Lastly, we call the `sample` method to begin collecting data.

### 8. Let's practice!

Now it's your turn. Let's practice writing the code to collect Twitter data. 

## Understanding Twitter JSON

### 1. Understanding Twitter JSON

Great job! Now that you've collected some Twitter data, let's dig into the structure of the data. In the examples which we are working with in this course, the information is returned in Javascript Object Notation, or JSON. JSON is a special data format which is both human-readable and is easily transferred between machines. JSON is structured a lot like Python objects and is composed of a combination of dictionaries and lists.

### 2. Contents of Twitter JSON

Understanding the Twitter JSON is critical to knowing how to analyze Twitter data. There's a lot of data in a single Twitter JSON. For instance, in a single original tweet -- that is, a tweet that's not a retweet or a quoted tweet, you have foundational information like the text, when it was created, and the unique tweet ID. You also have information like how many retweets or favorites it has at the time of collection, what language it's in, if it's a reply to a tweet and to which tweet, and to which user.

### 3. Child JSON objects

More importantly, the Twitter JSON contains several important child JSON objects. These are like dictionaries stored in other dictionaries. The important ones which we'll cover in this course are `user`, `place`, and `extended_tweet`. `user` contains all the useful information you'd want to know about the user who tweeted, including their name, their Twitter handle, their Twitter bio, their location, and if they're verified.

### 4. Places, retweets/quoted tweets, and 140+ tweets

`place` contains information on the geolocation of the tweet, and we'll get into that in chapter 4. `extended_tweet` contains the full text of a tweet which is over 140 characters in length. When a tweet is a retweet or contains a quoted tweet, the whole of that tweet will be contained with the Twitter JSON. For retweets, that tweet will be stored in `retweeted_status` and for a quoted tweet in `quoted_status`.

### 5. Accessing JSON

Let's start exploring the Twitter JSON by loading a single tweet. We'll use the open() and read() methods to load the JSON file into a JSON object. Then, we'll use the `json` package and the `loads` method to convert the JSON into a Python dictionary. Lastly, we access the value of interest by using its appropriate key.

### 6. Child tweet JSON

Child Twitter JSON can be accessed as nested dictionaries. Let's look at the `user` object. To access the user's handle, we use the `user` key, then the `screen_name` key. We can do the same with `name` to show the user's display name, and `created_at` to show when the user created their Twitter account.

### 7. Let's practice!

Let's practice working with Twitter JSON. 