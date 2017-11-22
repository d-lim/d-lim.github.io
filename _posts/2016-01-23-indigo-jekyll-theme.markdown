---
title: "Human Mobility Prediction"
layout: post
date: 2017-11-17 09:00
tag:
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

The metadata is as summarize below: 

<img src="../assets/images/Metadata.png" alt="Metadata" style="width: 1500px;"/>


The evaluation method is the Mean Haversine Distance. The Haversine Distance is commonly used in navigation. It measures distances between two points on a sphere based on their latitude and longitude.

The Harversine Distance between the two locations can be computed as follows:

<img src="../assets/images/json_loads.png" alt="Harversine" style="width: 1500px;"/>

where ϕ is the latitude, λ is the longitude, 

d is the distance between two points, and r is the Earth’s radius (6371km). 

---

Approach Taken:

- Extract, transform & load (ETL) data onto a VM Instance on Google Cloud
- Perform wrangling and exploratory data analysis (EDA)
- Create Train/Validation datasets
- Create machine learning models using train data
	- Regression  - Tree Based Models
	- Classification – Neural Network 
- Validate models using the mean haversine distance and thereafter predict on test data set


---

ETL:

As the dataset consists of over 1.7 million trips, functions are written to efficiently extract, transform and load based on a toy dataset consisting of 50,000 random trips. An interesting point is that “POLYLINE” which contains all the datatype of the GPS coordinates is a string instead of a list, and coordinates (such as start and end coordinates) which is required could not be extracted. Therefore, the “POLYLINE” feature is transformed using the functions written below and the start and end coordinates are extracted.

<img src="../assets/images/json_loads.png" alt="transform_coordinates" style="width: 1500px;"/>

In addition to transforming the “POLYLINE” feature, duplicates and missing data were also removed. Also, the timestamp data is converted into date and time, with features, quarter hour of a day, day of the week and week of year.  After establishing the initial clean processing, these were applied to the main dataset through the use of a virtual machine on Google Cloud. 

----

EDA: 

With the full dataset that has been initially cleaned, exploratory work can be carried out. 

As the predictions is on taxi destinations, all the end points are plotted out to better visualize them. 

<img src="../assets/images/initial_ends.png" alt="initial_ends" style="width: 1500px;"/>

From the plots of the destination coordinates it is observed there are a couple of trips which ended very far away from the majority, are concentrated into a small blob, seen in the figure above. It is important that our training data destinations do not contain end points which are very far away from majority of the end points as this will reduce the accuracy of the model predictions. In order to determine which are the outliers, a statistical approach, using the interquartile ranges of both the latitudes and longitudes are taken.  


<img src="../assets/images/1-5IQR_Ends.png" alt="3IQR_Ends" style="width: 1500px;"/> <img src="../assets/images/3IQR_Ends.png" alt="1-5IQR_Ends" style="width: 1500px;"/> 


Form the figures above, the points in green are the where most trips end. There is a high concentration of green points which seems to indicate that most trips end at the city centre. The plot with 1.5xIQR seems to a remove a fair bit of popular destinations after comparing the points at the left, top and bottom of both plots. Therefore, 3xIQR is chosen to remove outliers.

 Two new features, duration of trips and harversine distance between the start points are engineered with the code below. Trips with NIL duration and NIL distance are removed as that indicated that the taxi is stationary, most like it was due to the driver accidentally activating meter, logging a trip. 

Generally distance travelled is proportionally related to duration taken, a scatter is used to visualize this.

<img src="../assets/images/Dis_Dur_Noisy.png" alt="Dis_Dur_Noisy" style="width: 1500px;"/> 

From above, It seems that there is little relationship between both distance and duration. This could be explained as taxis do not travel in a straight path and there are also plenty of turns being made when travelling in a city.

However, on the left hand side of the plot, there are trips which covered a long distance over a short period of time, over 1100km in under 3hours and on the right hand side of the plot, trips which took a long time to complete but only covered a short distance are observed, trips with duration of over 10hours but travelled less than 10km.

In order to removed the noise identified, the same statistical approach using interquartile ranges is utilized. However, as it a noted both distributions of distance and duration are very right skewed, normalization by logging both features is required first. 

<img src="../assets/images/Dis_Dur_Clean.png" alt="Dis_Dur_Clean" style="width: 1500px;"/> 

Looking at the both figures above, using 1.5xIQR for trips' distance and duration will introduce a large amount of bais as the distance of the taxi trips will be range between to 300m - 15km and durations between ~2mins to ~40mins. Whereas by using 3*IQR, the distance of the taxi trips will be range between 0.08 - 60.8km and durations between ~45s to ~2h. Taking into considering the area of end points is ~383km sq, 3xIQR would be a better choice to remove outliers.

At this point, the training dataset is fairly clean. Now, more insights from the taxi trips can be extracted. Using the date and time data extracted, the distribution of the trips can be observed. 

<img src="../assets/images/trips_day.png" alt="trips_day" style="width: 1500px;"/> 

Most taxi rides appear to happen between the day, between 31 - 84 quarter hours, which is ~0800 - 2100 hours. The quarter hour at which most trips are conducted is 35, between 0845 to 0900 might be due to people being late and getting a taxi to rush to work/school. Least trips are conducted at quarter hour 10, between 0230 to 0245.

<img src="../assets/images/Dis_Dur_Clean.png" alt="Dis_Dur_Clean" style="width: 1500px;"/> 

Most taxi trips are on Fridays. It appears that most likely people are out for entertainment to kickstart the weekend and use taxis to commute. 












[Check it out](http://sergiokopplin.github.io/indigo/) here.
If you need some help, just [tell me](http://github.com/sergiokopplin/indigo/issues).
