---
layout: post
title: "Introduction to Stock Analysis with R"
tagline: "Ashiq Zaman"
---

Firstly, we install and load the necessary packages.To analyse stock data and to get stock data from the internet, we will use quantmod and ggplot2. Firstly, we install and load the necessary packages.

```{r}
rm(list=ls())
library(quantmod)
library(ggplot2)
```

### Getting and Visualizing Stock Data
#### Getting Data with quantmod from Yahoo! Finance

To get data from internet we can use quantmod and use getSymbols function. where IBM is the sticker symbol of company IBM from Yahoo Finance of the stock that we're going to analyse.The argument src= "yahoo" indicates the source of the data. The third and fourth arguments indicate the time period in which the data is going to be extracted, with data in the format “yyyy-mm-dd”.

```{r}
IBM<- getSymbols("IBM", src = "yahoo", from = "2013-01-01", to = "2020-05-01", auto.assign = FALSE)
head(IBM)

  IBM.Open IBM.High IBM.Low IBM.Close IBM.Volume IBM.Adjusted
2013-01-02   194.09   196.35  193.80    196.35    4234100     147.9810
2013-01-03   195.67   196.29  194.44    195.27    3644700     147.1670
2013-01-04   194.19   194.46  192.78    193.99    3380200     146.2024
2013-01-07   193.40   193.78  192.34    193.14    2862300     145.5618
2013-01-08   192.92   193.30  191.60    192.87    3026900     145.3583
2013-01-09   193.48   193.49  191.65    192.32    3212000     144.9438
```

Use chartseries funciton of quantmod to visualise IBM. 

```{r}
chartSeries(IBM, theme="white",TA="addVo();addBBands();addCCI()")
```

![quant 2](https://user-images.githubusercontent.com/47462688/81946384-352fb080-95f7-11ea-82f1-eb73f4d87387.JPG)

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

use ggplot2  to plot STOXX50

```{r}
library(ggplot2)
ggplot(price,aes(Date,STOXX50))+geom_line()+xlab("Year")+ylab("Index")
```

![stock price 1](https://user-images.githubusercontent.com/47462688/81951070-a0c84c80-95fc-11ea-9922-a651e19acacd.JPG)

multiple series in the same graph using ggplot

data frame each series

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




### References
