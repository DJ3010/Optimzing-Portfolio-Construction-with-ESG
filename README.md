# Optimzing-Portfolio-Construction-with-ESG

Analyzed the effect of ESG Risk score by RepRisk on Portfolio Optimization for the period of January 2016 to December 2020 with stocks in S&P 100. Optimized the stock weights using two methods: 
1) Maximize returns using the risk aversion parameter and
2) Minimize risk such that the portfolio tracks the benchmark (beta=1 with S&P 100). 

### Data
The data used here ranges from January 2015 to December 2020 for all companies in the S&P 100 point in time universe. It is important to note that the point in time universe is considered to remove the survivorship bias. So at any time instance, we trade on only those companies that are present in the universe at that time instance. Constituents of S&P 100 are considered to be rebalanced monthly. <br>
Apart from the point in time universe, the data includes three main components:
1. Daily price data for all companies in the universe and the S&P 100 (OEX) index
2. Monthly ESG data for all companies in the universe
3. Monthly factor data for all companies in the universe

The ESG data is from Wharton Research Data Services. The data vendor used is RepRisk, which gives the ESG Risk score denoting the current level of reputational risk exposure of a company related to ESG issues. Its values are between 0 to 100. the lower the ESG Risk score, the better the company is from an ESG point of view.

<img width="940" alt="Screenshot 2024-06-27 at 4 00 59 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/ed778068-b9d0-4bfb-943e-cd496b9d09f4">

### ESG Strategies
For building strategies focused on ESG, it is important to consider how to incorporate ESG in the process to construct a robust and adaptable portfolio. For this, different levels of importance are placed on ESG Risk score in the portfolio construction process. 

<img width="396" alt="Screenshot 2024-06-27 at 4 17 25 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/9b2c0690-bd96-4bc2-96ab-ba0b155169d5">

### Portfolio Optimization Analysis
Past one year daily returns data of the companies in the current point in time universe is used to calculate the mean and covariance matrix for optimization. For ESG parameters in the optimization, ESG Risk score from the past month is used. The portfolios are rebalanced monthly and out of sample returns and ESG scores are calculated to compare all the strategies. <br>

Two optimization methods are considered: 
1) Classical Markowitz and
2) Index Tracking Markowitz.

To test and compare the performance of all these strategies, following performance metrics are used,
<img width="686" alt="Screenshot 2024-06-27 at 4 21 25 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/ea2f27e7-fa02-4be3-b1ac-b3160c7d9c5c">






