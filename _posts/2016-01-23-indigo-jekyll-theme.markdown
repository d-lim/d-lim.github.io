---
title: ":Human Mobility Prediction"
layout: post
date: 2017-11-17 09:00
tag: jekyll
image: 
headerImage: false
projects: true
hidden: true # don't count this post in blog pagination
description: "Explored predicting human mobility by analysing a year’s worth of taxi trips and generating models to predict passenger destinations"
category: project
author: derrick
externalLink: false
---

Even in this age where communication technology is instantaneous, many of us still use or get the response: “If only I knew you are going to be here, I would have…”. It is inherent that most people do not constantly update others where they are heading towards, especially for businesses, they usually are not aware of which customers will be patronizing them for the day. While business do not have explicit information on their customers destinations, they have 

For the complete set of Jupyter notebooks with Python code and detailed Markdowns of my process through this project, please refer to my [Github](https://github.com/d-lim/Projects)

---

Data Origin:
I am fortunate to be able to gather a year’s worth of taxi trips from the city of Porto from a competition hosted on [Kaggle](https://www.kaggle.com/c/pkdd-15-predict-taxi-service-trajectory-i/data). The objective of the competition is to predict the destination of a taxi ride while the taxi is on the move. Therefore, I have decided to use this to test out my hypothesis on predicting human mobility. 

The metadata are as summarize in Table 1 below. 

![Metadata](assets/images/Metadata.png)





Approach Taken:

- Extract, transform & load (ETL) data onto a VM Instance on Google Cloud
- Perform wrangling and exploratory data analysis (EDA)
- Create Train/Validation datasets
- Create machine learning models using train data
	- Regression  - Tree Based Models
	- Classification – Neural Network 
- Validate models using the mean haversine distance and thereafter predict on test data set


---

[Check it out](http://sergiokopplin.github.io/indigo/) here.
If you need some help, just [tell me](http://github.com/sergiokopplin/indigo/issues).
