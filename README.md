# Flight Delay Estimation Project

## Authors

Adam Kenet: Graphic UI Design & Implementation

(Karl) Sangwon Lee: Model Design & Evaluation

## Link for the App: https://akenet.shinyapps.io/Flight_Delay_Predictor/ (demo version)

## Link for video: 

## System and Library Requirements

### System:
CPU: i9-10885H<br />
RAM: 32 GB


### Library:
library(flexdashboard)<br />
library(tidyverse)<br />
library(dplyr)<br />
library(plotly)<br />
library(shiny)<br />
library(lubridate)<br />
library(googleVis)<br />
library(leaflet)<br />
library(geosphere)


# Description of the App

## Background

Flight delay is a valuable topic of interest due to its associated financial losses in the aviation industry. US data from the Bureau of Transportation Statistics (BTS) indicates that more than 20% of US flights were delayed in 2018, resulting in severe economic impact around $41 billion. Such delays not only disrupt flight industries, but also many passengers, as the increase of travel time and schedule changes create financial inconveniences and additional stresses for many. Unexpected flight delays as a result further harms an airline’s reputation. 

Although the causes behind these delays can widely vary - air traffic congestion, weather conditions, mechanical issues, troubles with boarding passengers, etc. - we were motivated to see if we could adopt a machine learning based approach to extract a pattern from an existing dataset on flight delays and create a model that could predict and estimate flight delays that passengers could utilize.

Previous projects related to ML-based prediction of flight delays have been limited to the scope of classification problems - whether the flight is delayed or not. In our project, we focus on the numerical estimation of actual delay in minutes and hours for a specific flight, given a set of user inputs on flight date, airline, departing airport, and time of the flight. In doing so, we will utilize on-the-spot training of generalized linear models to estimate delay time based on the models that have been trained using curated data that have been filtered according to the user’s input. 



## Data
All data used in this model is publicly available on Kaggle: https://www.kaggle.com/yuanyuwendymu/airline-delay-and-cancellation-data-2009-2018. This dataset contains information on over 60 million US domestic flights from 2009 to 2018. The important features for the model are airline, departure/arrival airports, day of departure, scheduled time of departure, and delay. The delay was determined by subtracting the time of the scheduled departure from the time of the actual departure. Note that flights that left early have a negative delay time. For the purposes of our project, only flights with a delay greater than 10 minutes are considered to be delayed.


## Processing
The original dataset we are using had over 60 million flights and 28 features. This large amount of data (7+ GB) initially made it difficult to access and load into our model. Therefore, we first pre-processed the data to remove any irrelevant features which reduced the overall size of the data. Additionally, all abbreviations for airlines and airports were changed to include the full name of the airline/airport. The dataset containing information on each airport in the main dataset can be found here: https://github.com/akenet/flight_delay_predictor/blob/main/airports_filtered.csv. Finally, in order to upload the data onto Github where each file size needs to be less than 100 MB, the dataset was broken into 61 files each with approximately one million flights. These data can be found here: https://github.com/akenet/flight_delay_predictor/tree/main/Data and the Python code for preprocessing can be found here: https://github.com/akenet/flight_delay_predictor/blob/main/Flight%20Delay%20Predictor%20Data%20Munging.ipynb


## Model Design

We used a generalized linear model to fit a linear regression into the delay time data. Based on the user inputs, 5 predictors were relevant:

- Flight date
- Flight time of the day
- Airline name
- Departing airport
- Arrival airport

Then, we attempted several designs regarding the linear model based on these 5 predictors. However, as arrival airport was not relevant to the departing delay, the total predictors we used were the other 4 predictors:

- Flight date
- Flight time of the day
- Airline name
- Departing airport

Flight date and flight time of the day were numerical data, whereas the other three were categorical variables. Airline names had 23 categories, and airports had 357 categories. Our original attempt to create an interacting linear model using these categorical variables as an individual factor of one-hot vectors did not succeed due to an out of memory issue. 

An attempt at reducing the levels of these categorical factors, such as reducing the number of departing airports to top 50 frequent airports, actually ended up harming the model’s performance and usability. 

In light of this problem, we decided to perform an ‘on-the-spot’ training of our linear model, in which we train our model each time the user submits their flight's information, and train it only on the relevant data that has been filtered using the user's input. For example, the user’s choice of Southwest and departing airport as BWI would filter the dataset to show only Southwest flights from BWI.

Then, the linear regression problem becomes much simpler, where we fit delay time in minutes against two numerical predictors: flight date and time of the flight. Flight date was converted to an integer from 1 to 365, and time of the flight was defined as from 1 to 2400.

<img src="https://render.githubusercontent.com/render/math?math=Delay \: Time = b_{0} %2B (b_{1} * Flight \: Date) %2B (b_{2} * Time \: of \: Flight)">

The model’s performance was qualitatively evaluated based on our comparison against previous historical data, and found to be reasonably sufficient for our project’s purposes. However, the detailed performance metric was difficult to acquire and a more specific measure could be implemented for future project ideas. Furthermore, the limitation of data storage for Shiny student accounts reduced the amount of data we could access during training, which also impacted our performance. Overall, our approach to this estimation problem serves as an original prototype for a data-based numerical prediction of a real-world problem like flight delay prediction.

## Graphic UI Design

### Left Control Panel:

![Left Control Panel 1](https://github.com/akenet/flight_delay_predictor/blob/main/Figures/Fig1.png)

- The user inputs the airline of their flight
- The user inputs the departing airport name. The user can easily type the name of the airport instead of trying to find one among 357 airports.
- Likewise, the user can select the arrival airport.
- The user inputs their departure date by clicking the date on a visual calendar-style input.

- The user types the time of the day in the specified format (24 hour time).
- Pressing the ‘Submit’ button will then perform training and prediction, as well as display any available historical data.


### Right Display Panel:


![Right Display Panel 1](https://github.com/akenet/flight_delay_predictor/blob/main/Figures/Fig2.png)


First plot on the right is the text display of how much the user’s flight will be delayed in minutes.

![Right Display Panel 2](https://github.com/akenet/flight_delay_predictor/blob/main/Figures/Fig3.png)


Second plot on the right is a text display of how many specific flights similar to the user’s choice were delayed (by more than 10 minutes) or not. 

![Right Display Panel 3](https://github.com/akenet/flight_delay_predictor/blob/main/Figures/Fig4.png)


Third plot on the right depicts historical flight data for the specific airline and different departure dates.

![Right Display Panel 4](https://github.com/akenet/flight_delay_predictor/blob/main/Figures/Fig5.png)

Last plot on the right panel provides a visual aid to the geographic connection between two US airports. Geodesic line is also displayed to depict the flight’s physical route.


