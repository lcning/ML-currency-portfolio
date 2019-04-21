# ML-currency-portfolio
Using time series, boosted decision tree, SVM to forecast a foreign currency to USD price and to develop a portfolio strategy.

STEP 1:
Model Selection Method A (In-Sample):
1. At the time point t, calibrate/estimate the two models based on the past 24 monthly return observations (current observation at t included).
2. Calculate the AICs for the two estimated models for those 24 obsevations, and choose the one with lower AIC.
3. Forecast the next period return at time (t+1) using the selected model.

Model Selection Method B (Out-Sample):
1. At the time point t, for each model, out-of-samplely forecast the returns for the past 12 months.
2. for each time point from t-11 to t, say for t-k where k=0,…,11,
3. we'll estimate each model using the past 24 observations, i.e., observations at (t-k-24,…,t-k-1),
4. then based on the estimated model to forecast returns at t-k.
5. Now, for AR(1) model, we have forecasts for t-11, t-10,…,t, the MSE can be calculated as the mean of (forecast - true return)^2.
6. for MA(1) model, we have another vector of 12 forecasts, with corresponded 12 true returns, and we can calculate MSE.

STEP 2:
The model with lower MSE would be selected.
And then we need to calibrate that model using the past 24 observations.
Forecast the next period return at time (t+1).

STEP 3:
Now at time t, we have made our forecasts for the seven currencies of their monthly returns at t+1.
Suppose the forecasted returns are (r1,r2,…,r7). 
We'll transform the forecasts to return sign only: (sign(r1),sign(r2),…,sign(r7)). 
We'll invest in currency h with a long position if sign(rh)=1, otherwise we'll invest in it with a short position.
But right now, we need to figure out the weight vector based on Risk Parity.

STEP 4:
Prepare the covariance matrix corresponding to  (sign(r1),sign(r2),…,sign(r7)), i.e.,  
for currency h, if sign(rh)=1, we use the currency's original returns; otherwise, use the negative of original returns.
Then we'll calculate the covariance matrix based on past 60 monthly returns (time t included).

STEP 5:
the portfolio (w1*,…,w7*)'s return at time t+1 will be calculated as (sign(r1) x w1* x r1_new+…+ sign(r7) x w1* x r7_new)
where sign(rh) is your directional forecast for currency h at time t+1, and rh_new is its actual return at time t+1.

record the portfolio's return at time t+1, and go to Step 2 to make forecast for time t+2 and build the portfolio, and stop until 3/29/2018.
