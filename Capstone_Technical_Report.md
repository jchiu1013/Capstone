# Predicting trends in home sale prices
## DSI Capstone: James Chiu 

### Goals: 
Whether you are a real estate investor, homeowner, renter or government, housing is a part of the economy that affects everybody. This project will focus on some of the foundational questions that come up when going about researching housing. 
- How do we make informed decisions about a basic need? 
  * In order to make an informed decision, we look at past trends and the current condition of the market. 
- How can we predict trends in the housing market? 
  * The way to predict trends in the housing market requires a look at pricing data. This is because prices have imbedded information about the demand and temperature of a market. 
- Can we use time series data to predict the price of a house? 
  * After obtaining pricing data for the sales price of a house market, the next action steps are to make predictions based on inference from the information.  

The primary goal of this capstone project is to predict future home sales price 1-6 months ahead. This project will also look at other factors that may have an effect. Answering the quesion, does adding features relevant to the economy as a whole affect the accuracy of the model? In order to make informed decisions the following data that affect markets as a whole will be analyzed: CPI, Fed Funds Rate and Unemployment Rate.

### Exploratory Data Analysis: 
The time series data was collected from 2 primary sources. The first is Zillow, a website for real estate information and the St. Louis Federal Reserve, a website that provides economic data. A point of interest to note is the downward trend in housing price from 2008 until mid 2011 - 2012. This recession is likely due to the financial crisis in 2008. Further observation into the federal funds rate reveals monetary policy that was taken, moving rates close to zero. The lowering of the fed funds rate was taken to stimulate the economy by decreasing the cost of borrowing. The subsequent increase in inflation beginning in 2009, paralleled the decrease in fed funds rate. Also, the decrease in unemployment rate has an observed lag effect decreasing from its peak in 2010. 

Zillow[1]: 
The Zillow data set will be focused primarily on Portland median sale price data from March 2008 to December 2017. There are 118 monthly observations available for analysis in that date range. The choice for portland was chosen primarily due to the continuous stream of monthly data available. I also chose San Francisco as a comparison point to show the capacity to extend the model to other cities. The San Francisco data was missing data for December 2017. 

![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Portland%20Median%20Sales%20Price.jpg "Portland Median Sales Price")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/SF%20Median%20Sales%20Price.jpg "San Francisco Median Sales Price")

St. Louis Federal Reserve[2]:
The data from the St. Louis Federal Reserve website uses monthly data for consumer pricing index(CPI), unemployment rate, and federal funds rate from March 2008 to December 2017. 

For reference, the Consumer Price Index for All Urban Consumers: All Items (CPIAUCSL) is a measure of the average monthly change in the price for goods and services paid by urban consumers between any two time periods[3]. The CPI is indexed from 1982-1984=100, Seasonally Adjusted. 

![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/CPI%20Rate.jpg "CPI Rate")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Unemployment%20Rate.jpg "Unemployment Rate")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Federal%20Reserve%20Funds%20Rate.jpg "Fed Funds Rate")

### Feature Engineering:
The primary task in feature engineering for time series data were taken in the following order: 
1. Convert the dates to appropriate date time format
2. Create a function to shift the dataset by the number of months that will be used as a predictor. 
  - For example using 6 months of data to predict 1 month ahead. 
3. Create a function to shift the predictor by the number of months we wish to predict forward. 
  - For example 6 months of portland monthly price data will be used to predict 2 months ahead. 

### Modeling Table of Contents: 
The median house price predictive model was developed by going through the following procedures: 
1. Compare models Linear Regression, Neural Network and Long Short Term Memory Recurrent Neural Network
2. Use San Francisco median house price to check transferability of model 
3. Increase inputs using 3, 6, 12, 18 months of data for prediction (Does more data improve the MSE?)
4. Check the limit of predictive capacity 1, 3, 6, 12 month look ahead (How far can the model accurately predict into the future?)
5. Add feature inputs from interest rates, unemployment, CPI to prediction sale price
6. Create model that includes inputs from previous months and added features to predict sale price (Is this the optimal model?)

### Model: 
1. Compare models Linear Regression, Neural Network and Long Short Term Memory Recurrent Neural Network
The baseline model used 6 months of Portland median housing price data to predict 1 month ahead. The following models were used to determine the optimal choice for moving forward: 
- Linear Regression
- Neural Network
- Long Short Term Memory(LSTM) - Recurrent Neural Network(RNN)

After optimizing the models the Mean Squared Error was used as a success metric to determine the best choice moving forward. Mean squared error is the sum of the squared difference between the actual portland house sale price minus the predicted Portland house sale price divided by the number of predictions. The square root of the mean squared error was taken to find the dollar difference between the predicted and real portland median house sale price. 

MSE = $\frac{1}{n}\sum_{t=1}^{n} e_t^2$
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/MSE.png | height=24 width=48})

Model Results: 
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Model%20Comparison%20MSE.png "Model Comparison")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Linear%20Regression%206%20month.jpg "Portland Linear Regression 6 month")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Neural%20Network%206%20month.jpg "Portland Neural Network 6 month")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/LSTM%206%20month.jpg "Portland LSTM 6 month")

Long Short Term Memory(LSTM) had the lowest error for the baseline 1 month ahead prediction using previous 6 months of Portland housing price data. LSTM is a good model for time series data because it takes into account sequence dependence from the input variables. Also, observing the plot of the 6 month LSTM, the red prediction line smooths out the volatility and shows a good prediction of the real Portland median house sale price. 

2. Use San Francisco median house price to check transferability of model 
San Francisco 6 month input data was used as a feature to predict the San Francisco median house price 1 month ahead. Below reveal the result of the prediction. The red predicted line plotted with the black real San Francisco median house price reveals an accurate model with an error of $3464.32.
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/SF_LSTM_6_month.jpeg "SF LSTM 6 month")

3. Increase inputs using 3, 6, 12, 18 months of data for prediction (Does more data improve the MSE?)
The next iteration of the LSTM model used increments of 3, 6, 12 and 18 months of Portland median house sale price data to predict one month ahead. Observing the chart of the MSE. After observing the MSE using the incremental monthly input data, an additional step was taken using the autocorrelation plot to determine the relationship of past data had on the present price. 18 months of previous data was chosen using a threshold of correlation above 0.5. The prediction using 18 months of Portland median house sale price to predict one month ahead can be seen in the third graph. 

![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/MSE%20-%20Portland%20LSTM%201%203%206%2012%20month%20ahead%20prediction.jpg "MSE - Portland LSTM 3 6 12 18 month 1 ahead prediction")
### Autocorrelation of Portland Median Sales Price
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Autocorrelation%20Portland.jpeg "Autocorrelation")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Portland%2018%20month%201%20ahead%20LSTM.jpeg "Portland LSTM 18 month 1 ahead")

4. Check the limit of predictive capacity 1, 3, 6, 12 month look ahead (How far can the model accurately predict into the future?)



[1]: https://www.zillow.com/research/data/
[2]: https://fred.stlouisfed.org
[3]: https://fred.stlouisfed.org/series/CPIAUCSL