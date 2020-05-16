---
layout: post
title: "Introduction to Stock Analysis with R"
tagline: "Geeting data with quantmod, Import from CSV file and Visualize data"
---

Firstly, we install and load the necessary packages.To analyse stock data and to get stock data from the internet, we will use quantmod and ggplot2. Firstly, we install and load the necessary packages.

```{r}
rm(list=ls())
library(quantmod)
library(ggplot2)
```

```{r}
library(quantmod)
library(xts)
library(rvest)
library(tidyverse)
library(stringr)
library(forcats)
library(lubridate)
library(plotly)
library(dplyr)
library(PerformanceAnalytics)
```

### Getting and Visualizing Stock Data
**Getting Data with quantmod from Yahoo! Finance**

To get data from internet we can use quantmod and use getSymbols function. where IBM is the sticker symbol of company IBM from Yahoo Finance of the stock that we're going to analyse.The argument src= "yahoo" indicates the source of the data. The third and fourth arguments indicate the time period in which the data is going to be extracted, with data in the format “yyyy-mm-dd”.


Log return data of Amazon, Google, Apple, Facebook and IBM stock between Jan 2010 and May 2020:

```{r}
getSymbols("AMZN",from="2010-01-01",to="2020-05-16")
AMZN_log_returns<-AMZN%>%Ad()%>%dailyReturn(type='log')

getSymbols("GOOGL",from="2010-01-01",to="2020-05-16")
GOOGL_log_returns<-GOOGL%>%Ad()%>%dailyReturn(type='log')

getSymbols("AAPL",from="2010-01-01",to="2020-05-16")
AAPL_log_returns<-AAPL%>%Ad()%>%dailyReturn(type='log')

getSymbols("FB",from="2008-01-01",to="2020-05-16")
FB_log_returns<-FB%>%Ad()%>%dailyReturn(type='log')

getSymbols("IBM",from="2010-01-01",to="2020-05-16")
IBM_log_returns<-IBM%>%Ad()%>%dailyReturn(type='log')


[1] "AMZN"
[1] "GOOGL"
[1] "AAPL"
[1] "FB"
[1] "IBM"

```

Sample data: 

```{r}
head(IBM)

            IBM.Open IBM.High IBM.Low IBM.Close IBM.Volume IBM.Adjusted
2010-01-04   131.18   132.97  130.85    132.45    6155300     94.72456
2010-01-05   131.68   131.85  130.10    130.85    6841400     93.58028
2010-01-06   130.68   131.49  129.81    130.00    5605300     92.97238
2010-01-07   129.87   130.25  128.91    129.55    5840600     92.65054
2010-01-08   129.07   130.92  129.05    130.85    4197200     93.58028
2010-01-11   131.06   131.06  128.67    129.48    5730400     92.60048
```


**Technical Analysis:** Draw two charts where first chart is very straight forward as it shows IBM price chart and the second chart shows the Bollinger Band chart, % Bollinger change, Volume Traded and Moving Average Convergence Diverence in 2018 alone:

```{r}
AMZN%>%Ad()%>%chartSeries(theme = "white")
AMZN%>%chartSeries(TA='addBBands();addVo();addMACD()', subset="2019",theme = "white")

GOOGL%>%Ad()%>%chartSeries(theme = "white")
GOOGL%>%chartSeries(TA='addBBands();addVo();addMACD()',subset="2019",theme = "white")

AAPL%>%Ad()%>%chartSeries(theme = "white")
AAPL%>%chartSeries(TA='addBBands();addVo();addMACD()',subset='2019', theme = "white")

FB%>%Ad()%>%chartSeries(theme = "white")
FB%>%chartSeries(TA='addBBands();addVo();addMACD()',subset='2019', theme = "white")

IBM%>%Ad()%>%chartSeries(theme = "white")
IBM%>%chartSeries(TA='addBBands();addVo();addMACD()',subset='2019', theme = "white")

```

