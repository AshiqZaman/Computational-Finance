---
layout: page
title: Unit Root Tests
tagline: 
permalink: /now.html
ref: now
order: 2
---

### Dickey Fuller (DF) & Augmented Dickey Fuller (ADF)

The main condition for any time series regressions is the data must be stationary. Any non-stationary time series result gives a spurious result . So, for any time series analysis stationarity test is prerequisite. Unit Root test is one of the tests of stationarity which becomes popular over the last few years. There are some ways of doing Unit Root test and each way has its own advantages and disadvantages. In this study two tests is used to avoid each other disadvantages. The tests are conducted in log form and first difference log form of individual market indices. If the result gives the rejection of null hypothesis then it can be said that the selected time series data are stationary. The ADF test investigate the existence of unit root in an Autoregressive (AR) model. If any explanatory variable is the lagged values of dependant variable, then it is known as AR model. The most commonly used AR model is yt=β+ρyt-1+εt where yt represents variable of interest, t represents time index and ρ and ε are coefficient and disturbance terms accordingly. In this study the Augmented Dickey-Fuller (ADF) test is conducted based on the following regressions

![unit 1](https://user-images.githubusercontent.com/47462688/81876416-8baad980-957a-11ea-968f-25a06b981017.JPG)

where Δ represents the first difference operator, m represents the number of lags, εt represents pure white noise error term and Yt represents time series. Equation (4.1a) represents the pure random walk model without constant and trend and equation (4.1b) represents random walk with constant but without time trend and (4.1c) represents random walk with constant and time trend.

Before doing ADF test it assumes that the error terms of the model are statistically significant, and it has a constant variance. 


### KPSS


### Philipps-Perron

