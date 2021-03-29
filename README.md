# IDEAC

The IDEAC project’s goal is to provide more reliable temperature and humidity forecasts to be used by farmers to consistently predict and respond to damaging weather events. Individual forecasters often provide inaccurate predictions, but by combining forecasts from multiple providers, IDEAC has been able to decrease the error in temperature and humidity forecasting. This versino of IDEAC has been migrated to PySpark to better leverage cluster computing and decrease execution time while still maintaining maximum accuracy of predictions. 


The model is a four-layer ensemble learning model using a combination of random forest regression, gradient boosted regression and multilayer perceptron regression. Weather data is pulled from the APIs of weather forecasters API Agro, Weatherbit, and DarkSky. This data is hourly timestamped readouts from various sensors. The temperature, humidity and wind data is cleaned and concatenated with previously downloaded data. This dataframe is then fed into the multilayer ensemble learning model. 


![image](https://user-images.githubusercontent.com/49824508/112873549-a1cc6600-9076-11eb-883a-14486fc65eb8.png)


When the dataframe reaches the first layer, the data is partitioned into training (75%) and testing (25%). Each regression model is trained to predict temperature using the set of features from the training set and its accuracy is evaluated using the testing set. Root mean squared error is used to for mean error in degrees Celsius between predicted and actual temperatures. The predicted values from each model are then concatenated and used as input for the next layer. Successive layers process only 25% of the previous layer’s data. This funnel structure means that by the final layer, the test dataset is for only 6 timeslots and temperature is therefore only predicted for 6 hours. 



![image](https://user-images.githubusercontent.com/49824508/112873817-fc65c200-9076-11eb-9a87-6c3a640380fe.png)
