# Scalable Machine Learning and Deep Learning Lab 1

# Air Quality Prediction Service - Lab 1
By Serkan Anar & Roy Liu

## Goals
 - Create an AI system that predicts air quality given weather conditions for a city in Sweden. 
 - Get introduced to Hopsworks. 
 - Use Hopsworks as a feature and model store for the pipeline.
 - Use GitHub Pages to deploy a dashboard that displays daily forecasts and hindcasts. 

## Description

### Data Collection
Our city of choice is Västerås, and we are using one of two sensors, namely the one for Rotorvägen. The other one (Stora Gatan 78) did not have PM2.5 data, which is why we decided to choose that particular sensor. 

### Feature Backfill
In the notebook `1_air_quality_feature_backfill`, we update the information for the sensor and URL, and also update the path to the CSV file that contains historical data. The data is then cleaned before we create feature groups and insert our air quality and weather forecast dataframes into them. 

### Feature Pipeline
In the notebook `2_air_quality_feature_pipeline`, we get today’s air quality data and weather forecast data for the coming days. Then, we upload our new data to the feature store, so we have up-to-date data. 

### Training Pipeline
In the notebook `3_air_quality_training_pipeline`, we create a feature view from our two feature groups (air quality and weather data), in order to create our dataset for training and validation. An XGBoost model is trained, and the predictions are plotted against the true labels. Before saving the model in Hopsworks, the importance of each feature in the weather data is plotted. 

### Batch Inference
In the notebook `4_air_quality_batch_inference`, the model from the previous step and the batch inference data are downloaded. The model is used to make predictions given the weather data for the coming days, and the predictions are saved to a feature group. Then, the predictions are plotted. Finally, a hindcast is plotted. 

### Deployment
The pipeline is finally deployed through GitHub’s Actions and Pages features. Using the `.github/workflows/air-quality-daily.yml` file, notebooks 2 and 4 are run daily with GitHub Actions. This creates an automated pipeline where data is fetched every day from the sensor in Rotorvägen, and the GitHub Pages dashboard is then updated with our inferences.

## Obstacles
 - At the beginning, the dashboard only produced 6 predictions. In order to produce 7-10 predictions each day, we modify the API call to fetch 10 forecasts from Open-Meteo’s weather forecasts in the `util.get_hourly_weather_forecast` function.
 - The hindcast was problematic, and we had to switch the ordering of the code in cell 9 to sort by date before adding the `days_before_forecast_day` column. 
 - The GitHub Action was not being run automatically when we uncommented lines 4 and 5, and we found out that this is because `schedule:` should not be indented.

## Results
The predictions and hindcasts of the air quality conditions can be observed at our [GitHub Pages Dashboard](https://ruireng.github.io/mlfs-book/air-quality/), which is updated daily.
