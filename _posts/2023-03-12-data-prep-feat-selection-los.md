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
    - [Results](#overview-results)
    - [Growth/Next Steps](#overview-growth)
- [01. Data Overview](#data-overview)
- [02. K-Means](#kmeans-title)
    - [Concept Overview](#kmeans-overview)
    - [Data Preprocessing](#kmeans-preprocessing)
    - [Finding A Good Value For K](#kmeans-k-value)
    - [Model Fitting](#kmeans-model-fitting)
    - [Appending Clusters To Customers](#kmeans-append-clusters)
    - [Segment Profiling](#kmeans-cluster-profiling)
- [03. Application](#kmeans-application)
- [04. Growth & Next Steps](#growth-next-steps)

___

