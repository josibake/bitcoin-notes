# Core fee estimation is not great

- Core fee estimate over pays the needed amount [and is not used!](https://hackmd.io/@kEyqkad6QderjWKtcBF5Hg/cChallengies-with-estimating-transaction-fees)
- Something as simple as looking at the past average on the 0.25 cut often works better

## Aiming for the tx to appear in the next 2 blocks:

 
  ![image](https://github.com/ClaraShk/BData/assets/33697523/4f55fa0a-0656-4f64-a836-f1f6f870987f)


## Abubakar's mempool fee estimation

 ![image](https://github.com/ClaraShk/BData/assets/33697523/8b86940a-fd2b-41da-896c-405a6ce50b51)

## Other targets

  ![image](https://github.com/ClaraShk/BData/assets/33697523/d53bc3f7-f973-4692-b311-e489a8e2067f)

















# Our approach
## Goal
**input $N, tx$:** include $tx$ ASAP / in $N$ blocks / in $T$ minutes

**output $rec$:** fee recommendation that puts $tx$ in the lower feerate quantiles of transactions that are included by the deadline

- We are trying to predict the 0.25 quantile in the target.  
  - A balance between underpaying and overpaying.
- Keep things as simple as possible
  - Explainable models
  - Easy to change if circumstances change
- Consider adversarial actors
### First step - get txs in ASAP

![image](https://github.com/ClaraShk/BData/assets/33697523/eedd7dcc-acab-4a6a-b3b5-7a778d8e5da2)


## Other possible goals
- Addition of confidence parameter (95% vs 75%)
- RBF/CPFP strategy
- Max fee (with feedback)

# Method
## Data to be used
### Yes
- Previous $K$ blocks
  - Content
  - Time between blocks
- Mempool
  - Content (block template)
  - Size
### No
- Seasonality (time of the day, day of the week)
### Maybe
- Only consider blockchain $\cap$ mempool
- Time txs spent in the mempool (~current fee estimation)

## Models
### ARIMA(X)
**A**uto**R**egressive **I**ntegrated **M**oving **A**verage (with e**X**ogenous data)
#### AR
$X_t =  \phi_1 X_{t-1} +  \phi_2 X_{t-2} +...+  \phi_k X_{t-k} + \epsilon_t$

#### MA
$X_t = \mu + \theta_1 \epsilon_{t-1} +   \theta_2 \epsilon_{t-2} +...+  \theta_k \epsilon_{t-k} + \epsilon_t$

#### I
Make the series stationary by taking derivatives: $X_{t}^{'} = X_t - X_{t-1}$ 

#### (X)
Addition of exogenous data (time of previous block, mempool size, block template)

## Other options
Rather not, but will use if much better
- Random forest
- Gradient Boosting
- ETS (Error, Trend, Seasonality), maybe for further targets ~days and not ~blocks

# How do we judge models?
- Compare their performance on historical data
  - Choose different times (jumps, ordinals, high and low tx flow)
  - Mindful of variance
- Estimate possible manipulations (attackathon!)
    

# Adversarial players
- Miners pushing the fee estimation up?
- Push the estimation down to get in cheaper?


# Estimation for the distant future

- Maybe don't ignore seasonality?
- Can it work without RBF?

![image](https://github.com/ClaraShk/BData/assets/33697523/dd7b0707-2035-43cc-babd-cf06c96cd145)


# Bonus graphs

  
  ![image](https://github.com/ClaraShk/BData/assets/33697523/d8fc63c0-44ad-421b-85de-0e9c8d2085a9)

  ![image](https://github.com/ClaraShk/BData/assets/33697523/913a65a1-837b-4937-b939-ec2f8e06008c)



 

  ![image](https://github.com/ClaraShk/BData/assets/33697523/22f0d5ea-1571-4703-88c1-d7317f0584ee)
  ![image](https://github.com/ClaraShk/BData/assets/33697523/dde52911-1197-4c44-b81a-6f00ab6ace33)
  ![image](https://github.com/ClaraShk/BData/assets/33697523/97cf34a0-1a1e-4801-b059-47d54eb742e5)
  ![image](https://github.com/ClaraShk/BData/assets/33697523/81d4c7a6-e350-48a3-b370-3e7030709d57)
  ![image](https://github.com/ClaraShk/BData/assets/33697523/5e2d99ed-dfc3-429f-a4f5-f5001f0a2881)

 ![image](https://github.com/ClaraShk/BData/assets/33697523/0a442498-10a2-4af3-8827-2df29099f4dd)
  ![image](https://github.com/ClaraShk/BData/assets/33697523/47e347c0-6420-4b1d-9942-d3ca0473ded3)

![image](https://github.com/ClaraShk/BData/assets/33697523/aac45c41-b7e0-4b94-9a07-28eda4a53e60)
  ![image](https://github.com/ClaraShk/BData/assets/33697523/2c3897bf-dbee-4db7-a292-882b9f1ed60a)
  ![image](https://github.com/ClaraShk/BData/assets/33697523/22de4940-0607-470c-845c-e55729219898)

 ![image](https://github.com/ClaraShk/BData/assets/33697523/8992b070-771e-4bc3-8c80-4101c0df16e2)
 ![image](https://github.com/ClaraShk/BData/assets/33697523/55fa7258-2378-4a43-a227-a362b3ef6a10)


 # Other thoughts and challenges
- Add "confidence" to aim for 0.75 for example
- What happens if many txs skip the mempool?
- Will the model be good in the future?

## Things to keep in mind
- Next block vs. next 10 min
- Should look at both mempool and blocks (especially if they strongly disagree)
  - 50% of block is sold of market
  - React quickly to changes
  - For now, ignore weekly seasonality and such


   ![image](https://github.com/ClaraShk/BData/assets/33697523/e67bbef7-8394-4e6d-957e-24591bb9759f)