![bolinger 1](https://user-images.githubusercontent.com/47462688/82132378-138d1f80-97d7-11ea-9dc1-c8c37d7901cc.JPG)

![ibm 1](https://user-images.githubusercontent.com/47462688/82132381-1851d380-97d7-11ea-95f9-41b5e95ec7fd.JPG)

The moving average is important to understand technical charts. It smoothes out daily price fluctuations by averaging stock prices and is effective in identifying potential trends.

The Bollinger Band chart plots two standard deviations away from the moving average and is used to measure the stock’s volatiliy. The Volume chart shows how its stocks are traded on the daily. The Moving Average Convergence Divergence gives technical analysts buy/sell signals. The rule of thumb is: If it falls below the line, it is time to sell. If it rises above the line, it is experiencing an upward momentum.


Diversify your investment alogn with Google, Facebook and Amazon:

```{r}
data<-cbind(diff(log(Cl(AMZN))),diff(log(Cl(GOOGL))),diff(log(Cl(AAPL))),diff(log(Cl(IBM))),diff(log(Cl(FB))))
chart.Correlation(data)
```

![corr 1](https://user-images.githubusercontent.com/47462688/82132438-a463fb00-97d7-11ea-850b-4caf3ee5ac70.JPG)

Facebook (FB) and IBM have the smallest correlation of 0.29 while Amazon (AMZN) and Google (GOOGL) have the highest correlation of 0.57. The correlation between each stocks is high because they are all technology stocks. 

It is better to purchase stocks from different sectors to truly minimize the risk and maximize rates of return.


To calcualte return of IBM stock price, we need to create new object with calculated returns using closing price with log return of stock price.

*rt= ln(Pt)-ln(Pt-1)*

```{r}
IBM_ret<-diff(log(IBM[,4]))
IBM_ret<-IBM_ret[,1]
head(IBM_ret)

             IBM.Close
2013-01-02           NA
2013-01-03 -0.005515575
2013-01-04 -0.006576600
2013-01-07 -0.004391328
2013-01-08 -0.001398948
2013-01-09 -0.002855673
```

we can visulaize calcualted return: 

```{r}
plot(IBM_ret)
```
![return 1](https://user-images.githubusercontent.com/47462688/81949564-e6841580-95fa-11ea-889e-e83ce5e7e473.JPG)

It is possible to caluate return by using quantmood:

```{r}
daily.ret.IBM<-dailyReturn(IBM)
head(daily.ret.IBM)

 daily.returns
2013-01-02   0.011644134
2013-01-03  -0.005500392
2013-01-04  -0.006555021
2013-01-07  -0.004381700
2013-01-08  -0.001397970
2013-01-09  -0.002851600
```

To plot the calcualted daily return 

```{r}
chart_Series(daily.ret.IBM)
```

![ret 2](https://user-images.githubusercontent.com/47462688/81949579-e97f0600-95fa-11ea-8907-36e4960ea736.JPG)

It is also possible to calcuate other returns also 

```{r}
weekly.ret.IBM<-weeklyReturn(IBM)
monthly.ret.IBM<-monthlyReturn(IBM)
quartely.ret.IBM<-quarterlyReturn(IBM)
yearly.ret.IBM<-yearlyReturn(IBM)
```

#### Loading pre-downloaded data and converting them into TimeSeries

Import Pilot data from a CSV file (preloaded data from Thomson Reuters datastream and use logarithm).

```{r}
price<-read.csv("price.csv")
head(price)
```

Change the dates as factors to actual dates

```{r}
price[,1]<-as.Date(price[,1],tz="UTC", format = "%d/%m/%Y")
head(price)
```

![h 1](https://user-images.githubusercontent.com/47462688/81950700-34e5e400-95fc-11ea-8ea7-bc9e04b92009.JPG)

#### Visualizing Stock Data

*use ggplot2  to plot STOXX50*

```{r}
library(ggplot2)
ggplot(price,aes(Date,STOXX50))+geom_line()+xlab("Year")+ylab("Index")
```

![stock price 1](https://user-images.githubusercontent.com/47462688/81951070-a0c84c80-95fc-11ea-9922-a651e19acacd.JPG)

multiple series in the same graph using ggplot

*data frame each series*

```{R}
stoxx<-data.frame(Time=price$Date, Indices=price$STOXX50)
CAC40<-data.frame(Time=price$Date,Indices=price$CAC40)
dax30<-data.frame(Time=price$Date, Indices=price$DAX30)
russell<-data.frame(Time=price$Date, Indices=price$Russell1000)
```

```{r}
ggplot(stoxx,aes(Time,Indices))+geom_line(aes(color="STOXX50"))+geom_line(data=CAC40,aes(color="CAC40"))+geom_line(data=dax30,aes(color="DAX30"))+ geom_line(data = russell,aes(color="Russell1000"))+ ggtitle("Stock Prices: STOXX50, DAX30, CAC40 & Russell1000")+theme(plot.title = element_text(hjust = 0.5))+ scale_x_date(date_labels = "%b %y", date_breaks = "6 months")+labs(color="Country")
```

![st 2](https://user-images.githubusercontent.com/47462688/81951359-f8ff4e80-95fc-11ea-888c-f3b962a6e109.JPG)

### Moving Averages

Calculating 10 day & 20 day moving average of IBM closing stock price: Calculates the arithmetic mean of the series over the past n observations.

```{r}
IBM_MA10<-SMA(IBM[,4,n=10]) # 10 day moving average
IBM_MA20<-SMA(IBM[,4],n=20) # 20 day moving average
plot(IBM_MA10)
plot(IBM_MA20)
```

![MA 10](https://user-images.githubusercontent.com/47462688/81986562-f1a56880-962f-11ea-826c-93e250255f95.JPG)

![MA20](https://user-images.githubusercontent.com/47462688/81986573-f4a05900-962f-11ea-8abc-8b0b116f88fa.JPG)

Exponential moving average. calculates an exponentially-weighted mean, giving more weight to recent observations.

```{r}
IBM_EMA10<-EMA(IBM[,4,n=10]) # 10 day Exponential moving average
IBM_EMA20<-EMA(IBM[,4],n=20) # 20 day Exponential moving average
plot(IBM_MA10)
plot(IBM_MA20)
```

![SMA 1](https://user-images.githubusercontent.com/47462688/81986994-c3745880-9630-11ea-83e2-061106673a18.JPG)

![SMA 2](https://user-images.githubusercontent.com/47462688/81986996-c40cef00-9630-11ea-88e3-732604dc4a62.JPG)


**References**

will add soon
