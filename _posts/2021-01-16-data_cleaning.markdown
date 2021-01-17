---
layout: post
title:      "Data Cleaning"
date:       2021-01-17 00:53:37 +0000
permalink:  data_cleaning
---


![](https://iterable.com/wp-content/uploads/2019/06/061319_Migration-Pt-2_768x512-1.png)



When I am dealing with fresh new data, I get two mixed feelings. Part of me is excited to learn about the data; the other part of me is anxious about how much clean up I need to do. Just as much as we wish data to be perfect to bring out the most accurate result, data cleaning is a process that we, data scientists, cannot dodge. There is a famous quote about it.

> "If we cannot avoid it, enjoy it."

As I went through the course of learning, here are a few cleaning tools that I found which helped me clean up my data.

## Business and Data Understanding

Cleaning up can be described as an act of removing the dirt, and organizing. In data cleaning, we have to know which part of the data we need to remove and organize. To determine whether data should be removed or organized, we need to understand the value of the information to the business or subject. A key aspect here is to build the judgemental decision to choose items to be removed or organized. Upon the understanding of the weight of importance, we can then start the cleaning process. 

## Clean Up Process

Once your understanding of business is set, it is time for clean up. In the cleaning process, we need to note a few points and ask the following question. Which part of data is incomplete, incorrect, corrupted, out formatted, mislabeled, or duplicated?

## Incomplete Data: 
Some parts of your data might have null values. Since you cannot completely ignore missing data, because it will impact your model. To deal with incompleteness, we can ask these questions and decide your answer.
1. What observations do incomplete data have?
2. Will removing incomplete data results in lose critical information?
3. Can incomplete data be filled with other observations?
4. Should incomplete data be used as null values? In other words, does incompleteness has any meaning to the business?

## Incorrect, Corrupted, Out Formatted, Mislabeled Data: 
Your data can have errors like typos, unit mismatching, incorrect capitalization, inconsistent labeling, etc. To deal with these structural errors key is to unify your data. Use conversion and transformation methods to put your data in line with each other and have them find uniformity, accuracy, and consistency.

## Duplicate Data: 
Duplication could happen quite often during data gathering. A duplication check should be one of the first steps in your data cleaning processes as it is a simple procedure. If the duplication check is done in the middle of the cleaning process, it could cause quite confusion to your cleaning. Once thought unimportant observation could contain valuable information. Altered data could create duplication in data that did not exist in the original.

## Outliers: 
If the outliers are caused by incorrect data entry, they can simply be removed after careful investigation. Outliers can be a valid data entry and hold important information. As they hold important information and small in number, outliers can be picked and studied individually. In machine learning, you want to generalized the data, and having outliers can create confusion in your model. You can manage to reduce outliers with cleaning processes like log transformations, scaling, and normalization.

## Wrap-Up

There are many benefits to cleaning your data. I can say that the number one reason what data scientists practice data cleaning to bring out the best result in their models. I hope the methods in this blog helps you clean your data and improve your results.

![](https://twt-thumbs.washtimes.com/media/image/2016/09/08/HomepageCleaningProductsSolutions-1_c0-0-1080-630_s561x327.jpg?2dc3d8e6f99b59a8b04875792135a074a2b1d578)

