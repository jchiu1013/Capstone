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

Long Short Term Memory(LSTM) had the lowest error for the baseline 1 month ahead prediction using previous 6 months of Portland housing price data. LSTM is a good model for time series data because it takes into account sequence dependence from the input variables. Also, observing the plot of the 6 month LSTM, the red prediction line smooths out the volatility and shows a good prediction of the real Portland median house sale price. 

Model Results: 
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Model%20Comparison%20MSE.png "Model Comparison")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Linear%20Regression%206%20month.jpg "Portland Linear Regression 6 month")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Neural%20Network%206%20month.jpg "Portland Neural Network 6 month")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/LSTM%206%20month.jpg "Portland LSTM 6 month")

2. Use San Francisco median house price to check transferability of model 
San Francisco 6 month input data was used as a feature to predict the San Francisco median house price 1 month ahead. Below reveal the result of the prediction. The red predicted line plotted with the black real San Francisco median house price reveals an accurate model with an error of $3464.32.

Model Results: 
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/SF_LSTM_6_month.jpeg "SF LSTM 6 month")

3. Increase inputs using 3, 6, 12, 18 months of data for prediction (Does more data improve the MSE?)
The next iteration of the LSTM model used increments of 3, 6, 12 and 18 months of Portland median house sale price data to predict one month ahead. Observing the chart of the MSE. After observing the MSE using the incremental monthly input data, an additional step was taken using the autocorrelation plot to determine the relationship of past data had on the present price. 18 months of previous data was chosen using a threshold of correlation above 0.5. The prediction using 18 months of Portland median house sale price to predict one month ahead can be seen in the third graph. 

Model Results: 
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/MSE%20-%20Portland%20LSTM%201%203%206%2012%20month%20ahead%20prediction.jpg "MSE - Portland LSTM 3 6 12 18 month 1 ahead prediction")
- ### Autocorrelation of Portland Median Sales Price
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Autocorrelation%20Portland.jpeg "Autocorrelation")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Portland%2018%20month%201%20ahead%20LSTM.jpeg "Portland LSTM 18 month 1 ahead")

4. Check the limit of predictive capacity 1, 3, 6, 12 month look ahead (How far can the model accurately predict into the future?)
The next step after optimizing the input data was to observe the limit of the model to predict into the future. The LSTM model used 18 months of Portland median house sale price data to predict 1, 3, 6 and 12 months ahead. As you can see from the graph of the MSE below, the error widens as the model is used to predict further into the future. This shows that there is a limit to the accuracy of prediction with a longer term forecast. It is also important to note that time series analysis uses past prices to forecast the future, therefore it is important to take into account analysis of other fundamental metrics when using this technique. 

Model Results: 
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/MSE%20-%20Portland%20LSTM%2018%20month%201%2C3%2C6%2C12%20ahead%20prediction.jpg "MSE - Portland LSTM 18 month 1,3,6,12 ahead prediction")

5. Add feature inputs from interest rates, unemployment, CPI to prediction sale price
In an attempt to build in economic factors that also effect the median sale price of houses additional input features such as federal funds rate, unemployment rate and consumer price index were used. Below shows how well these features are able to predict the median sale price of a house in Portland. 

Model Results: 
- The 18 months of CPI data was used in the LSTM model to predict Portland median house sale price 1 month ahead. Observing the data visually, it can be seen that CPI follows a similar trend to that of the Portland median house sale price. After running the LSTM model the results can be seen in the second graph below. The red line is the Portland median house sale price prediction that the CPI data produced while the black is the real price. This model is able to track the trend fairly well but is not as accurate when it comes to magnitude with an error of $2639.88.

![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/CPI%20Rate.jpg "CPI Rate")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/CPI-Portland%2018%20Months%201%20Month%20Prediction.png "CPI 18 month Portland 1 month prediction")

- In the following 2 graphs, 18 months of unemployment rate data was used in the LSTM model to predict the Portland median house sale price 1 month ahead. After observing the trend for unemployment rate in the first graph, an inverse relationship of unemployment rate to Portland median sales price is slightly visible. From, here the second plot reveals the predicted Portland median house sale price against the real price. This model is also able to do predict fairly well with an error of $2627.04. 
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Unemployment%20Rate.jpg "Unemployment Rate")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/UR-Portland%2018%20Months%201%20Month%20Prediction.png "UR 18 month Portland 1 month prediction")

- The following 2 graphs show the data from the federal funds rate and the LSTM model using 18 months of federal funds rate data to predict 1 month ahead for Portland median house sale price. The federal funds rate was reduced to zero late 2008 and remained near zero through late 2015. Since the fed funds rate represents the interbank cost of borrowing, intuitively it would seem to be a good indicator for house prices. This is because mortgages would be directly effected by shifts in this rate. But, as observed in the regression the predicted price of the Portland median house sale price does not follow the real value very well, having an error of $5378.96. As a result, this was not chosen as an input for the next iteration of the LSTM model. 
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Federal%20Reserve%20Funds%20Rate.jpg "Fed Funds Rate")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/FFR-Portland%2018%20Months%201%20Month%20Prediction.png "FFR 18 month Portland 1 month prediction")

6. Create model that includes inputs from previous months and added features to predict sale price (Is this the optimal model?)

Model Results: 
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Added%20Features%20Portland%20Model%20Accuracy.png "Added features model accuracy")
![alt text](https://github.com/jchiu1013/Housing_Capstone/blob/master/Images/Portland%20LSTM%20Model%20-%20Unemployment%2C%20CPI%2C%20Lag%2012%20months%20-%20predict%201%20month%20ahead.png "Combined Model")

### Conclusion: 
- LSTM 18 months best model for predicting prices
- More months of data help, but limited by correlation
- Predicting more months ahead widens error
- Adding features helps reduce error, but not significantly

### Application: 

- Homebuyer: 
  - Best time to buy? 
  - Best price to bid? 
- Renter: 
  - Best time to lock in a rental contract? 
- Government: 
  - There is a upward trend in prices. 
  - Increase the supply of homes? 

[1]: https://www.zillow.com/research/data/
[2]: https://fred.stlouisfed.org
[3]: https://fred.stlouisfed.org/series/CPIAUCSL