## A/B testing for marketing

### 1. A/B testing for marketing

In this lesson, we discuss how A/B testing is used within marketing departments.

### 2. What is A/B testing?

A/B testing refers to a randomized experiment which evaluates which variant performs better. In order for our tests to have meaning, we must have a clear control. The control should be something that currently exists and is running in production. Each variant in the test should have only one major change from the control; otherwise, it will be impossible to parse what led to the change in your key metrics. Prior to beginning a test, you must develop a hypothesis and determine which metric you are trying to impact. Always set key metrics ahead of running the test. It's easy to redefine success in retrospect, especially if you are under pressure to find a positive result. If you document success metrics ahead of time, you can maintain clarity around the success of the test.

### 3. Testing allows us to understand marketing impact

A big benefit of running A/B tests is we can be confident that the increase in the relevant metrics was due to the action we took in the test.

### 4. How long does a test need to run?

Your stakeholders will likely ask you how long a test needs to run. If a decision is needed by the end of the week, but a test would require two weeks to reach statistical significance, you should use a different methodology for this decision. Testing is popular because it can provide a definitive answer to controversial business questions, but it is only effective if tests are run properly.

### 5. Personalized email test

The focus of this chapter will be the A/B test that was run where half the emails were generic upsells to our product while the other half contained personalized messaging around individual usage of our site. Note that we will take a high-level look into how A/B tests are conducted in marketing departments. If you are interested in developing a more robust understanding of A/B testing and statistics, there are other excellent DataCamp courses where you can learn about these topics in depth.

### 6. Test allocation

Before we can begin assessing the impact of the test, we must ensure the test was executed correctly. The variant column contains the group each user was allocated to. We can do so by looking at how many people were allocated to the control and personalization variants. The code should look similar to how we sliced DataFrames and used groupby in previous lessons, except this time, we are counting the unique number of users who received each variant of email. Next, let's plot the results. Since we are comparing two groups, we'll use a bar chart.

### 7. Allocation plot

Allocation is relatively even, but not exactly the same. This will typically be the case. If you're worried allocation has gone wrong, there are statistical tests to determine the likelihood that the difference in allocation is due to random chance, but we will not explore that in this lesson. In this case, we can proceed with the assumption that there were no issues in the randomization process.

### 8. Setting up our data to evaluate the test

First, we ensure each user and variant has only one subscription outcome by using the groupby() and max() methods. We use the max() method on the 'converted' column since it's a boolean, and if there are multiple rows with False and True values, we want to consider the row where the user was converted, that is, True. Next, we unstack the DataFrame.

### 9. Setting up our data to evaluate the test

Finally, we create a Series of outcomes for both the control and the personalization variants. In this case, the series has a row for each user in the test which equals "True" if the user subscribed and "False" otherwise. We can use dropna() on each Series to only include conversion outcomes for all users in each variant.

### 10. Conversion rates

We can then calculate the conversion rate by taking the mean of each Series. Is this difference significant?

### 11. Let's get testing!

We will find out in the next lesson. For now, its time for you to analyze the personalization test! 


## Calculating lift and significance testing

### 1. Calculating lift & significance testing

In this lesson, we will talk about calculating lift and statistical significance.

### 2. Treatment performance compared to the control

The first question you'll want to answer when running a test is, "what's the lift?". In this case, what this means is, "Was the conversion rate higher for the treatment and by how much?". Lift is calculated by taking the difference between the treatment conversion rate and the control conversion rate divided by the control conversion rate. The result is the relative percent difference of treatment compared to control.

### 3. Calculating lift

To calculate the lift, we calculate the conversion rates of the control and the personalization groups. Then, we calculate lift using the equation from the previous slide, and we have our result! As you can see, the personalization variant improved on the control conversion rate by 194%. That's a huge improvement and a very good signal that we should consider running personalized emails again in the future. But before we get ahead of ourselves, let's talk statistical significance.

### 4. T-distribution

One way to calculate statistical significance is by conducting a two-sample t-test. A t-test uses the mean and the sample variance to determine the likelihood that the variation between the two samples occurred by chance. The image on the slide shows two overlapping sample distributions. The smaller the overlap between the two distributions, the more likely that there is a true difference between the two samples. I'm not going to explain the details of the t-test, but I highly recommend you do further research if you plan to run these tests at work.

    1 Identification of Timed Behavior Models for Diagnosis in Production Systems. Scientific Figure on ResearchGate.

### 5. P-values

The t-test gives us a t-statistic and a p-value which allows us to estimate the likelihood of finding a result at least as extreme as the treatment in our test. While it depends on sample size and the test, typically a t-statistic of 1.96 evaluates to a p-value of 0.05, which translates to a 95% significance level, a commonly used threshold for significance tests.

### 6. T-test in Python

To run a t-test in Python, you can use the ttest_ind() function from the stats module of the scipy package. The function takes a list of outcomes for each variant. In this case, the "outcomes" are whether or not each user converted. We can utilize the control and personalization Series we created in the previous lesson as the list of outcomes. This conveniently gives us both a t-statistic and a p-value. Remember, a p-value less than 0.05 is typically considered statistically significant at 95% significance level. Since the p-value here is indeed less than 0.05, we can be confident that the difference in conversion rates is statistically significant.

### 7. Let's practice!

It's time for you to calculate lift and run a t-test! 

## A/B testing & segmentation

### 1. A/B testing & segmentation

One of the most common pitfalls in A/B testing is assuming that a treatment equally affects everyone in a population.

### 2. Don't forget about segmentation!

Just like with any other kind of marketing, some treatments are particularly effective on users of a specific engagement level, age, race, or any other of a number of factors. It is important to break down results by various demographics in order to obtain a holistic understanding of the impact of the test. Not all customers are alike!

### 3. Personalization test segmented by language

Much like in the previous chapters, the primary challenge of segmentation is figuring out the best way to avoid repetitive work. In this lesson, we will use for loops to calculate lift and statistical significance across multiple segments of users. We begin by looping through all languages in the language_displayed column using numpy's unique() function and printing the language we are evaluating in this loop for reference.

### 4. Isolate the relevant data

First, we need to remake our dataset each time to isolate the data for only the selected language in each loop.

### 5. Isolate subscribers

Then, we ensure each user and variant has only one subscription outcome by using the groupby() and max() methods as we did in the first lesson of this chapter.

### 6. Isolate control and personalization

Next, we unstack the DataFrame and create a Series of outcomes for both the control and the personalization variants by dropping all missing values.

### 7. Full for loop

Finally, we can use the lift() and ttest() functions from the previous lesson to print the conversion rate and statistical significance for each language.

### 8. Results

Our for loop will run all the calculations for each unique language in the language_displayed column, and print the results. As you can see, the test performed very well among English and Spanish speakers, while the other language's results are not statistically significant. These are the kinds of differences across groups that are crucial to keep an eye out for.

### 9. Let's practice!

Go ahead and practice the same! 