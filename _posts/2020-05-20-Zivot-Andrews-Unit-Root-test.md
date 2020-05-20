---
layout: post
title: "Unit Root Tests with Structural Breaks"
tagline: "Zivot-Andrews (1992) unit root test with a single structural break: An investigation on Stock Market data with R"
categories: junk
image: /thumbnail-mobile.png
author: "Ashiq Zaman"
meta: "Springfield"
---
In this blog, I examine the issue of identifying unit roots in the presence of structural breaks. Stationary series have a mean and covariance that do not change over time. This implies that a series is mean-reverting and any shock to the series will have a temporary effect. A number of different unit root tests have emerged from the research surrounding structural breaks and unit roots. These tests vary depending on the number of breaks in the data, whether a trend is present or not, and the null hypothesis that is being tested. Today I'll use Zivot-Andrews test to check Unit Root for Stock Index.

### Zivot-Andrews Test to analyze the presense of a unit root & structural break

**Read data from CSV file**

```{r}
price<-read.csv("price.csv")
```

**Converting the data into time series by using zoo package**

```{r}
library(zoo)
price.zoo=zoo(price[,-1], order.by=as.Date(strptime(as.character(price[,1]), "%d/%m/%Y")))
head(price.zoo)

            STOXX50    DAX30    CAC40 Russell1000
2010-01-01 7.994619 8.692394 8.278004    7.070286
2010-01-04 8.012283 8.707533 8.297536    7.086295
2010-01-05 8.010479 8.704811 8.297272    7.089636
2010-01-06 8.009582 8.705220 8.298457    7.090651
2010-01-07 8.008811 8.702736 8.300230    7.094652
2010-01-08 8.012300 8.705764 8.305271    7.097789
```

**Assign each stock index into a new time series to prepare for analysis**

```{r}
stoxx<- price.zoo[,1]
dax30<- price.zoo[,2]
cac40<- price.zoo[,3]
russell<- price.zoo[,4]
```

*Load all necessary r packages*

```{r}
library(urca)
```

The Zivot-Adrews test to analyze the presence of unit root and structural break in the data set.If the unit root tests find that a series contain one unit root, the appropriate route in this case is to transform the data by differencing the variables prior to their inclusion in the regression model, but this incurs a loss of important long-run information.it facilitates the analysis of whether a structural break on a certain variable is associated with a particular event such as a change in government policy, a currency crisis, war and so forth. The Zivot-Andrews test only allow for one structural break. In Zivot-Andrews test a break date will be chosen where the evidence is least favorable for the unit root null. In the Zivot-Andrews tests, the null hypothesis is that the series has a unit root with structural break(s) against the alternative hypothesis that they are stationary with break(s). REject Null if t-value statistic is lower than tabulated critical value (left tailed test).

*ZA test for CAC40 with intercept*

```{r}
cac40.za.intercept <- ur.za(cac40, model=c("intercept"), lag=1)
summary(cac40.za.intercept)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.125011 -0.005653  0.000424  0.006340  0.092540 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)  1.120e-01  2.316e-02   4.838 1.38e-06 ***
y.l1         9.862e-01  2.854e-03 345.580  < 2e-16 ***
trend        3.026e-06  6.676e-07   4.533 6.07e-06 ***
y.dl1        5.378e-03  1.921e-02   0.280     0.78    
du          -9.395e-03  1.953e-03  -4.811 1.59e-06 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.01285 on 2692 degrees of freedom
  (2 observations deleted due to missingness)
Multiple R-squared:  0.9947,	Adjusted R-squared:  0.9947 
F-statistic: 1.272e+05 on 4 and 2692 DF,  p-value: < 2.2e-16


Teststatistic: -4.8446 
Critical values: 0.01= -5.34 0.05= -4.8 0.1= -4.58 

Potential break point at position: 2646 
```

The Zivot-Andrews test suggests that there is break in data trend at observation number 2646.The T-statistics of Z-A test is lower than critical value (in absolute value), REJECTING Null hypothesis of unit root. The data has structure break at 20 February 2020.

*Plot the Unit root test to visualise data*

```{r}
plot(cac40.za.intercept)
```

![za1](https://user-images.githubusercontent.com/47462688/82130084-fdc03000-97bf-11ea-8522-01cf195b5917.JPG)

*ZA test for CAC40 with trend*

```{r}
cac40.za.trend <- ur.za(cac40, model=c("trend"), lag=1)
summary(cac40.za.trend)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 

Call:
lm(formula = testmat)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.128957 -0.005632  0.000457  0.006400  0.092623 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)  9.904e-02  2.302e-02   4.302 1.75e-05 ***
y.l1         9.878e-01  2.838e-03 348.101  < 2e-16 ***
trend        2.636e-06  6.688e-07   3.941 8.31e-05 ***
y.dl1        8.141e-03  1.924e-02   0.423 0.672291    
dt          -9.206e-05  2.741e-05  -3.359 0.000794 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.01288 on 2692 degrees of freedom
  (2 observations deleted due to missingness)
Multiple R-squared:  0.9947,	Adjusted R-squared:  0.9947 
F-statistic: 1.266e+05 on 4 and 2692 DF,  p-value: < 2.2e-16


Teststatistic: -4.3054 
Critical values: 0.01= -4.93 0.05= -4.42 0.1= -4.11 

Potential break point at position: 2606 

```

The Zivot-Andrews test suggests that there is break in data trend at observation number 2606.The T-statistics of Z-A test is lower than critical value (in absolute value), REJECTING Null hypothesis of unit root. The data has structure break at 27 December 2019.

*Plot the Unit root test to visualise data*

```{r}
plot(cac40.za.trend)
```

![za 2](https://user-images.githubusercontent.com/47462688/82130101-1cbec200-97c0-11ea-8ed4-03bdb5e0ddd2.JPG)


### References
