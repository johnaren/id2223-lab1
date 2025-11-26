Air Quality Prediction – Serverless ML System
ID2223 – Lab 1 
This project implements a serverless Machine Learning system that predicts PM2.5 air quality for the next 7 days.
The system uses Hopsworks Feature Store, GitHub Actions, and an XGBoost model trained on weather and air-quality data.

Project Overview
The goal of this lab is to build an end-to-end ML pipeline:
•	A Backfill Pipeline for historical data
•	A Daily Feature Pipeline that updates data automatically
•	A Training Pipeline that builds a regression model
•	A Batch Inference Pipeline producing predictions
•	A Dashboard visualizing future PM2.5 levels
•	A Monitoring (Hindcast) graph
•	A Model upgrade using lag features (for Grade C)
Everything runs serverlessly, without managing servers manually.

 Data Sources
We use two open-source APIs:
Air Quality Data (PM2.5)
Source: AQICN (https://aqicn.org)
Selected sensor: Stockholm – Sollentuna Ekmans Väg 11
Weather Data
Source: Open-Meteo (https://open-meteo.com)
Includes wind, temperature, humidity, etc.
Both are stored in Hopsworks as Feature Groups:
•	air_quality
•	weather

 Task 1 – Backfill Feature Pipeline
We downloaded ~1 year of historical weather data and a CSV of historical PM2.5.
We cleaned the data and registered two Feature Groups in Hopsworks.
The model needs historical data to learn long-term trends.

Task 2 – Daily Feature Pipeline
A GitHub Actions workflow runs every day and:
1.	Downloads yesterday’s weather
2.	Downloads yesterday’s air quality
3.	Gets 7–10 day weather forecast
4.	Updates both Feature Groups in Hopsworks
Fresh data is required for correct predictions.

 Task 3 – Training Pipeline
We created a Feature View joining weather + PM2.5.
We trained an XGBoost Regressor using train/test split.
The trained model was saved in the Hopsworks Model Registry.
Feature Store ensures consistent data for training and inference.

Task 4 – Batch Inference Pipeline
This pipeline:
•	Downloads the latest model from Hopsworks
•	Reads future feature data
•	Predicts PM2.5 for the next 7 days
•	Generates a PNG Dashboard using matplotlib
The dashboard communicates the model’s value visually.

Task 5 – Monitoring (Hindcast)
We compared Predicted vs Actual PM2.5 values for previous days.
A hindcast plot shows model accuracy over time.

Why predictions diverge at the end:
•	Weather forecasts become less accurate for later days
•	We don’t have real lag values for future days
•	PM2.5 is highly noisy → uncertainty increases

Task 6 – Lag Features (Grade C)
To improve model accuracy, we added:
•	pm2_5_lag1
•	pm2_5_lag2
•	pm2_5_lag3
These represent PM2.5 from 1, 2, and 3 days ago.
Why it helps:
Past pollution levels strongly influence future values.
Weather alone cannot explain PM2.5.
Result:
•	Model Version 2 improved significantly
•	Higher R²
•	Lower MSE
•	Predictions follow real trends more closely

Project Structure
/notebooks
   backfill_feature_pipeline.ipynb
   daily_feature_pipeline.ipynb
   training_pipeline.ipynb
   batch_inference_pipeline.ipynb

/src
   feature_pipeline.py
   training.py
   inference.py

/dashboard
   pm25_predictions.png
   hindcast.png

Conclusion
This lab implements a full end-to-end serverless ML system with:
•	automated data ingestion
•	feature engineering
•	model training and versioning
•	daily batch inference
•	a working dashboard
•	model monitoring
•	performance improvement via lag features

