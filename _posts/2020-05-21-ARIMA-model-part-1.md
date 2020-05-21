---
layout: post
title: "ARIMA model for Stock Forecasting: Part One"
tagline: "ARIMA (Autoregressive Integrated Moving Average) for Stock forecasting with R: Exploratory Data Analysis"
image: /thumbnail-mobile.png
author: "Ashiq Zaman"
meta: "Springfield"
---
This is a part one of ARIMA forecasting for STOXX50 index. This part going to show some exploratory Data analysis which shows preparing R with required packages, importing data and converting data to time series data. 

load all required packages:


```{r}
library(ggplot2)
library(forecast)
library(tseries)
library(tidyverse)
library(rio)
library(readxl)
library(tidyquant)
```

Import data from the locally downloaded file:

```{r}
data.price<-import("C:/Users/ashiq/Desktop/pilot Study/price 1.csv")
head(data.price)
```

Now convert the data into a date class (must be in yyyy-mm-dd format from the Excel or you need to use different function):

```{r}
data.price$DATES=as.Date(data.price$Date)
```

Plot the STOXX50 to show data pattern over time:

```{r}
ggplot(data.price,aes(DATES, STOXX50))+geom_line()+ scale_x_date(date_labels = "%b %y", date_breaks = "6 months")+ylab("Daily STOXX50 Price")+xlab("")
```

![arima1](https://user-images.githubusercontent.com/47462688/82580751-80ae0580-9b87-11ea-9219-a5017184dd30.JPG)


Plot month over month to see the range and outerliners

```{r}
ggplot(data.price,aes(DATES,STOXX50))+geom_point(color="navyblue")+facet_wrap(~Month)+geom_line()+ylab("Daily STOXX50 Price")+xlab("")
```

![arima 2](https://user-images.githubusercontent.com/47462688/82580849-a6d3a580-9b87-11ea-9321-7031a8c0ee37.JPG)

Create a time series object based on STOXX50 to pass to tsclean()

```{r}
count_TSObject=ts(data.price[,c("STOXX50")]) 

data.price$clean_count=tsclean(count_TSObject) # tsclean function to ID and replace outliners and input missing values if they exist. 

# Graph cleaned data
ggplot()+geom_line(data = data.price,aes(x=DATES, y=clean_count))+ylab("Cleaned Count")+ scale_x_date(date_labels = "%b %y", date_breaks = "6 months")
```

Then I computed both weekly Moving Average and Monthly Moving Average to cleaned daily data which still has a lot of variance and volatility in it. 

```{r}
data.price$cnt_ma=ma(data.price$clean_count,order = 7) # weekly Moving Average (MA) with clean count no outliners.
data.price$cnt_ma30=ma(data.price$clean_count,order = 30) # Monthly Moving Average (MA) with clean count no outliners.
ggplot()+geom_line(data = data.price,aes(x=DATES,y=clean_count, colour="STOXX50"))+geom_line(data = data.price,aes(x=DATES,y=cnt_ma, colour="Weekly Moving Average"))+geom_line(data = data.price,aes(x=DATES, y=cnt_ma30, colour="Monthly Moving Average"))+ylab("STOXX50")+ scale_x_date(date_labels = "%b %y", date_breaks = "6 months")
```

![arima 3](https://user-images.githubusercontent.com/47462688/82581053-de425200-9b87-11ea-871c-5a6e74737056.JPG)




**References**

Dancho, M., 2020. R Quantitative Analysis Package Integrations In Tidyquant. [online] Cran.r-project.org. Available at: <https://cran.r-project.org/web/packages/tidyquant/vignettes/TQ02-quant-integrations-in-tidyquant.html>.
