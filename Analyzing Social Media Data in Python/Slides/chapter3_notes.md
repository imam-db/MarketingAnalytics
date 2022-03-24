## Twitter Networks

### 1. Twitter networks

In this chapter, we're going to be working with Twitter networks. Twitter, after all, is a social networking site, and a major part of its functionality is connecting to other people in networked ways. We'll discuss how there are multiple types of networks which are embedded in Twitter data. We'll generate those networks using the `networkx` package and graph those networks. If you want more details on network analysis, you can check out the Network Analysis in Python courses on DataCamp.

### 2. Dataset: State of the Union

For this and the next chapter, we're going to use a dataset generated from the 2018 State of the Union address, the first State of the Union address given by Donald Trump. Given Trump's contentious presidency and the processes of political polarization in the US, network analysis can give us a way to look into how political groups are divided online.

### 3. Network analysis: terms

Let's define some basic network terms. A *node* is the actor within a network. Within Twitter, the nodes are users. An *edge* or *tie* specifies the relationship between nodes. We're going to look at multiple types of relationships. In this course, we are only going to be working with *directed* networks, which means that an edge only goes one way -- the relationships are not mutual. For example, you can retweet someone else, but they don't have to retweet you. The *source* node is where the relationship begins, whereas the *target* node is where the relationship ends.

    1 http://mathworld.wolfram.com/GraphEdge.html

### 4. Types of Twitter network ties

We're going to be working with three distinct types of networks within Twitter: retweet networks, quote tweet networks, and reply networks. You can imagine how each of these relationships can have a different social meaning. They also have different source and target users. Let's take a second to explain each of these in detail.

### 5. Retweet networks

The first is the retweet network. The retweet is a directed edge in which the source node retweets the target node. From the point of view of interpretation, retweets often signal agreement, but you may be familiar with the common refrain: retweets do not equal endorsement. Retweets can also can be used for a more neutral type of information dissemination. In this example, @DataCamp -- the source node -- retweets a tweet by @DatIO -- the target node.

### 6. Quote networks

The second is the quote tweet network. This is a newer Twitter feature which allows a user to add their own commentary to an existing tweet. The quoting user is the source node, while the quoted user is the target node. Interpretation can vary here -- a quote tweet may signal disagreement, agreement, or some kind of warning or added information. In this case, @alexhanna -- the source user -- quotes a tweet by @today_explained -- the target user.

### 7. Reply networks

The third type of network is the reply tweet network. The source user replies to a tweet by a target user. Interpretation can vary wildly: I may reply to someone who is my friend or someone I have agreed with, or I may reply to a celebrity with whom I really disagree. In this example, the source user -- @timnitgebru -- quotes a tweet authored by the target user, @histoftech.

### 8. Let's practice!

Let's review the different types of networks you can analyze from Twitter. 

## Importing and visualizing Twitter networks

### 1. Importing and visualizing Twitter networks

Now that you have a good idea of the different kinds of networks in Twitter data, now we can begin importing data into Python, as well as constructing and visualizing networks.

### 2. Edge Lists

One of the most common methods of constructing a network is by creating what's called an *edge list*, which is a list of all the edges between nodes. Luckily, in the way that we've formed our tweet datasets, we already have a way of easily retrieving the edge lists.

### 3. Importing a retweet network

We first import the `networkx` package, a full-featured package for network analysis in Python. We'll then flatten the tweets and load them into a pandas data frame. Lastly, we use the networkx method `from_pandas_edgelist` to construct a network. This methods takes four arguments -- the data frame, the source, which in this case is the user's screen name, the target, which is the retweeted user's screen name, and lastly `created_using` to specify that this is a directed graph. We specify this by passing in a `DiGraph` object.

### 4. Importing a quoted network

Importing a quoted status network is very similar. We flatten the tweets and convert the JSON to a data frame like before. The largest change is that we replace the target argument with the quoted status's user screen name.

### 5. Importing a reply network

Importing a reply network is similar but slightly different. We use a new field as the target argument: `in_reply_to_screen_name`.

### 6. Visualization

