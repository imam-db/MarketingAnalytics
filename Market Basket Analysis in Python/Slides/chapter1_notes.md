## Market basket analysis

### 1. What is market basket analysis?

Hi! I'm Isaiah Hull and this is a course about market basket analysis using Python. In this first video, you'll learn what market basket analysis is and how it can be applied.

### 2. Selecting a bookstore layout

Let's start with a simple example. A small bookstore carries four genres: fiction, biography, poetry, and history. Due the store's layout, genres must be grouped into two pairs of sections. The owner does not know which pairs are best, so she approaches you and asks whether to pair fiction and biography together or fiction and poetry together. How would you answer this question? Well, you'd probably start by requesting the sales data.

### 3. Exploring transaction data

Fortunately, the store owner has recorded sales data in a format that is convenient for processing. Each row contains a transaction ID and a list of items. The items, in this case, are the genres of books a customer has purchased. Each list of items is called a transaction. One thing you may have noticed is that no items are repeated in a transaction. This is because we only consider unique items in market basket analysis.

### 4. What is market basket analysis?

So what is market basket analysis and what role does it play in the problem we've set up? First, it gives us the tools to identify which products are most frequently purchased together, such as biography and history books or fiction and poetry books. And second, it provides us with the means to construct useful recommendations based on these findings, such as which genres should be located close to each other in the store's layout.

### 5. The use cases of market basket analysis

Market basket analysis is a versatile analytical tool. It can be used to build Netflix-style recommendation engines, improve product recommendations for e-commerce sites, cross-sell products in retail stores, improve inventory management, and select items to upsell.

### 6. Using market basket analysis

Market basket analysis is structured around the use of something called "association rules." Association rules tell us that items are associated with each other, perhaps because they are purchased together frequently. Such rules take the form of an if-then relationship between two sets of items. The first is called the antecedent and the second is called the consequent. If, for instance, we find that purchasing fiction books is associated with purchasing biographies, as the transactions on the left of the slide indicate, then we state it as the following rule: "if fiction then biography."

### 7. Loading the data

Let's find some useful rules. We'll start by loading the bookstore data into a pandas DataFrame. We can do this by passing the file path to read csv. Using the head method of the DataFrame, we can view the first two observations. We have two columns: TID or transaction ID and the transaction itself. Throughout the first chapter, we'll make use of the same bookstore transaction data in each video, but will apply what we've learned in each video to a separate dataset that contains items from a small grocery store.

### 8. Building transactions

Now that we've loaded the data, we need to do some light preprocessing. We'll start by splitting each transaction, which is loaded as a string, into a list. We can do this for the entire column using a lambda function. We'll then convert that column into a list, yielding a list of lists.

### 9. Counting the itemsets

To get some intuition for what we've just done, let's print the first item in the list of transactions to see how it is formatted. As expected, it is a list of book genres associated with the first transaction. Now that we know how list items are formatted, let's pass a few to the count method, which will tell us how many transactions contain the exact same set of items that we pass to it. We'll first try a list that contains biography and fiction. As it turns out, only 218 transactions contain both genres.

### 10. Making a recommendation

If we try fiction and poetry, many more transactions appear to contain this list of genres. This suggests that we should recommend that the owner group fiction and poetry together, rather than fiction and biography.

### 11. Let's practice!

You've now learned the basic concepts behind market basket analysis. Let's put that knowledge to work in a set of exercises that make use of a new dataset. 

## The simplest metric

### 1. The simplest metric

Market basket analysis is centered around the identification and analysis of rules. As we saw earlier in the chapter, there are many rules, even if we limit ourselves to those about the association between two items. But what if we limit ourselves to useful rules? We'll do that in this video. To get those rules, we'll make use of a metric called support and a process called pruning.

### 2. Metrics and pruning

A metric is a measure of the performance of a rule. For example, under some metric, the rule "if humor then poetry" might map to the number 0-point-81. The same metric might yield 0-point-23 for "if fiction then travel." Pruning makes use of a metric to discard rules. For instance, we could keep only the those rules with a metric value of greater than 0-point-50. In the example we gave, we'd retain "if humor then poetry" and discard "if fiction then travel."

### 3. The simplest metric

The simplest metric is something called support, which measures the frequency with which itemsets appear in transactions. Support can also be applied to single items. For instance, in the small grocery store dataset, milk is one of the nine items that appear in transactions. We can compute milk's support as the number of transactions that contain milk, divided by the total number of transactions.

### 4. Support for language

As a concrete example, let's check the support for language in the first 10 transactions of the bookstore dataset. Language appears in transaction 1 and 3. Thus, the support value is 2 out of 10 or 0-point-2.

### 5. Support for {Humor} $\rightarrow$ {Language}

What if we instead want to check the support for an association rule, such as "if humor then language"? We would compute the share of transactions that contained both humor and language. In that case, we can see there is only one, so the support is 1 over 10 or 0-point-1. Notice that we would get the same value if we instead computed support for "if language then humor."
### 6. Preparing the data

Now that we've defined support, let's see how we can compute it in a more systematic way for all items. We'll focus on the first ten items in the bookstore dataset, which we'll assume have been imported as a pandas DataFrame and then converted to a list of lists called "transactions." We'll next import TransactionEncoder from the preprocessing submodule of mlxtend. After that, we'll instantiate a transaction encoder and use its fit method to identify the unique items in the dataset.

### 7. Preparing the data

We next use the transform method to construct an array of one-hot encoded transactions called onehot. Each column in onehot corresponds to one of the nine items in our dataset. If the item is present in a transaction, this is encoded as TRUE. Otherwise, it is FALSE. Finally, we'll use this array to construct a DataFrame. We'll use the item names as column headers and will recover them using the column underscore attribute of encoder.

### 8. Computing support for single items

We can now calculate the support metric by computing the mean over each column.

### 9. Computing support for multiple items

What if we want to compute the support for a rule, such as "if fiction then poetry"? We can create a new column in the DataFrame that is TRUE if both the fiction and poetry columns are true using numpy logical and, along with the two columns as arguments. We can again print the column means to get the support values.

### 10. Let's practice!

It's now time to compute support in some exercises! 