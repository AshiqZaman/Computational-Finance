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

**Import Pilot data from a CSV file (preloaded data from Thomson Reuters datastream)**

```{r}
price<-read.csv("price.csv")
```

**Changes the dates as factors to actual dates**

```{r}
library(zoo)
price.zoo=zoo(price[,-1], order.by=as.Date(strptime(as.character(price[,1]), "%d/%m/%Y")))
head(price.zoo)

          STOXX50    DAX30    CAC40     Russell1000
2010-01-01 7.994619 8.692394 8.278004    7.070286
2010-01-04 8.012283 8.707533 8.297536    7.086295
2010-01-05 8.010479 8.704811 8.297272    7.089636
2010-01-06 8.009582 8.705220 8.298457    7.090651
2010-01-07 8.008811 8.702736 8.300230    7.094652
2010-01-08 8.012300 8.705764 8.305271    7.097789
```
**Given each series a name**

```{r}
stoxx<- price.zoo[,1]
dax30<- price.zoo[,2]
cac40<- price.zoo[,3]
russell<- price.zoo[,4]
```

**Load urca to run ADF test**

```{r}
library(urca)
```

**Run ADF test with one lag with lag selection AIC neither an intercept nor a trend is included in the test regression**

```{r}
adf.stoxx.none<-ur.df(stoxx,type = c("none"),lags = 1, selectlags = "AIC")
summary(adf.stoxx.none)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression none 


Call:
lm(formula = z.diff ~ z.lag.1 - 1 + z.diff.lag)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.132369 -0.005705  0.000096  0.006212  0.098561 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)
z.lag.1    -4.057e-06  3.132e-05  -0.130    0.897
z.diff.lag  1.461e-03  1.926e-02   0.076    0.940

Residual standard error: 0.01304 on 2695 degrees of freedom
Multiple R-squared:  8.361e-06,	Adjusted R-squared:  -0.0007337 
F-statistic: 0.01127 on 2 and 2695 DF,  p-value: 0.9888


Value of test-statistic is: -0.1295 

Critical values for test statistics: 
      1pct  5pct 10pct
tau1 -2.58 -1.95 -1.62
```

**Run ADF test with one lag with lag selection AIC: with drift**

```{r}
adf.stoxx.drift<-ur.df(stoxx,type = c("drift"), lags = 1, selectlags = "AIC")
summary(adf.stoxx.drift)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression drift 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + z.diff.lag)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.132557 -0.005590  0.000417  0.006275  0.097817 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)  
(Intercept)  0.034597   0.014392   2.404   0.0163 *
z.lag.1     -0.004319   0.001795  -2.406   0.0162 *
z.diff.lag   0.003630   0.019263   0.188   0.8506  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.01303 on 2694 degrees of freedom
Multiple R-squared:  0.002146,	Adjusted R-squared:  0.001405 
F-statistic: 2.897 on 2 and 2694 DF,  p-value: 0.05537


Value of test-statistic is: -2.4058 2.8977 

Critical values for test statistics: 
      1pct  5pct 10pct
tau2 -3.43 -2.86 -2.57
phi1  6.43  4.59  3.78
```

**Run ADF test with one lag with lag selection AIC: with trend**

```{r}
adf.stoxx.trend<-ur.df(stoxx,type = c("trend"), lags = 1, selectlags = "AIC")
summary(adf.stoxx.trend)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression trend 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + tt + z.diff.lag)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.134032 -0.005497  0.000497  0.006237  0.098413 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)   
(Intercept)  6.509e-02  2.001e-02   3.252  0.00116 **
z.lag.1     -8.291e-03  2.551e-03  -3.250  0.00117 **
tt           1.003e-06  4.580e-07   2.190  0.02858 * 
z.diff.lag   5.842e-03  1.928e-02   0.303  0.76186   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.01302 on 2693 degrees of freedom
Multiple R-squared:  0.00392,	Adjusted R-squared:  0.002811 
F-statistic: 3.533 on 3 and 2693 DF,  p-value: 0.01422


Value of test-statistic is: -3.2503 3.5338 5.2968 

Critical values for test statistics: 
      1pct  5pct 10pct
tau3 -3.96 -3.41 -3.12
phi2  6.09  4.68  4.03
phi3  8.27  6.25  5.34
```

### Philipps-Perron

In Phillips-Perron test it makes the error terms weakly dependent and another properties of this test is the error terms are heterogeneously distributed. 

The Phillips-Perron (PP) test is conducted based on the equation (4.2)

![unit pp](https://user-images.githubusercontent.com/47462688/81879712-6f5f6a80-9583-11ea-845b-80d2481d700d.JPG)

where Δ represents first difference operator, β represents constant and µt represents error term. Equation (4.2a), (4.2b) and (4.2c) represents PP without constant and time trend, with constant and with constant and time trend accordingly. 

*Run PP test with Z alpha type and model constant* 

```{r}
pp.stoxx.constant<-ur.pp(stoxx, type = c("Z-alpha"), model = c("constant"), lags = c("short"))
summary(pp.stoxx.constant)

################################## 
# Phillips-Perron Unit Root Test # 
################################## 

Test regression with intercept 


Call:
lm(formula = y ~ y.l1)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.132568 -0.005588  0.000428  0.006275  0.097654 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 0.034534   0.014376   2.402   0.0164 *  
y.l1        0.995690   0.001793 555.284   <2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.01303 on 2696 degrees of freedom
Multiple R-squared:  0.9913,	Adjusted R-squared:  0.9913 
F-statistic: 3.083e+05 on 1 and 2696 DF,  p-value: < 2.2e-16


Value of test-statistic, type: Z-alpha  is: -11.1338 

         aux. Z statistics
Z-tau-mu            2.3502
```

**Run PP test with Z alpha type and model trend** 

```{r}
pp.stoxx.trend<-ur.pp(stoxx, type = c("Z-alpha"), model = c("trend"), lags = c("short"))
summary(pp.stoxx.trend)

################################## 
# Phillips-Perron Unit Root Test # 
################################## 

Test regression with intercept and trend 


Call:
lm(formula = y ~ y.l1 + trend)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.133993 -0.005497  0.000497  0.006216  0.098137 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) 6.514e-02  2.039e-02   3.195  0.00142 ** 
y.l1        9.919e-01  2.544e-03 389.950  < 2e-16 ***
trend       9.665e-07  4.569e-07   2.115  0.03449 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.01302 on 2695 degrees of freedom
Multiple R-squared:  0.9913,	Adjusted R-squared:  0.9913 
F-statistic: 1.544e+05 on 2 and 2695 DF,  p-value: < 2.2e-16


Value of test-statistic, type: Z-alpha  is: -21.3576 

           aux. Z statistics
Z-tau-mu              3.3191
Z-tau-beta            2.0785

```


### KPSS

The KPSS test, short for, Kwiatkowski-Phillips-Schmidt-Shin (KPSS), is a type of Unit root test that tests for the stationarity of a given series around a deterministic trend.

**Read data as CSV file**

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
**Changes the dates as factors to actual dates**

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

**given each series a name**

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

**KPSS test for UK before crisis**

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
