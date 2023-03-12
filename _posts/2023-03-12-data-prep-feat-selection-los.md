---
layout: post
title: Data Preparation & Feature Selection for Predicting Length of Stay
image: "/posts/cleaning.jpg"
tags: [Data Cleaning, Healthcare, Python, RFECV]
---

This project uses Python to demonstrate the steps for cleaning and preparing a dataset with multiple variables and data types, and then selecting the appropriate features that contribute to a specific outcome; in this case, hospital length of stay.

The dataset contains about 32,000 patients with data consisting of patient demographics, diagnoses, medical history, and vital signs. This data is fictitious, but represents true clinical outcomes and values. 


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

It is assumed that most people working with data understand the importance of cleaning and preparing a dataset before attempting to use the dataset in a Machine Learning project. Dealing with extreme or missing values, and correcting / converting data types are common steps in this process. Datasets often have variables that are unnecessary for making predictions on the outcome under investigation, and steps should therefore be completed for removing these. 

This dataset came to me as part of a coding test for a position I elected not to pursue. The test requirement was to prepare the dataset for use with predictive models and specifically, for predicting hospital length of stay as the outcome variable. There are a number of clinical and non-clinical input variables included. While I did well enough at the time with the task, in reviewing the work I submitted then, I realized I could have made it more efficient and as I began building this portfolio, it occurred to me to revisit these steps as a project to include in my portfolio, as data cleaning and feature selection are regular steps performed by data scentists and these steps also demonstrate a person's thought process when analyzing raw data needing some cleansing.

<br>
<br>
### Actions <a name="overview-actions"></a>

This project focuses on completing the steps for dealing with extreme values (i.e., outliers), missing values, converting categorical values, and performing feature scaling before using a technique called Recursive Feature Elimination with Cross-Validation to select features, using the RFECV module from the Sci-Kit Learn library.  Full details of this module and the concept behind its use are in the dedicated section below. Coding samples are included.

My first step with any project is to import the required packages that I'll use. I then import the dataset, generally as a Comma-Separated Values (CSV) file, into a dataframe using the Pandas library. 

**Note:** while the data preparation steps typically occur after the dataframe is constructed, if I'm the person retrieving the raw data (i.e., writing the SQL), I'll often do some of the preparation in SQL first. Examples include compressing the data to the appropriate cardinality, grouping the data into appropriate groups, or creating Boolean (1/0) columns from the raw data values. I may have to repeat these steps after creating the dataframe in Python, but I often save time by doing as much of this in SQL first.

I next perform the following steps:

**Data Preparation** 

* Check the shape of the dataframe
* Check for duplicate rows
* Drop the duplicate rows
* Recheck the shape to confirm the previous drop
* Check for missing values
* Determine the percentage of missing values for each variable
* Drop or impute missing values
* Review the data types of the variables
* Perform data type conversions / convert categorical variables to numeric
* Check for extreme (outlier) values
* Perform feature scaling, if indicated
* Shuffle the data, if indicated

**Note:**the above steps are outlined at a high level and describe how I go about preparing data, but are not meant to imply any absolutes and realize that others may have their own process. Some of the above steps can also occur in different sequences depending on the project needs.

**Feature Selection** 


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

___

