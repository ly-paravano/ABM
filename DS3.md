## Data Insight 3 - Time Series Analysis

For this data science insight, I wanted to discuss different time series analysis models and how effective they are at directly predicting the future. 



Before we introduce any model we need to understand if our data contains any **autocorrelation**, **seasonality** and **stationary**

* Autocorrelation 

Autocorellation is the similarity between observations as a function of the time lag between them.

![](/Users/lucaparavano/Desktop/Screen Shot 2020-10-31 at 8.01.52 AM.png)

Above is an example of an autocorrelation plot. The first value and the 24th value have a high autocorrelation. Similarly, the 12th and 36th observations are highly correlated. This means that we will find a very similar value at every 24 unit of time.



* Seasonality

Seasonality refers to periodic fluctuations. For example, electricity consumption is high during the day and low during night, or the increase in flu shots duirng the last quarter of the year.

![](/Users/lucaparavano/Desktop/Screen Shot 2020-10-31 at 8.02.03 AM.png)

* Stationary

Stationarity is an important characteristic of time series. A time series is said to be stationary if its statistical properties do not change over time. In other words, it has constant mean and variance, and covariance is independent of time.

Ideally, we want to have a stationary time series for modelling. Of course, not all of them are stationary, but we can make different transformations to make them stationary.



Once that is established we can begin impletementing models, for the purposes of this DSI I will be going over:

* Moving Averages
* Exponential Smoothing 

#### **Moving Averages**

Very simple model, just take the last X periods and calculate the average and that will be your next periods predicted variable. 

For example, lets say you have the data:

```
| Day      |   Weather (C) |  
|----------|:-------------:|
| DAY 1.   |  			13		 | 
| DAY 2.   |  			15		 | 
| DAY 3.   |  			16.2	 | 
| DAY 4.   |  			15		 |               
| DAY 5.   |  			14		 |               
| DAY 6.   |  			12		 |               
| DAY 7.   |  			10		 |               
| DAY 9.   |  			12		 |               

```

If we were using a three day moving average, our predictions would start at day 4, and that value would be (DAY1+DAY2+DAY3)/3.

We can complicate our model by adding seperate weights to days. We would want to do this if we prioritize the most recent day of data over the data that is at the very end of our weighted moving average. For example, for that same 3 day moving average we could implement (.60,.30,10) to our model very easily. That would look like: 0.6(DAY3) + 0.3(DAY2) + 0.1(DAY3).

When using a standard moving average our predicted Day value would of been **14.73**, whilst using a weighted moving average our Day 4 value comes out to **15.52**.

In this example, the standard moving average was better at predicting our day 4 value. I believe this is the case because our DAY 3 value seems to be an outlier / peak and our WMA is largely made up of that value. 

#### **Exponential Smoothing**

Exponential smoothing uses a similar logic to moving average, but this time, a different decreasing weight is assigned to each observations. In other words, less importance is given to observations as we move further from the present.

Mathematically, exponential smoothing is expressed as:

![](/Users/lucaparavano/Desktop/Screen Shot 2020-10-31 at 8.21.43 AM.png)



As we can see from the formula, we are only using the previous value to form our predictions. For example, if we have our same formualted data

```
| Day      |   Weather (C) |  
|----------|:-------------:|
| DAY 1.   |  			13		 | 
| DAY 2.   |  			15		 | 
| DAY 3.   |  			16.2	 | 
| DAY 4.   |  			15		 |               
| DAY 5.   |  			14		 |               
| DAY 6.   |  			12		 |               
| DAY 7.   |  			10		 |               
| DAY 9.   |  			12		 |  
```



If we wanted to calculate the weather for day 4 again, we would do (with a smoothing coeff of .2):

DAY 4 = DAY3pred + SmoothingCoeff(DAY3actual - DAY3pred)

The higher the value of alpha the more sensitive our forecast is to variations in the trend. As we can see below:

![](/Users/lucaparavano/Desktop/Screen Shot 2020-10-31 at 9.13.00 AM.png)

The alpha of .3 follows the slope of the realized data almost perfectly, whilst the forecast with a smoothing coeffecient of .05 barely captures any of the variation. 







