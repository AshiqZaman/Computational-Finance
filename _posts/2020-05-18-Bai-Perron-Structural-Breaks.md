---
layout: post
title: "Structural Break Cointegration Test between Stock Markets"
tagline: "An analysis of Bai and Perron Cointegration test of Stock Markets with R"
image: /thumbnail-mobile.png
author: "Ashiq Zaman"
meta: "Springfield"
---

The foundation for estimating breaks in time series regression models was given by Bai (1994) and was extended to multiple breaks by Bai (1997ab) and Bai & Perron (1998). Bai and Perron (2003) test will use the followign classical linear regression model.

![bp 1](https://user-images.githubusercontent.com/47462688/82270666-6f89ac80-996d-11ea-86ce-c18b4a8413aa.JPG)

Where, it assumes m breakpoints and coefficients shift from one stable regression relationship to a different regression relationship. There will be total m+1 segment which will have constant regression. So, the new model can be represented as

![bp 2](https://user-images.githubusercontent.com/47462688/82270667-70224300-996d-11ea-9639-1d306ea4a072.JPG)

Where j represents the segment index. 

The test compute triangular RSS matrix which show Residual Sum of Squares (RSS) for each segment. The test use BIC estimator of the number of breakpoint. 

### Bai and Perron (2003) Cointegration test R code:

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


**OLS based CUSUM Test:**

```{r}
ocus.price<-efp(STOXX50~Russell1000,data = price, type = "OLS-CUSUM")
plot(ocus.price)
```

![CUSUM 1](https://user-images.githubusercontent.com/47462688/82272624-09078d00-9973-11ea-8bdf-5d03f9c9d4fa.JPG)

**Tests based on F statistics**

```{r}
price.fstast.stoxx.russell<-Fstats(STOXX50~Russell1000,data = price, from = 0.15)
plot(price.fstast.stoxx.russell)
```

![F test 1](https://user-images.githubusercontent.com/47462688/82272986-20934580-9974-11ea-8cb3-d7908acb71dd.JPG)

**Now use the breakpoints function to implements the algorithm described in Bai & Perron (2003) for simultaneous estimation of multiple breakpoints.**

* Break Point Test:

```{r}
price.break.stoxx.russell<-breakpoints(STOXX50~Russell1000, data = price, breaks = 5)
summary(price.break.stoxx.russell)
plot(price.break.stoxx.russell)
```

![bp 3](https://user-images.githubusercontent.com/47462688/82273071-57695b80-9974-11ea-9f19-135ee00f6a58.JPG)


![bp 4](https://user-images.githubusercontent.com/47462688/82273073-5801f200-9974-11ea-858c-a2e2b679223c.JPG)

The Bai and Perron Test shows five different structural breaks at the follwoing dates:

*	2011: Japan Tsunami and Greece economic crisis.

*	2013-2014: China overtake USA in scientific research. 

* 2014-2015: Latvia and Lithonia joins Euro Zone and Brazil world cup. 

* 2016: Brexit, US election and Euro and Olympic

*	2018: US China trade War, US leaves Iran nuclear deal. 

These could be possible reasons but this post doesnot claim any connection between these events with the structural breaks. However, by lokking into internet, I can find these are the key economic events over the last 10 years. 

**References**

* Bai J., Perron P. (2003), Computation and Analysis of Multiple Structural Change Models, Journal of Applied Econometrics, 18, 1-22.

