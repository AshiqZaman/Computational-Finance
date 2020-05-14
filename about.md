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


### Gregory-Hansen Test


### Bai Perron Test


### Granger (1969) Causality Test

If any test result gives co-integration of selected markets, then it indicates that there exists a long run relationship among these markets. The long run relationship indicates that there may be causality exists among the selected markets. To investigate the causality among the markets, this study used the Granger (1969) Causality test. if there are two different stock markets indices namely xt and yt then the Granger Causality test can be written as follows

![gc 1](https://user-images.githubusercontent.com/47462688/81881480-2bbb2f80-9588-11ea-8791-252da414268d.JPG)

where, Δ represents first difference operator and ε1t  and ε2t  represents the two disturbance terms of two stock markets indices. n represents the number of lag lengths. In this test the X2 (= F-test) is used to conduct the test. If the test gives the result of rejection of null hypothesis then it can be conclude that the two stock market indices xt and yt are co-integrated. In addition with it can also be said that either xt granger cause yt or yt granger cause xt (Roca,1999).



