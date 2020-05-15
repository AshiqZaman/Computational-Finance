---
layout: page
title: Cointegration Tests
tagline: Ashiq Zaman
permalink: /about.html
ref: about
order: 0
---

### Engle-Granger Test

The initial condition of co-integration test is the selected stock market need to be integrated of the first order I (1). If the initial condition fulfilled, then it is possible to examine the long-run relationship of the selected market. So, it is convenient to use residual based Engle-Granger Co-integration test. The first step of the Engle-Granger test is to use Ordinary Least Square (OLS) regression. Then the second step is to calculate its residuals. If the results show that the residuals are stationary, then it can be said that the chosen stock markets are co-integrated with each other. For Engle-Granger test the following model is used

![eg 1](https://user-images.githubusercontent.com/47462688/81881019-fbbf5c80-9586-11ea-8f15-095ffa7c284c.JPG)

where xt and yt are two different stock market indices. And et represents residual of this model and it can be predicted by using Augmented Dickey-Fuller (ADF) test. And the equation can be written as follows

![eg 2](https://user-images.githubusercontent.com/47462688/81881059-1bef1b80-9587-11ea-81d2-c19db554a938.JPG)

where γt = estimated parameters. εt = disturbance terms. To find out the long run relationship between markets, a hypothesis test is conducted on the coefficient (γt). If the results shows that the test statistics of the coefficients is greater than the critical value. Then it rejects the null hypothesis. So it could be conclude that the residual is stationary. This indicates that the markets are co-integrated in other word there is some linkages between the markets.      

*read data at CSV file* 

```{r}
price<-read.csv("price.csv")
head(price)
```
*1st step of Engle-granger test is to take OLS test*

```{r}
Engle.stoxx<-lm(STOXX50~DAX30, data = price)
summary(Engle.stoxx)

Call:
lm(formula = STOXX50 ~ DAX30, data = price)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.193098 -0.044365  0.001703  0.036345  0.199596 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 3.716828   0.042085   88.32   <2e-16 ***
DAX30       0.470479   0.004604  102.20   <2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.06339 on 2697 degrees of freedom
Multiple R-squared:  0.7948,	Adjusted R-squared:  0.7947 
F-statistic: 1.044e+04 on 1 and 2697 DF,  p-value: < 2.2e-16
```

*calcualte residual from OLS test and test unit root to finish second step of Engle Granger test*

```{r}
residual.stoxx<-resid(Engle.stoxx)
library(urca)
adf.residual.stoxx<-ur.df(residual.stoxx,type = c("trend"),lags = 1)
summary(adf.residual.stoxx)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression trend 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + tt + z.diff.lag)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.071546 -0.003404  0.000255  0.003716  0.074266 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)  1.640e-05  2.978e-04   0.055 0.956094    
z.lag.1     -8.524e-03  2.372e-03  -3.594 0.000331 ***
tt          -1.018e-07  1.924e-07  -0.529 0.596608    
z.diff.lag  -7.860e-03  1.925e-02  -0.408 0.683010    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.007558 on 2693 degrees of freedom
Multiple R-squared:  0.004948,	Adjusted R-squared:  0.003839 
F-statistic: 4.464 on 3 and 2693 DF,  p-value: 0.003919


Value of test-statistic is: -3.5941 4.5738 6.5133 

Critical values for test statistics: 
      1pct  5pct 10pct
tau3 -3.96 -3.41 -3.12
phi2  6.09  4.68  4.03
phi3  8.27  6.25  5.34

```

### Johansen (1988,1991) Bivariate and Multivariate Co-integration Test

The main disadvantage of Engle-Granger co-integration test is that if there are more than two variables in a model then this test ignore the possibility of getting more than one co-integrated vector. On the other hand Maximum Likelihood based Johansen Bivariate and Multivariate co-integration test offset the ADF test disadvantage. This test has been used to find out the long run relationship between international stock markets by many researchers. Majid et al. used Johansen Bivariate and multivariate co-integration test to find out long run relationship among some OIC countries in 2007. Moreover by using this test in 2003 Balios and Xanthakis examined the linkage among some chosen developed countries financial market. Johansen suggest maximum likelihood procedure to find out the number of co-integrating vectors. This test also investigate bivariate co-integration and multivariate co-integration. By using this test joint market efficiency will be examined in this study. The starting point of Johansen's method of co-integration is the Vector Auto Regression (VAR) of order p given by

![jh 1](https://user-images.githubusercontent.com/47462688/81881175-5e185d00-9587-11ea-8631-be0c328631e2.JPG)

where zt= n×1 vector that are integrated of order one, commonly denoted as I(1) and
µt= n×1 vector innovation. 
The Vector Auto Regression (VAR) model which is equation (4.5) can be written as follows

![jh 2](https://user-images.githubusercontent.com/47462688/81881217-7daf8580-9587-11ea-807f-f7963599133f.JPG)

The co-efficient matrix Π from the equation (4.6) has reduced rank r which is less than n then existing n×r matrices denoted α and β each with rank r can be written as follows
	        
		Π= αβ' ---------------------------------------------(4.7)
		
where α=n×1, is a column vector which represents the pace of short-run correction to disequilibrium. In addition , β' = 1×n, is co-integrating row vector which stand for the long run co-efficient matrix. 

Johansen proposed two different maximum likelihood ratio test denoted as follows

![jh 3](https://user-images.githubusercontent.com/47462688/81881285-b0f21480-9587-11ea-96d0-7930599d990d.JPG)

The trace statistics tests the null hypothesis of r co-integrating vectors where the alternative hypothesis is n co-integrating vectors. 

![jh 4](https://user-images.githubusercontent.com/47462688/81881328-d3842d80-9587-11ea-9947-cfa6af24ac9b.JPG)

On the other hand the Maximum Eigen Value test λmax tests the null hypothesis of r co-integrating vectors and the alternative hypothesis of (r+1) co-integrating vectors. To apply Johansen methods it is required to select the lag length from the VAR and it is selected from Schwarz Info Criterion (SIC). (Gilmore and McManus, 2003).

From the equation (4.8) and (4.9) T represents sample size and λ ̂(_j^ ) represents estimated values of characteristic root ranked from largest to smallest.    

*We can now perform the Johansen test on the  logarith daily price series and output the summary of the test: With trace test type*

```{r}
jotest.price.trace<-ca.jo(price[,c("STOXX50","DAX30","CAC40","Russell1000")], type=c("trace"), K=2, ecdet=c("none"), spec=c("longrun"),season = NULL, dumvar = NULL)
summary(jotest.price.trace)


###################### 
# Johansen-Procedure # 
###################### 

Test type: trace statistic , with linear trend 

Eigenvalues (lambda):
[1] 0.006652944 0.004878674 0.002355921 0.001285875

Values of teststatistic and critical values of test:

          test 10pct  5pct  1pct
r <= 3 |  3.47  6.50  8.18 11.65
r <= 2 |  9.83 15.66 17.95 23.52
r <= 1 | 23.02 28.71 31.52 37.22
r = 0  | 41.02 45.23 48.28 55.43

Eigenvectors, normalised to first column:
(These are the cointegration relations)

               STOXX50.l2   DAX30.l2    CAC40.l2 Russell1000.l2
STOXX50.l2      1.0000000  1.0000000  1.00000000       1.000000
DAX30.l2       -0.6992685 -0.5909108  0.09304002      -4.919011
CAC40.l2       -0.3361010 -1.2006345 -1.08319298       2.394870
Russell1000.l2  0.3459827  0.7165897  0.14706331       4.337408

Weights W:
(This is the loading matrix)

                STOXX50.l2     DAX30.l2      CAC40.l2 Russell1000.l2
STOXX50.d     -0.013676027  0.013077195 -0.0028363648  -0.0003794127
DAX30.d       -0.006350745  0.013369343 -0.0040456884  -0.0004371413
CAC40.d       -0.013452692  0.011928001  0.0001995051  -0.0004257001
Russell1000.d -0.009616529 -0.001486424 -0.0055048756  -0.0004753761
```

*We can now perform the Johansen test on the  logarith daily price series and output the summary of the test:  test typ eigene*

```{r}
jotest.price.eigen<-ca.jo(price[,c("STOXX50","DAX30","CAC40","Russell1000")], type=c("eigen"), K=2, ecdet=c("none"), spec=c("longrun"),season = NULL, dumvar = NULL)
summary(jotest.price.eigen)

###################### 
# Johansen-Procedure # 
###################### 

Test type: maximal eigenvalue statistic (lambda max) , with linear trend 

Eigenvalues (lambda):
[1] 0.006652944 0.004878674 0.002355921 0.001285875

Values of teststatistic and critical values of test:

          test 10pct  5pct  1pct
r <= 3 |  3.47  6.50  8.18 11.65
r <= 2 |  6.36 12.91 14.90 19.19
r <= 1 | 13.19 18.90 21.07 25.75
r = 0  | 18.00 24.78 27.14 32.14

Eigenvectors, normalised to first column:
(These are the cointegration relations)

               STOXX50.l2   DAX30.l2    CAC40.l2 Russell1000.l2
STOXX50.l2      1.0000000  1.0000000  1.00000000       1.000000
DAX30.l2       -0.6992685 -0.5909108  0.09304002      -4.919011
CAC40.l2       -0.3361010 -1.2006345 -1.08319298       2.394870
Russell1000.l2  0.3459827  0.7165897  0.14706331       4.337408

Weights W:
(This is the loading matrix)

                STOXX50.l2     DAX30.l2      CAC40.l2 Russell1000.l2
STOXX50.d     -0.013676027  0.013077195 -0.0028363648  -0.0003794127
DAX30.d       -0.006350745  0.013369343 -0.0040456884  -0.0004371413
CAC40.d       -0.013452692  0.011928001  0.0001995051  -0.0004257001
Russell1000.d -0.009616529 -0.001486424 -0.0055048756  -0.0004753761
```


### Gregory-Hansen Test


### Bai Perron Test


### Granger (1969) Causality Test

If any test result gives co-integration of selected markets, then it indicates that there exists a long run relationship among these markets. The long run relationship indicates that there may be causality exists among the selected markets. To investigate the causality among the markets, this study used the Granger (1969) Causality test. if there are two different stock markets indices namely xt and yt then the Granger Causality test can be written as follows

![gc 1](https://user-images.githubusercontent.com/47462688/81881480-2bbb2f80-9588-11ea-8791-252da414268d.JPG)

where, Δ represents first difference operator and ε1t  and ε2t  represents the two disturbance terms of two stock markets indices. n represents the number of lag lengths. In this test the X2 (= F-test) is used to conduct the test. If the test gives the result of rejection of null hypothesis then it can be conclude that the two stock market indices xt and yt are co-integrated. In addition with it can also be said that either xt granger cause yt or yt granger cause xt (Roca,1999).


### Runs and variance Ratio test

This study also investigates the weak form market efficiency of selected stock markets. To investigate the individual weak form efficiency of UK stock market and other selected stock market stock indices two more test will used called Runs test and Variance Ratio test. Run test is used to check whether the stock market return is random or not. The Run test is a nonparametric test. The main idea of Run test is if a time series data is random then actual number of runs should be near to expected number of runs . According to Run test the expected number of runs is calculated as follows

![run 1](https://user-images.githubusercontent.com/47462688/81881894-404bf780-9589-11ea-9a1d-160e81fd7c2a.JPG)
	
where i= 1,2, 3,4----------------n

If the number of observations is larger than 30 then the expected numbers of runs are about normally distributed with σm standard deviation. The σm is calculated as follows

![r 2](https://user-images.githubusercontent.com/47462688/81882045-a46ebb80-9589-11ea-9c15-fa8ad9ea519d.JPG)

It is possible to convert the runs into Z- statistics. If the calculated Z- statistics is greater than 1.96 then it rejects the null hypothesis at 5% significance level. Then the equation can be written as 

![run 3](https://user-images.githubusercontent.com/47462688/81882061-ae90ba00-9589-11ea-93cf-7043f9249f06.JPG)

On the other hand, the variance ratio also used in this study. In 1988 Lo and Mackinlay proposed this test. This test states that "whether the increment of all three random walk hypothesis are linear function of time interval" (Lo and McKinlay, 1988). The variance ratio test is written as follows

![v1](https://user-images.githubusercontent.com/47462688/81882337-6aea8000-958a-11ea-9185-1a4e33a63c64.JPG)

where σ2(q) represents unbiased estimator of 1/q of the variance of q- period returns and
σ2(1) represents unbiased estimator of 1/q of the variance of 1- period returns. There are two statistics in VR test and these are Z(q) and Z*(q). The null hypothesis of Z(q) is homoskedasticity and the null hypothesis of Z*(q) is heteroskedasticity. Now it can be rewrite as

![v 2](https://user-images.githubusercontent.com/47462688/81882407-98cfc480-958a-11ea-803f-4e8f5cb55e3b.JPG)

here if the calculated Z-statistics is greater than 1.96 then it rejects the null hypothesis.  at 5% significance level. 




### Reference

will follow later
