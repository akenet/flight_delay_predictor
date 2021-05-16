# flight_delay_predictor

[Link to Data](<https://drive.google.com/drive/folders/1RYEbID7LjLgQw-ikG2i801AvsaWu56qq?usp=sharing>)

•Airline Delay and Cancellation Data, 2009 - 2018 | Kaggle <https://www.kaggle.com/yuanyuwendymu/airline-delay-and-cancellation-data-2009-2018>

•2015 Flight Delays and Cancellations | Kaggle <https://www.kaggle.com/usdot/flight-delays>

•Find Open Datasets and Machine Learning Projects | Kaggle <https://www.kaggle.com/datasets?topic=trendingDataset>

•Predicting flight delays [Tutorial] | Kaggle <https://www.kaggle.com/fabiendaniel/predicting-flight-delays-tutorial/data?select=flights.csv>

•Using Machine Learning to Predict Flight Delays | by Javier Herbas | Analytics Vidhya | Medium <https://medium.com/analytics-vidhya/using-machine-learning-to-predict-flight-delays-e8a50b0bb64c>





-------



#Flight Delay Estimation Project

##Authors: 

Adam Kenet: Graphic UI Design & Implementation

(Karl) Sangwon Lee: Model Design & Evaluation

##Link for the App: https://akenet.shinyapps.io/Flight_Delay_Predictor/

Description of the App

##Background

Flight delay is a valuable topic of interest due to its associated financial losses in the aviation industry. 2018 US data from Bureau of Transportation Statistics (BTS) indicates that more than 20% of US flights were delayed in 2018, resulting in severe economic impact around 41 billion USD. Such delays not only disrupt flight industries, but also many passengers, as the increase of travel time and schedule changes create financial inconveniences and additional stresses for many. Unexpected flight delays as a result further harms an airline’s reputation. 

Although the causes behind these delays can widely vary - air traffic congestion, weather conditions, mechanical issues, troubles with boarding passengers, etc. - we were motivated to see if we could adopt a machine learning based approach to extract a pattern from an existing dataset on flight delays and create a model that could predict and estimate flight delays that passengers could utilize.

Previous projects related to ML-based prediction of flight delays have been limited to the scope of classification problems - whether the flight is delayed or not. In our project, we focus on the numerical estimation of actual delay in minutes and hours for a specific flight, given a set of user inputs on flight date, airline, departing airport, and time of the flight. In doing so, we will utilize on-the-spot training of generalized linear models to estimate delay time based on the models that have been trained using curated data that have been filtered according to the user’s input. 




## Data:
All data used in this model is publicly available on Kaggle: https://www.kaggle.com/yuanyuwendymu/airline-delay-and-cancellation-data-2009-2018. This dataset contains information on over 60 million US domestic flights from 2009 to 2018. The important features for the model are airline, departure/arrival airports, day of departure, scheduled time of departure, and delay. The delay was determined by subtracting the time of the scheduled departure from the time of the actual departure. Note that flights that left early have a negative delay time. For the purposes of our project, only flights with a delay greater than 10 minutes are considered to be delayed.


## Processing:
The original dataset we are using had over 60 million flights and 28 features. This large amount of data (7+ GB) initially made it difficult to access and load into our model. Therefore, we first pre-processed the data to remove any irrelevant features which reduced the overall size of the data. Additionally, all abbreviations for airlines and airports were changed to include the full name of the airline/airport. The dataset containing information on each airport in the main dataset can be found here: https://github.com/akenet/flight_delay_predictor/blob/main/airports_filtered.csv. Finally, in order to upload the data onto Github where each file size needs to be less than 100 MB, the dataset was broken into 61 files each with approximately one million flights. These data can be found here: https://github.com/akenet/flight_delay_predictor/tree/main/Data and the Python code for preprocessing can be found here: https://github.com/akenet/flight_delay_predictor/blob/main/Flight%20Delay%20Predictor%20Data%20Munging.ipynb


## Model Design:

We used a generalized linear model to fit a linear regression into the delay time data. Based on the user inputs, 5 predictors were relevant:

Flight date
Flight time of the day
Airline name
Departing airport
Arrival Airport

Then, we attempted several designs regarding the linear model based on these 5 predictors. However, as arrival airport was not relevant to the departing delay, the total predictors we had to deal with were 4 predictors.

Flight date and flight time of the day were numerical data, whereas the other three were categorical variables. Airline names had 23 categories, and airports had 357 categories. Our original attempt to create an interacting linear model using these categorical variables as an individual factor of one-hot vectors did not succeed due to an out of memory issue. 

An attempt at reducing the levels of these categorical factors, such as reducing the number of departing airports to top 50 frequent airports, actually ended up harming the model’s performance and usability. 

In light of this problem, we decided to perform an ‘on-the-spot’ training of our linear model, in which we decided to train our model each time the user submitted the job and train it only on the relevant data that has been filtered using the user's input. For example, the user’s choice of American Airline and departing airport as BWI would filter the dataset for the specific airline and the departing airport.

Then, the linear regression problem becomes much simpler, where we fit delay time in minutes against two numerical predictors: flight date and time of the flight. Flight date was converted to an integer from 1 to 365, and time of the flight was defined as from 1 to 2400.

Delay Time = b_0 + b_1 * flight_date + b_2 * time_of_flight

Model’s performance was qualitatively evaluated based on our comparison against previous historical data, and found to be reasonably sufficient for our project’s purposes. However, the detailed performance metric was difficult to acquire and a more specific measure could be implemented for future project ideas. Furthermore, the limitation of data storage for github student accounts reduced the amount of data we could access during training, which also impacted our performance. Overall, our approach to this estimation problem serves as an original prototype for a data-based numerical prediction of a real-world problem like flight delay prediction.

## Graphic UI Design

Left Control Panel:

Users input the airline of their flight
Users input the departing airport name. Users can easily type the name of the airport instead of trying to find one among 357 airports.
Likewise, users can select the arrival airport.
Users input their departure date by clicking the date on a visual calendar-style input.

Users type the time of the day in the specified format.
Pressing the ‘Submit’ button will then perform training and prediction, as well as display of any available historical data.


Right Display Panel:



First plot on the right is the text display of how much the user’s flight will be delayed in minutes.



Second plot on the right is a text display of how many specific flights similar to the user’s choice were delayed or not. 



Third plot on the right depicts historical flight data for the specific airline and different departure dates.



Last plot on the right panel provides a visual aid to the geographic connection between two US airports. Geodesic line is also displayed to depict the flight’s physical route.
