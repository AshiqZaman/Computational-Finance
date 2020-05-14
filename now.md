---
layout: page
title: Unit Root Tests
tagline: Ashiq Zaman
permalink: /now.html
ref: now
order: 2
---

### Dickey Fuller (DF) & Augmented Dickey Fuller (ADF)

The main condition for any time series regressions is the data must be stationary. Any non-stationary time series result gives a spurious result . So, for any time series analysis stationarity test is prerequisite. Unit Root test is one of the tests of stationarity which becomes popular over the last few years. There are some ways of doing Unit Root test and each way has its own advantages and disadvantages. In this study two tests is used to avoid each other disadvantages. The tests are conducted in log form and first difference log form of individual market indices. If the result gives the rejection of null hypothesis then it can be said that the selected time series data are stationary. The ADF test investigate the existence of unit root in an Autoregressive (AR) model. If any explanatory variable is the lagged values of dependant variable, then it is known as AR model. The most commonly used AR model is yt=β+ρyt-1+εt where yt represents variable of interest, t represents time index and ρ and ε are coefficient and disturbance terms accordingly. In this study the Augmented Dickey-Fuller (ADF) test is conducted based on the following regressions

![unit 1](https://user-images.githubusercontent.com/47462688/81876416-8baad980-957a-11ea-968f-25a06b981017.JPG)

where Δ represents the first difference operator, m represents the number of lags, εt represents pure white noise error term and Yt represents time series. Equation (4.1a) represents the pure random walk model without constant and trend and equation (4.1b) represents random walk with constant but without time trend and (4.1c) represents random walk with constant and time trend.

Before doing ADF test it assumes that the error terms of the model are statistically significant, and it has a constant variance. 


### Philipps-Perron

In Phillips-Perron test it makes the error terms weakly dependent and another properties of this test is the error terms are heterogeneously distributed. 

The Phillips-Perron (PP) test is conducted based on the equation (4.2)

![unit pp](https://user-images.githubusercontent.com/47462688/81879712-6f5f6a80-9583-11ea-845b-80d2481d700d.JPG)

where Δ represents first difference operator, β represents constant and µt represents error term. Equation (4.2a), (4.2b) and (4.2c) represents PP without constant and time trend, with constant and with constant and time trend accordingly. 

### KPSS

The KPSS test, short for, Kwiatkowski-Phillips-Schmidt-Shin (KPSS), is a type of Unit root test that tests for the stationarity of a given series around a deterministic trend.

*Read data as CSV file*
```{r}
kpss<-read.csv("kpss_before.csv")
head(kpss)

##         DATE       UK      USA  GERMANY   FRANCE   CANADA    JAPAN
## 1 03/01/2000 8.843644 7.282912 8.817410 8.685647 9.037623 9.848732
## 2 04/01/2000 8.804754 7.243813 8.792846 8.643301 9.012206 9.852345
## 3 05/01/2000 8.785065 7.245734 8.779876 8.608806 9.002014 9.827823
## 4 06/01/2000 8.771407 7.246689 8.775692 8.603391 9.001376 9.807432
## 5 07/01/2000 8.780288 7.273419 8.821874 8.619679 9.039483 9.808815
## 6 10/01/2000 8.795992 7.284547 8.842968 8.638724 9.059808 9.808815
##   HONG.KONG    KOREA   BRAZIL   RUSSIA    INDIA    CHINA SOUTH.AFRICA
## 1  9.762479 6.935439 9.736842 5.166271 8.589534 7.282912     9.030878
## 2  9.745243 6.965118 9.670988 5.166271 8.610867 7.243813     9.028508
## 3  9.670718 6.893971 9.695540 5.187442 8.586159 7.245734     9.017965
## 4  9.625969 6.867756 9.686947 5.223163 8.598133 7.246689     9.019450
## 5  9.642488 6.855040 9.699472 5.223163 8.596832 7.273419     9.044496
## 6  9.670808 6.894913 9.742262 5.258797 8.615841 7.284547     9.077054
```
*Changes the dates as factors to actual dates*

```{r}
install.packages("zoo")
library(zoo)

d.zoo=zoo(kpss[,-1], order.by=as.Date(strptime(as.character(kpss[,1]), "%d/%m/%Y")))
head(d.zoo)

##                  UK      USA  GERMANY   FRANCE   CANADA    JAPAN HONG.KONG
## 2000-01-03 8.843644 7.282912 8.817410 8.685647 9.037623 9.848732  9.762479
## 2000-01-04 8.804754 7.243813 8.792846 8.643301 9.012206 9.852345  9.745243
## 2000-01-05 8.785065 7.245734 8.779876 8.608806 9.002014 9.827823  9.670718
## 2000-01-06 8.771407 7.246689 8.775692 8.603391 9.001376 9.807432  9.625969
## 2000-01-07 8.780288 7.273419 8.821874 8.619679 9.039483 9.808815  9.642488
## 2000-01-10 8.795992 7.284547 8.842968 8.638724 9.059808 9.808815  9.670808
##               KOREA   BRAZIL   RUSSIA    INDIA    CHINA SOUTH.AFRICA
## 2000-01-03 6.935439 9.736842 5.166271 8.589534 7.282912     9.030878
## 2000-01-04 6.965118 9.670988 5.166271 8.610867 7.243813     9.028508
## 2000-01-05 6.893971 9.695540 5.187442 8.586159 7.245734     9.017965
## 2000-01-06 6.867756 9.686947 5.223163 8.598133 7.246689     9.019450
## 2000-01-07 6.855040 9.699472 5.223163 8.596832 7.273419     9.044496
## 2000-01-10 6.894913 9.742262 5.258797 8.615841 7.284547     9.077054

```
*given each series a name*
```{r}
uk <- d.zoo[,1]
uk_diff<-diff(uk)
usa <- d.zoo[,2]
usa_diff<-diff(usa)
ger<-d.zoo[,3]
ger_diff<-diff(ger)
fra<-d.zoo[,4]
fra_diff<-diff(fra)
can<-d.zoo[,5]
can_diff<-diff(can)
jap<-d.zoo[,6]
jap_diff<-diff(jap)
hk<-d.zoo[,7]
hk_diff<-diff(hk)
kor<-d.zoo[,8]
kor_diff<-diff(kor)
bra<-d.zoo[,9]
bra_diff<-diff(bra)
rus<-d.zoo[,10]
rus_diff<-diff(rus)
ind<-d.zoo[,11]
ind_diff<-diff(ind)
chi<-d.zoo[,12]
chi_diff<-diff(chi)
sa<-d.zoo[,13]
sa_diff<-diff(sa)
```
### KPSS test for UK before crisis

*KPSS test with a drift*

```{r}
library(urca)
kpss.uk.drift<- ur.kpss(uk, type="mu", lags="short")
summary(kpss.uk.drift)

## 
## ####################### 
## # KPSS Unit Root Test # 
## ####################### 
## 
## Test is of type: mu with 8 lags. 
## 
## Value of test-statistic is: 6.195 
## 
## Critical value for a significance level of: 
##                 10pct  5pct 2.5pct  1pct
## critical values 0.347 0.463  0.574 0.739

kpss.uk.drift.diff<- ur.kpss(uk_diff, type="mu", lags="short")
summary(kpss.uk.drift.diff)

## 
## ####################### 
## # KPSS Unit Root Test # 
## ####################### 
## 
## Test is of type: mu with 8 lags. 
## 
## Value of test-statistic is: 0.2521 
## 
## Critical value for a significance level of: 
##                 10pct  5pct 2.5pct  1pct
## critical values 0.347 0.463  0.574 0.739
```

*KPSS test with a trend *
```{r}
kpss.uk.trend<- ur.kpss(uk, type="tau", lags="short")
summary(kpss.uk.trend)

## 
## ####################### 
## # KPSS Unit Root Test # 
## ####################### 
## 
## Test is of type: tau with 8 lags. 
## 
## Value of test-statistic is: 5.0498 
## 
## Critical value for a significance level of: 
##                 10pct  5pct 2.5pct  1pct
## critical values 0.119 0.146  0.176 0.216

kpss.uk.trend.diff<- ur.kpss(uk_diff, type="tau", lags="short")
summary(kpss.uk.trend.diff)
## 
## ####################### 
## # KPSS Unit Root Test # 
## ####################### 
## 
## Test is of type: tau with 8 lags. 
## 
## Value of test-statistic is: 0.1315 
## 
## Critical value for a significance level of: 
##                 10pct  5pct 2.5pct  1pct
## critical values 0.119 0.146  0.176 0.216
```

### References

will be added soon
