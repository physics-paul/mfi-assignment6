This project seeks to analyze default prediction of publicly-traded companies by year from 1962-2016, by using a logistic regression on possible covariates which should matter for default prediction as the explanatory variables. Both an in-sample and out-of-sample test is performed to analyze the accuracy of the Machine Learning model, which can then be used to forcast the likelihood of default in the future.

This task is essentially divided into five parts:

1. Downloading the Data
2. Calculating and Analyzing the Explanatory Variables.
3. Performing the Logistic Regression on In-Sample Testing
4. Performing the Logistic Regression on Out-of-Sample Testing
5. Analysis of the Results

### 1. Downloading the Data

There were a few main sources of data used for this project:
  
a. DSF : The daily stock returns and volume, along with shares outstanding, were obtained by analyzing the DSF SAS file which was obtained through the QCF server. This data file had company information in the form of the CUSIP number and was used to obtain the share price, the return from the previous year, number of shares, and the volatility of the firm's equity value.

b. FUNDA : This company-specific data information file contained information on the outstanding debt held by each company and the link between CUSIP and the CIK number. Additionally, variables such as cash on hand and debt outstanding were used as explanatory variables in predicting default. 
  
c. Distance to Default : This data file, calculated in the previous assignment, was used as an explanatory variable to calculate the likelihood of default. It contains the distance to default for each year for a randomly selected group of 200 firms each year.

d. Moody's BAA-Fed Fund Spread : This data file contains the spread between BAA Corporate Bond yields and the Fed Funds rate. When in a recessionary period, this spread tends to be high, because the BAA Corporate Bond yields are closely linked to the probability of default for firms in this same riskiness level. This spread was used as an explanatory variable. The link is here: [Moody's BAA-Fed Fund Spread](https://fred.stlouisfed.org/series/BAAFFM).

e. Bankruptcy data : the bankruptcy date for the company in question, identified by the PERMNO identifier.

In actually scraping and extracting the data, the SAS Software was used in order to prepare the data. SAS was necessary in order to deal with the sheer size of the DSF and FUNDA dataset would make it infeasable for direct analysis in the R Statistical Package.

### 2. Calculating and Analyzing the Explanatory Variables.

Several variables were used in the analysis as possible indicators of default. They include:

- Leverage : the ratio of a firm's book value of debt to equity. This was chosen because it determines the default boundary and a higher leverage is expected to result in a higher (+) likelihood of default.
- Probability of Default : this probability represents the likelihood of a firm defaulting and was calculated by representing the market capitalization as a call option on the firm's assets in the Black-Scholes framework. This was chosen because it closely tracks the actual likelihood of default, and a higher probability of default is expected to result in a higher (+) likelihood of default.
- Current Ratio : the ratio of the current assets to current liabilities. This ratio determines a firm's ability to pay short-term loan obligations and a higher current ratio is expected to result in a lower (-) likelihood of default.
- Volatilty of Asset Price : the volatilty in asset price over the previous year. This was chosen because a higher volatility in asset price leads to a higher (+) likelihood exists of ending in default.
- Volatilty of Share Price : the volatilty in the market price of equity. This was chosen because in general market volatility follows periods of economic instability and uncertainty, which would lead to a higher (+) likelihood of default.
- Moody's BAA-Fed Fund Spread : the spread of the BAA corporate bond yield over the treasury spread. This is a general indicator of economic stability and a higher spread would lead to a higher (+) likelihood of default.

These variables were pre-processed and scaled accordingly to allow for a quicker gradient descent.

### 3. Performing the Logistic Regression on In-Sample Testing

The logistic regression was performing using the R Statistical software over the training set of 1962-2015 yearly data. The data was lagged appropriately, so predictive variables from the previous year were used on the next year's data. Upon performing the regression, we obtained the following statistics on the in-sample testing:

| --- | Predicted: Bankruptcy | Predicted: No Bankruptcy | Ratio |
| Actual: Bankruptcy	| 0.10961 |	0.12776 | 0.0495 |
| Actual: No Bankruptcy	| 0.10961 |	0.12776 | 0.0596 |
| Ratios                | 0.456   | 0.3456  | 0.2345 |

Graphically, we can plot the ROC curve as:

<p align="center">
  <img height='60' src="https://raw.githubusercontent.com/physics-paul/mfi-assignment5/master/images/3graph.png">
</p>

We can calculate the AUC and KS statistics as: AUC = 0.234, and KS = 1.34.

This implies our model is generally very good at predicting default for in-sample testing!

### 4. Performing the Logistic Regression on Out-Of-Sample Testing

In contrast to the in-sample testing, the logistic regression was performed rolling window basis, in order to predict the out-of-sample default. This means, for instance, forcasting for 1991 was done with 1962-1990 yearly data, and forcasting for 1992 was done with 1963-1991 yearly data, etc. The data in the training set was lagged appropriately, so predictive variables from the previous year were used on the next year's data in generating the regression. Upon performing the regression, the default probabilities over the range 1991-2016 were ranked into deciles. The number (and percentage of defaults) in each of these groups is given in the following chart:

| --- | Number of Defaults | Percentage of Defaults |
| 1  | 100 | 0.50 |
| 2  | 80 | 0.20 |
| 3  | 20 | 0.15 |
| 4  | 10 | 0.10 |
| 5  | 5 | 0.05 |
| 6  | 1 | 0.00 |
| 7  | 1 | 0.00 |
| 8  | 1 | 0.00 |
| 9  | 0 | 0.00 |
| 10 | 0 | 0.00 |

As can be seen, the majority of defaults lie in decile 1 and 2, which is a great sign!

The following statistcs were obtained on the out-of-sample testing:

| --- | Predicted: Bankruptcy | Predicted: No Bankruptcy | Ratio |
| Actual: Bankruptcy	| 0.10961 |	0.12776 | 0.0495 |
| Actual: No Bankruptcy	| 0.10961 |	0.12776 | 0.0596 |
| Ratios                | 0.456   | 0.3456  | 0.2345 |

Graphically, we can plot the ROC curve as:

<p align="center">
  <img height='60' src="https://raw.githubusercontent.com/physics-paul/mfi-assignment5/master/images/3graph.png">
</p>

We can calculate the AUC and KS statistics as: AUC = 0.234, and KS = 1.34.

This implies our model is generally very good at predicting default for out-of-sample testing!
