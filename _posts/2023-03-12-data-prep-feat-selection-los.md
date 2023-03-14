---
layout: post
title: Under Construction Data Preparation & Feature Selection for Predicting Length of Stay
image: "/posts/cleaning.jpg"
tags: [Data Cleaning, Healthcare, RFECV]
---

This project uses Python to demonstrate the steps for preparing a dataset with multiple variables and data types, and then selecting the appropriate features that contribute to a specific outcome; in this case, hospital length of stay.

The dataset contains about 32,000 records with data consisting of patient demographics, diagnoses, medical history, and vital signs. This data is fictitious, but represents true clinical outcomes and values. 


# Table of contents

- [00. Project Overview](#overview-main)
    - [Context](#overview-context)
    - [Actions](#overview-actions)
    - [Results & Discussion](#overview-results)
- [01. Concept Overview](#concept-overview)
- [02. Data Overview & Preparation](#data-overview)
- [03. Applying RFECV for Feature Selection](#rfecv-application)
- [04. Analyzing The Results](#rfecv-results)
- [05. Discussion](#discussion)

___

# Project Overview  <a name="overview-main"></a>

### Context <a name="overview-context"></a>

It is assumed that most people working with data understand the importance of cleaning and preparing a dataset before attempting to use the dataset in a Machine Learning project. Dealing with extreme or missing values, and correcting / converting data types are common steps in this process. Datasets often have variables that are unnecessary for making predictions on the outcome under investigation and steps should therefore be completed for removing these. 

This dataset came to me as part of a coding test for a position I chose not to pursue. The test requirement was to prepare the dataset for use with predictive models and specifically, for predicting hospital length of stay as the outcome variable. There are multiple clinical and non-clinical input variables included. While I did well enough at the time with the task, looking back on it, I realized I could have made it more efficient, so when I began building this portfolio, it occurred to me to revisit this as a project for inclusion.

<br>
### Actions <a name="overview-actions"></a>

This project focuses on completing the steps for dealing with extreme values (i.e., outliers), missing values, converting categorical values, and performing feature scaling before using a technique called Recursive Feature Elimination with Cross-Validation (RFECV) to perform Feature Selection. For the RFECV process, I use the RFECV module from the Sci-Kit Learn library.  Full details of this module and the concept behind its use are in the dedicated section below. Coding samples are included.

My first step with any project is to import the required packages that I'll use. I then import the dataset, generally as a Comma-Separated Values (CSV) file, into a dataframe using the Pandas library. 

**Note:** while the data preparation steps typically occur after the dataframe is constructed, if I'm the person retrieving the raw data (i.e., writing the SQL), I'll often do some of the preparation in SQL first. Examples include compressing the data to the proper cardinality, grouping the data into appropriate groups, or creating Boolean (1/0) columns from the raw data values. I may have to repeat these steps after creating the dataframe in Python, but I often save time by doing as much of this in SQL first.

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


**Feature Selection with RFECV** 

* Create variables for inputs and outputs
* Instantiate the regressor and selector objects
* Fit the variables
* Identify the optimal number of features
* Identify the chosen features
* Plot the results for the optimal number 
* Sort and rank the chosen features by an appropriate statistic

**Note:** the above steps are outlined at a high level and describe how I generally prepare data and perform feature selection, but are not meant to imply any absolutes. It is important to recognize that others may have their own, albeit similar, process. Some of the above steps may also occur in different sequences depending on the project needs.
<br>
### Results & Discussion <a name="overview-results"></a>

The initial dataframe presents with 32,000 rows and 221 columns (features). There are 8790 duplicate rows and dropping these results in 23,210 remaining rows. Checking for missing values results in input variables missing between 5 - 98% of values. Because of this widespread variability, I chose not to impute any missing values, even for input variables with a low percentages. Because this is clinical data, if this were being used for a maching learning model, it is likely more appropriate to use factual data, as opposed to imputed, since no two patients are 100% alike. Therefore, I chose to drop any features that contained missing values. 

This action reduced the number of columns from 221 to 49! The upside to such an action is the dimensionality reduction that occurs, which likely results in less overall complexity, less required storage space, and improved computational time. The downside to this includes potentially losing important data which can affect model training later. Still, in reviewing the columns that remained after dropping, they all seemed to be clinically relevant to the outcome variable and I felt satisfied with the decision, especially knowing that this can always be revisited and with a different approach used for handling missing data. This decision-making example reflects an important skill for a Data Scientist, whether performing it as a domain subject-matter expert, or with the appropriate stakeholder feedback, or both!

With the remaining columns, I dropped an unnecessary column for encounter ID as this column had no value other than to create noise and take up feature space. From there, I checked for exteme values, or outliers. I used a Boxplot approach for this where I created a for loop that passes over all of the remaining continuous numeric variables and identifies which ones are outliers based on threshold borders set around the 25% and 75% quartiles, or the interquartile range (IQR), from the respective column values. The decision to keep or remove outliers from the data always depends on the data and what you want to do with it. From the outliers assessment, I determined that, aside from Age, each of the other continuous numeric variables has a small percentage of outliers present. I chose to include these for this project as these values would contribute to a patient's length of stay, and so, are clinically appropriate. 

Next, I shuffled the data and converted variables that presented as numeric, but are actually categorical. I then performed One Hot Encoding on the categorical variables to convert these to Boolean (1/0) values. I used the OneHotEncoder module from Sci-Kit Learn for this step. This increased the number of input variables to 60.

Next, I scaled the remaining numeric data to a Normalized scale, so that these features would not be inappropriately weighted higher than the other features. I used the MinMaxScaler module from Sci-Kit Learn to do this. I created a new normalized dataframe after the scaling action.

Finally, with the data in a prepared state in a new dataframe, I performed the steps for Feature Selection listed above. As these steps are more technical, I describe those more in the "Applying RFECV for Feature Selection" section. 

As the output variable (Length of Stay) is a continuous numeric variable and because I chose to keep the outliers in the dataset, I chose a Random Forest Regressor for use with the RFECV selector object. Random Forests, by nature, are less sensitive to outliers, so it made sense to use this for the Regressor object used by the RFECV selector. The selector identified 13 inputs as the optimal number of features and I ranked these by their mean cross-validation test score. The input variable, "Chronic Condition Count', had the highest score and a rank of 1 with "Age" coming in 13th with the lowest score. 

<br>

# Concept Overview  <a name="concept-overview"></a>

#### Data Preparation & Feature Selection

There are likely many definitions and variations on what it means to prepare data for analysis and machine learning depending on the source. I put my own spin on a definition that I liked from Databricks where they define Data Preparation (also referred to as “data preprocessing”) as the process of transforming raw data, so that data analysts and data scientists can run it through machine learning algorithms to uncover insights or make predictions. Another benefit to having properly prepared, or “cleaned”, data is that machine learning models will perform better than with messy, or unprepared data. Messy data also probbly has many meanings for different audiences, but one inerpretation is that messy data includes missing values, unscaled data, unstructured data, variables with incorrect data types, and whitespace. This is not an exhaustive list and is simply meant to provide a some high-level reasoning for why data needs to be processed before a machine learning algorithm, let alone a human, can interpret or use it.

A basic and easy to understand definition for Feature Selection comes from Simplilearn where they define it as “the method of reducing the input variable to your model by using only relevant data and getting rid of noise in data. It is the process of automatically choosing relevant features for your machine learning model based on the type of problem you are trying to solve.” Feature Selection is similar to Data Preparation in that there is the need for optimal model performance and interpretability, both of which occur when the “noisy” inputs are removed.

<br>
___

# Data Overview and Preparation <a name="data-overview"></a>

The initial dataframe presents with 32,000 rows and 221 columns (features). Through a series of data preparation steps, I arrived at 48 remaining feature columns. Performing One Hot Encoding on the categorical variables increased the number of features to 60. I then scaled all features to a normalized scale, given that most already consisted of 1/0 values. These steps are included in the Jupyter Notebook for the project [here](https://github.com/jlaurellax/project_notebooks/blob/master/Data%20Preparation%20%26%20Feature%20Selection%20with%20Healthcare%20Data.ipynb).