Visualization is a standard part of exploratory data analysis and this is no different for networks. With this randomly generated network stored in the variable T, we use the method `draw_networkx` to visualize the network. We also turn off the axes in `matplotlib` so we don't have axis lines around it. The default visualization has red nodes and displays labels for each of the nodes. Nodes are also all the same size. We can change each of those properties, however.

### 7. Visualization options

In this version of the visualization, we have set a few more arguments in the `draw_networkx` method. We have changed the size of the nodes by using a list comprehension to get the degree -- or the number of edges the node has -- and using them to create a list of size values. We also removed the labels by setting the `with_labels` argument to False. We changed the transparency of the nodes with the `alpha` argument. Lastly, we changed the width of the edges with the `width` argument.

### 8. Circular layout

Often times, plotting a large network with the default layout can take a long time to calculate. While the default layout can show which nodes are more important, we can use a circular layout to illustrate the density of edges. First, we calculate the positions with `circular_layout` method and store that in its own variable, `circle_pos`. Then, we set the `pos` argument to that variable.

### 9. Let's practice!

Let's practice importing and visualizing networks in the following exercises. 

## Individual level network metrics

### 1. Node-level metrics

Now that you know how to load and visualize Twitter data, let's look at a few ways to analyze this data quantitatively. For this section, we're going to focus on a few node-level network metrics.

### 2. Centrality: node importance

Say we'd like to understand who the most important user in the network is, or find the group of most influential users, those who are the elites of communication. This is where the concept of centrality comes in. Centrality is a metric in social network analysis which attempts to find the most important node in a given network. There are many different ways to do this, but we're going to focus on two types: degree centrality and betweenness centrality.

### 3. Degree centrality

Degree centrality is the simplest measure of centrality. It's a measure of how many edges are connected to a particular node. In a directed network, we can further decompose degree into in-degrees and out-degrees. Remember, Twitter networks are directed, which means that edges only go one way and are not mutual. In-degree is the number of edges going into a node, while out-degree is the number of edges going out of the node. You can think of this as the number of times someone is retweeted versus how much retweeting they do. In this particular network visualization, node sizes are proportional to the in-degree, which could represent how many times the user is retweeted.

### 4. Betweenness centrality

Betweenness centrality measures how many shortest paths between pairs of nodes need to pass through any given node. Think of an airport. It doesn't have to have many inbound planes to be important, but if it connects cities from many parts of the world to each other, it would have high betweenness centrality.

### 5. Printing highest centrality

For node-level metrics, we'll often want to see which users have the highest value for a particular type of centrality. Who's being retweeted the most, or who is bridging discussion networks? Doing this is straight-forward with pandas. We first calculate our metric, in this case, betweenness centrality. We then store it in a data frame. We have to use the `items` method to get the name of the user and the metric. We'll also pass an argument for column names. We can then sort using the `sort_values` method.

### 6. Centrality in different networks

The meaning of each centrality measure in Twitter networks depends highly on which network you are looking at. For retweet networks, high in-degree centrality signals someone who gets retweeted a bunch. High out-degree centrality signals someone does a lot of retweeting. And high betweenness centrality means someone bridges different types of topical or ideological communities. Meanwhile, in reply networks, high in-degree centrality signals someone who gets a lot of inbound messages, which could signal either agreement or disagreement. High out-degree centrality can signal someone who gets into many discussions. And high betweenness centrality may signal someone who bridges several different discussion communities.

### 7. The ratio

A last node-level metric that's particular to Twitter is the Reply-to-Retweet ratio, also just called the ratio. While this has yet to be systematically shown in scientific study, a popular Twitter belief is that a user or even a single tweet with a high ratio may signify that the user or their tweet is deeply unpopular due to users replying in disagreement. We can calculate this by creating a in-degree data frame for both the reply and retweet networks. We then join the two data frames into a single data frame using the `merge` method. Lastly, we can calculate the ratio by dividing the number of replies by the number of retweets.

### 8. Let's practice!

In the following exercises, we're going to generate these metrics for the State of the Union dataset. Pay special attention to which users end up in the top spots for each of the metrics. Are they the same, or are they different? 

