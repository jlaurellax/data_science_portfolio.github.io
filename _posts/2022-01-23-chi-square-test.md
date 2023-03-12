---
layout: post
title: Assessing Campaign Performance Using Chi-Square Test For Independence
image: "/posts/ab-testing-title-img.png"
tags: [AB Testing, Hypothesis Testing, Chi-Square]
---

In this project we apply Chi-Square Test For Independence (a Hypothesis Test) to assess the performance of two types of mailers that were sent out to promote a new service! 

# Table of contents

- [00. Project Overview](#overview-main)
    - [Context](#overview-context)
    - [Actions](#overview-actions)
    - [Results & Discussion](#overview-results)
- [01. Concept Overview](#concept-overview)
- [02. Data Overview & Preparation](#data-overview)
- [03. Applying Chi-Square Test For Independence](#chi-square-application)
- [04. Analyzing The Results](#chi-square-results)
- [05. Discussion](#discussion)

___

# Project Overview  <a name="overview-main"></a>

### Context <a name="overview-context"></a>

Earlier in the year, the client, a grocery retailer, ran a campaign to promote their new "Delivery Club" - an initiative that costs a customer $100 per year for membership, but offers free grocery deliveries rather than the normal cost of $10 per delivery.

For the campaign promoting the club, customers were put randomly into three groups - the first group received a low quality, low cost mailer, the second group received a high quality, high cost mailer, and the third group were a control group, receiving no mailer at all.

The client knows that customers who were contacted, signed up for the Delivery Club at a far higher rate than the control group, but now want to understand if there is a significant difference in signup rate between the cheap mailer and the expensive mailer.  This will allow them to make more informed decisions in the future, with the overall aim of optimizing campaign ROI!

<br>
<br>
### Actions <a name="overview-actions"></a>

For this test, as it is focused on comparing the *rates* of two groups - the project applies the Chi-Square Test For Independence.  Full details of this test can be found in the dedicated section below.

**Note:** Another option when comparing "rates" is a test known as the *Z-Test For Proportions*.  While this test could be used, I chose the Chi-Square Test For Independence because:

* The resulting test statistic for both tests will be the same
* The Chi-Square Test can be represented using 2x2 tables of data - meaning it can be easier to explain to stakeholders
* The Chi-Square Test can extend out to more than 2 groups - meaning the client can have one consistent approach to measuring signficance

From the *campaign_data* table in the client database, I isolated customers that received "Mailer 1" (low cost) and "Mailer 2" (high cost) for this campaign, and excluded customers who were in the control group.

I set the hypotheses and Acceptance Criteria for the test, as follows:

**Null Hypothesis:** There is no relationship between mailer type and signup rate. They are independent.
**Alternate Hypothesis:** There is a relationship between mailer type and signup rate. They are not independent.
**Acceptance Criteria:** 0.05

The Chi-Square Test For Independence requires the data to be aggregated to a 2x2 matrix for *signup_flag* by *mailer_type* and then fed into the algorithm (using the *scipy* library) to calculate the Chi-Square Statistic, p-value, Degrees of Freedom, and expected values.

<br>
<br>

### Results & Discussion <a name="overview-results"></a>

Based upon the observed values, the observer is provided some context with the sign-up rate of each group:

* Mailer 1 (Low Cost): **32.8%** signup rate
* Mailer 2 (High Cost): **37.8%** signup rate

However, the Chi-Square Test provides the following statistics:

* Chi-Square Statistic: **1.94**
* p-value: **0.16**

The Critical Value for the specified Acceptance Criteria of 0.05 is **3.84**

Based upon these statistics, the decision is to retain the null hypothesis, and conclude that there is no relationship between mailer type and signup rate.

In other words - while the higher cost Mailer 2 had a higher signup rate (37.8%) than the lower cost Mailer 1 (32.8%) it appears that this difference is not significant, at least at the Acceptance Criteria of 0.05.

Without running this Hypothesis Test, the client may have concluded that they should always look to go with higher cost mailers - and from what this test yields, that may not be a great decision.  It would result in them spending more, but not *necessarily* gaining any extra revenue as a result.

The results here also do not say that there *definitely isn't a difference between the two mailers* - just not to make any rigid conclusions *at this point*.  

Running more A/B Tests like this, gathering more data, and then re-running this test may provide more insight.

<br>
<br>

___

# Concept Overview  <a name="concept-overview"></a>

<br>
#### A/B Testing

An A/B Test can be described as a randomized experiment containing two groups, A & B, that receive different experiences. Within an A/B Test, we look to understand and measure the response of each group - and the information from this helps drive future business decisions.

Application of A/B testing can range from testing different online ad strategies, different email subject lines when contacting customers, or testing the effect of mailing customers a coupon, vs a control group.  Companies like Amazon are running these tests in an almost never-ending cycle, testing new website features on randomised groups of customers...all with the aim of finding what works best so they can stay ahead of their competition.  Reportedly, Netflix will even test different images for the same movie or show, to different segments of their customer base to see if certain images pull more viewers in.

<br>
#### Hypothesis Testing

A Hypothesis Test is used to assess the plausibility, or likelihood of an assumed viewpoint based on sample data - in other words, a it helps us assess whether a certain view we have about some data is likely to be true or not.

There are many different scenarios we can run Hypothesis Tests on, and they all have slightly different techniques and formulas - however they all have some shared, fundamental steps & logic that underpin how they work.

<br>
**The Null Hypothesis**

In any Hypothesis Test, we start with the Null Hypothesis. The Null Hypothesis is where we state our initial viewpoint, and in statistics, and specifically Hypothesis Testing, our initial viewpoint is always that the result is purely by chance or that there is no relationship or association between two outcomes or groups.

<br>
**The Alternate Hypothesis**

The aim of the Hypothesis Test is to look for evidence to support or reject the Null Hypothesis.  If we reject the Null Hypothesis, that would mean we’d be supporting the Alternate Hypothesis.  The Alternate Hypothesis is essentially the opposite viewpoint to the Null Hypothesis - that the result is *not* by chance, or that there *is* a relationship between two outcomes or groups.

<br>
**The Acceptance Criteria**

In a Hypothesis Test, before we collect any data or run any numbers - we specify an Acceptance Criteria.  This is a p-value threshold at which we’ll decide to reject or support the null hypothesis.  It is essentially a line we draw in the sand saying "if I was to run this test many many times, what proportion of those times would I want to see different results come out, in order to feel comfortable, or confident that my results are not just some unusual occurrence."

Conventionally, we set our Acceptance Criteria to 0.05 - but this does not have to be the case.  If we need to be more confident that something did not occur through chance alone, we could lower this value down to something much smaller, meaning that we only come to the conclusion that the outcome was special or rare if it’s extremely rare.

So to summarise, in a Hypothesis Test, we test the Null Hypothesis using a p-value and then decide it’s fate based on the Acceptance Criteria.

<br>
**Types Of Hypothesis Test**

There are many different types of Hypothesis Tests, each of which is appropriate for use in differing scenarios - depending on a) the type of data that you’re looking to test and b) the question that you’re asking of that data.

In the case of our task here, where we are looking to understand the difference in sign-up *rate* between two groups - we will utilize the Chi-Square Test For Independence.

<br>
#### Chi-Square Test For Independence

The Chi-Square Test For Independence is a type of Hypothesis Test that assumes observed frequencies for categorical variables will match the expected frequencies.

The *assumption* is the Null Hypothesis, which as discussed above is always the viewpoint that the two groups will be equal.  With the Chi-Square Test For Independence we look to calculate a statistic which, based on the specified Acceptance Criteria, will mean we either reject or support this initial assumption.

The *observed frequencies* are the true values that we’ve seen.

The *expected frequencies* are essentially what we would *expect* to see based on all of the data.

**Note:** Another option when comparing "rates" is a test known as the *Z-Test For Proportions*.  While, we could absolutely use this test here, we have chosen the Chi-Square Test For Independence because:

* The resulting test statistic for both tests will be the same.
* The Chi-Square Test can be represented using 2x2 tables of data - meaning it can be easier to explain to stakeholders.
* The Chi-Square Test can extend out to more than 2 groups - meaning the business can have one consistent approach to measuring signficance.

___

<br>
# Data Overview & Preparation  <a name="data-overview"></a>

In the client database, there is a *campaign_data* table which shows which customers received each type of "Delivery Club" mailer, which customers were in the control group, and which customers joined the club as a result.

This task seeks to find evidence that the Delivery Club signup rate for customers that received "Mailer 1" (low cost) was different to those who received "Mailer 2" (high cost), so from the *campaign_data* table, I extracted customers in those two groups, and excluded customers in the control group.

The code below lists these steps:

* Load in the Python libraries we require for importing the data and performing the chi-square test (using scipy).
* Import the required data from the *campaign_data* table.
* Exclude customers in the control group, giving us a dataset with Mailer 1 & Mailer 2 customers only.

<br>
```python

# install the required python libraries
import pandas as pd
from scipy.stats import chi2_contingency, chi2

# import campaign data
campaign_data = ...

# remove customers who were in the control group
campaign_data = campaign_data.loc[campaign_data["mailer_type"] != "Control"]

```
<br>
A sample of this data (the first 10 rows) can be seen below:
<br>
<br>

| **customer_id** | **campaign_name** | **mailer_type** | **signup_flag** |
|---|---|---|---|
| 74 | delivery_club | Mailer1 | 1 |
| 524 | delivery_club | Mailer1 | 1 |
| 607 | delivery_club | Mailer2 | 1 |
| 343 | delivery_club | Mailer1 | 0 |
| 322 | delivery_club | Mailer2 | 1 |
| 115 | delivery_club | Mailer2 | 0 |
| 1 | delivery_club | Mailer2 | 1 |
| 120 | delivery_club | Mailer1 | 1 |
| 52 | delivery_club | Mailer1 | 1 |
| 405 | delivery_club | Mailer1 | 0 |
| 435 | delivery_club | Mailer2 | 0 |

<br>
In the DataFrame we have:

* customer_id
* campaign name
* mailer_type (either Mailer1 or Mailer2)
* signup_flag (either 1 or 0)

___

<br>
# Applying Chi-Square Test For Independence <a name="chi-square-application"></a>

<br>
#### State Hypotheses & Acceptance Criteria For Test

The very first thing we need to do in any form of Hypothesis Test is state our Null Hypothesis, our Alternate Hypothesis, and the Acceptance Criteria (more details on these in the section above).

In the code below we code these in explcitly & clearly so we can utilize them later to explain the results.  We specify the common Acceptance Criteria value of 0.05.

```python

# specify hypotheses & acceptance criteria for test
null_hypothesis = "There is no relationship between mailer type and signup rate.  They are independent"
alternate_hypothesis = "There is a relationship between mailer type and signup rate.  They are not independent"
acceptance_criteria = 0.05

```

<br>
#### Calculate Observed Frequencies & Expected Frequencies

As mentioned in the section above, in a Chi-Square Test For Independence, the *observed frequencies* are the true values seen. In other words, the actual rates per group in the data itself.  The *expected frequencies* are what we would *expect* to see based on *all* of the data combined.

The below code:

* Summarizes the dataset to a 2x2 matrix for *signup_flag* by *mailer_type*
* Based on this, it calculates the:
    * Chi-Square Statistic
    * p-value
    * Degrees of Freedom
    * Expected Values
* Prints out the Chi-Square Statistic & p-value from the test.
* Calculates the Critical Value based upon our Acceptance Criteria & the Degrees Of Freedom.
* Prints out the Critical Value.

```python

# aggregate our data to get observed values
observed_values = pd.crosstab(campaign_data["mailer_type"], campaign_data["signup_flag"]).values

# run the chi-square test
chi2_statistic, p_value, dof, expected_values = chi2_contingency(observed_values, correction = False)

# print chi-square statistic
print(chi2_statistic)
>> 1.94

# print p-value
print(p_value)
>> 0.16

# find the critical value for our test
critical_value = chi2.ppf(1 - acceptance_criteria, dof)

# print critical value
print(critical_value)
>> 3.84

```
<br>
Based upon the observed values, some context provided for the sign-up rate of each group is:

* Mailer 1 (Low Cost): **32.8%** signup rate
* Mailer 2 (High Cost): **37.8%** signup rate

From this, it appears that the higher cost mailer does lead to a higher signup rate.  The results from the Chi-Square Test provide more information about whether or not this difference is robust, or if it might have occured by chance.

The results are a Chi-Square Statistic of **1.94** and a p-value of **0.16**.  The critical value for the specified Acceptance Criteria of 0.05 is **3.84**

**Note** When applying the Chi-Square Test above, I use the parameter *correction = False* for applying what is known as the *Yate's Correction* which is applied when the Degrees of Freedom are equal to one.  This correction helps to prevent overestimation of statistical signficance in this case.

___

<br>
# Analyzing The Results <a name="chi-square-results"></a>

From the results above, since the resulting p-value of **0.16** is *greater* than the Acceptance Criteria of 0.05, the decision is to _retain_ the Null Hypothesis and conclude that there is no significant difference between the signup rates of Mailer 1 and Mailer 2.

The same conclusion is made based upon the Chi-Square statistic of **1.94** being _lower_ than the Critical Value of **3.84**.

To make this script more dynamic, I added code to automatically interpret the results and explain the outcome...

```python

# print the results (based upon p-value)
if p_value <= acceptance_criteria:
    print(f"As our p-value of {p_value} is lower than our acceptance_criteria of {acceptance_criteria} - we reject the null hypothesis, and conclude that: {alternate_hypothesis}")
else:
    print(f"As our p-value of {p_value} is higher than our acceptance_criteria of {acceptance_criteria} - we retain the null hypothesis, and conclude that: {null_hypothesis}")

>> As our p-value of 0.16351 is higher than our acceptance_criteria of 0.05 - we retain the null hypothesis, and conclude that: There is no relationship between mailer type and signup rate.  They are independent


# print the results (based upon p-value)
if chi2_statistic >= critical_value:
    print(f"As our chi-square statistic of {chi2_statistic} is higher than our critical value of {critical_value} - we reject the null hypothesis, and conclude that: {alternate_hypothesis}")
else:
    print(f"As our chi-square statistic of {chi2_statistic} is lower than our critical value of {critical_value} - we retain the null hypothesis, and conclude that: {null_hypothesis}")
    
>> As our chi-square statistic of 1.9414 is lower than our critical value of 3.841458820694124 - we retain the null hypothesis, and conclude that: There is no relationship between mailer type and signup rate.  They are independent

```
<br>
As the outputs of these print statements demonstrate, we do indeed retain the null hypothesis.  there is not enough evidence that the signup rates for Mailer 1 and Mailer 2 were different - and the conclusion is that there is no significant difference.

___

<br>
# Discussion <a name="discussion"></a>

While the higher cost Mailer 2 had a higher signup rate (37.8%) than the lower cost Mailer 1 (32.8%), it appears that this difference is not significant, at least at the Acceptance Criteria of 0.05.

Without running this Hypothesis Test, the client may have concluded that they should always look to go with higher cost mailers - and the results of this test, that may not be a great decision.  It would result in them spending more, but not *necessarily* gaining any extra revenue as a result.

The results here also do not say that there *definitely isn't a difference between the two mailers* - they are only advising not to make any rigid conclusions *at this point*.  

Running more A/B Tests like this, gathering more data, and then re-running this test may provide more overall insight!
