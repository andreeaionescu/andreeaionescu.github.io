---
layout: post
title: Hazard Prediction - Full Stack Solution Using React And Data Science
date: 2020-10-04 00:00:00 +0300
description: Exploring the project built as part of NASA Space App Challenge which helps with predicting hazards. # Add post description (optional)
img: nasa-space-apps.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [NASA, Software, React, Redux, Python, NumPy, Pandas, Flask, MachineLearning] # add tag
---

[NASA Space Apps London Hackathon][nasa-space-apps] took place virtually on the 2 - 4th of October weekend. Over 22 teams participated and this year's theme was "Take Action" - a reminder that despite of the pandemic, people can still make a difference even from the comfort and safety of their homes. The space agency partners who provided the datasets as well as volunteers as subject-matters expert were: CSA, CNES, JAXA, ESA and, of course, NASA. 

###  Automated Detection of Hazards - Our Team's Challenge
Countless phenomena such as floods, fires, and algae blooms routinely impact ecosystems, economies, and human safety. Our challenge was to use satellite data to create a machine learning model that detects a specific phenomenon. On top of this, we built an interface that, not only displays the detected phenomenon, but also layers it alongside ancillary data to help researchers and decision-makers better understand its impacts and scope.

One of the reasons why we chose this theme is that natural disasters, are becoming more frequent due to climate change and hence, a greater number of people's lives are being disrupted every year. For example, the [Australian Wildfires][australian-wildfires] burned almost 12 million acres, destroying almost 2000 homes and killing 480 million animals in 2020. Therefore, predicting with an accuracy of at least 80% would help with detecting the hazards from an early stage. 

### How We Addressed This Challenge

In order to automatically detect hazard, we built a Machine Learning model using open source NASA satellite images dataset that predicts Transversal Cirrus Bands on an interactive world-map. The application consists of 3 main services: ui-service, hazard-prediction-service and hazard-analytics-service.

![NASA Hazard Detection Webpage]({{site.baseurl}}/assets/img/nasa-solution.png)

### Interactive UI showing the map

Interactive Web interface build with React and Redux, HTML5 and CSS3. It consists of 3 main sections (Menu section, Map sections, Analytics Section). Interactive map using Google Maps API and the satellite view. See picture above.

### Hazard Prediction Service

The role of this service is to call the MODIS API to retrieve the satellite data in real time from the location selected by the user. After receiving the images, it runs the Machine Learning model against the data and calculates the probability that the location selected is an Transverse Cirrus Band which what hurricane or thunderstorms look like from the above. 

Transverse Cirrus Bands are bands of clouds oriented perpendicular to the atmospheric flow in which they are embedded. TCBs are often an indicator of strong turbulence and often associated with severe weather such as hurricanes and thunderstorms or atmospheric jets. The sateliatte images look like the following:

![NASA MODIS API Images]({{site.baseurl}}/assets/img/nasa-modis.png)

The ML model was built using JupyterNotebooks and uses Random Forests to make the predictions and categorise satellite images as isHurricane or isNotHurricane.

### Hazard Analytics Service

Exposes different kind of statistics and events about hazards around the globe. This service reuses existing Kaggle open datasets for wildfires in Brazil, Australia, California and mines insightful analytics which are further displayed in the web application. The analytics consist of correlation matrixes, barcharts as well as graphs to better showcase the data relations in order to help people understanding more about the impacts. Technology stack: Python, Pandas, Matplot, Numpy, Flask.

### Architecture

![NASA Full Solution Architecture]({{site.baseurl}}/assets/img/nasa-architecture.png)


### Github Repos
[Skyfolks Interactive UI][front-end]
[Skyfolks Hazard Prediction Service][skyfolks-hazard-prediction-service]
[Skyfolks Hazard Analytics Service][analytics-service]

[nasa-space-apps]: https://2020.spaceappschallenge.org/locations/london/
[australian-wildfires]: https://www.weforum.org/agenda/2020/01/australia-bushfires-size-impact-wildlife-emissions/
[skyfolks-hazard-prediction-service]: https://github.com/andreeaionescu/skyfolks-hazard-prediction-service
[front-end]: https://github.com/andreeaionescu/skyfolks-frontend
[analytics-service]: https://github.com/airineivlad/skyfolks-backend