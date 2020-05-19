---
layout: post
title: "Structural Break Cointegration Test between Stock Markets"
tagline: "An analysis of Bai and Perron Cointegration test of Stock Markets with R"
categories: junk
image: /thumbnail-mobile.png
author: "Ashiq Zaman"
meta: "Springfield"
---

The foundation for estimating breaks in time series regression models was given by Bai (1994) and was extended to multiple breaks by Bai (1997ab) and Bai & Perron (1998). Bai and (2003) Perron test will use the followign classical linear regression model.

![bp 1](https://user-images.githubusercontent.com/47462688/82270666-6f89ac80-996d-11ea-86ce-c18b4a8413aa.JPG)

Where, it assumes m breakpoints and coefficients shift from one stable regression relationship to a different regression relationship. There will be total m+1 segment which will have constant regression. So, the new model can be represented as

![bp 2](https://user-images.githubusercontent.com/47462688/82270667-70224300-996d-11ea-9639-1d306ea4a072.JPG)

Where j represents the segment index. 

The test compute triangular RSS matrix which show Residual Sum of Squares (RSS) for each segment. The test use BIC estimator of the number of breakpoint. 

### Bai and Perron Cointegration test R code:

**Load relevant packages:**

```{r}
library(zoo)
library(strucchange)
library(ggplot2)
```

**Load data set from CSV, Convert into time series and converrt to data frame:**

```{r}
price<-read.csv("price.csv")
head(price)
price[,1]<-as.Date(price[,1],tz="UTC", format = "%d/%m/%Y")
head(price)
stoxx<-data.frame(Time=price$Date, Indices=price$STOXX50)
CAC40<-data.frame(Time=price$Date,Indices=price$CAC40)
dax30<-data.frame(Time=price$Date, Indices=price$DAX30)
russell<-data.frame(Time=price$Date, Indices=price$Russell1000)
```

**Now use the breakpoints function to implements the algorithm described in Bai & Perron (2003) for simultaneous estimation of multiple breakpoints.**


**References**

Bai J., Perron P. (2003), Computation and Analysis of Multiple Structural Change Models, Journal of Applied Econometrics, 18, 1-22.

