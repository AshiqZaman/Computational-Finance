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
chartSeries(IBM)
```

![quant 2](https://user-images.githubusercontent.com/47462688/81946384-352fb080-95f7-11ea-82f1-eb73f4d87387.JPG)

#### Calculating Returns

#### Loading pre-downloaded data and converting them into TimeSeries

#### Visualizing Stock Data

### Moving Averages




### References
