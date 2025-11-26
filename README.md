# ID2223 Scalable Machine Learning and Deep Learnin, Lab 1

Johannes Arenander and Elmira Mirzaee

Originally forked from [https://github.com/featurestorebook/mlfs-book]

## Intro

This is the first lab assignment in the course ID2223 Scalable Machine Learning and Deep Learning, given by KTH during the fall of 2025. The goal of this assignment was to create a feature pipeline and train a regression model to predict PM25 air quality measurements from other weatherly features including temperature, percipitation, wind speed and direction.

## Method

We choose a air quality sensor located on the street Ekmans Väg, Sollentuna in the suburbs of Stockholm. We start by creating two feature groups on the feature store Hopsworks [https://hopsworks.ai/]: one for historical sensor readings [https://aqicn.org/] and the other for weatherly features [https://open-meteo.com/] fetched from their respective API's. We then create a feature view with weatherly data as features and air quality readings as targets in order to train a simple regression model which we upload to the feature store. Finally, we schedule a daily feature and inference pipeline with GitHub actions, that updates the store with new daily features and makes a forecast by predicting air quality the next few days.

## Results

The dashboard for the forecast and hindcast produced by the model can be seen the GitHub pages for this repository [https://johnaren.github.io/id2223-lab1/air-quality/]. We also trained another model on lagged features for 1-3 days, the regression for the models can be found here:

 - Original model: [https://github.com/johnaren/id2223-lab1/blob/main/notebooks/airquality/air_quality_model/images/pm25_hindcast.png]
 - Lagged features model: [https://github.com/johnaren/id2223-lab1/blob/aq-lag/notebooks/airquality/air_quality_model/images/new_pm25_hindcast.png]

## Directory

 - Notebooks: [https://github.com/johnaren/id2223-lab1/tree/aq-lag/notebooks/airquality]
 - GitHub actions scheduler: [https://github.com/johnaren/id2223-lab1/blob/main/.github/workflows/air-quality-daily.yml]
