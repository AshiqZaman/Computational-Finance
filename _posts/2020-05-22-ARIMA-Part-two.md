---
layout: post
title: "ARIMA Model for Stock Forecasting"
tagline: "ARIMA (Autoregressive Integrated Moving Average) for Stock forecasting with R"
image: /thumbnail-mobile.png
author: "Ashiq Zaman"
meta: "Springfield"
---

ARIMA (autoregressive integrated moving average) is one of the most common statistical technique which used to fit time series data and forecasting. ARIMA is a generalized version of ARMA (autoregressive moving average) process, where ARMA process is applied to differenced version of the data instead the original data. The ARIMA model use p, d and q to specify order, where p represents order of AR, d represents the order of difference and MA part represents the MA part. 

### Auto Regression (AR) and Moving Average (MA) process

**Auto Regression (AR)**

AR is a class of liner model where the variable of interest is regressed on its own lagged values. So, AR can be written as follows: 

<a href="https://www.codecogs.com/eqnedit.php?latex=y_{t}=\delta&space;&plus;\phi&space;_{1}y_{t-1}&plus;\phi&space;_{2}y_{t-2}&plus;--------&plus;\phi&space;_{p}y_{t-p}&plus;\epsilon&space;_{t}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?y_{t}=\delta&space;&plus;\phi&space;_{1}y_{t-1}&plus;\phi&space;_{2}y_{t-2}&plus;--------&plus;\phi&space;_{p}y_{t-p}&plus;\epsilon&space;_{t}" title="y_{t}=\delta +\phi _{1}y_{t-1}+\phi _{2}y_{t-2}+--------+\phi _{p}y_{t-p}+\epsilon _{t}" /></a>

**Moving Average (MA)**
