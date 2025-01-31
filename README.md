# Optimzing-Portfolio-Construction-with-ESG

Analyzed the effect of ESG Risk score by RepRisk on Portfolio Optimization for the period of January 2016 to December 2020 with stocks in S&P 100. Optimized the stock weights using two methods: 
1) Maximize returns using the risk aversion parameter and
2) Minimize risk such that the portfolio tracks the benchmark (beta=1 with S&P 100). 

### DATA
The data used here ranges from January 2015 to December 2020 for all companies in the S&P 100 point in time universe. It is important to note that the point in time universe is considered to remove the survivorship bias. So at any time instance, we trade on only those companies that are present in the universe at that time instance. Constituents of S&P 100 are considered to be rebalanced monthly. <br>
Apart from the point in time universe, the data includes three main components:
1. Daily price data for all companies in the universe and the S&P 100 (OEX) index
2. Monthly ESG data for all companies in the universe
3. Monthly factor data for all companies in the universe

The ESG data is from Wharton Research Data Services. The data vendor used is RepRisk, which gives the ESG Risk score denoting the current level of reputational risk exposure of a company related to ESG issues. Its values are between 0 to 100. the lower the ESG Risk score, the better the company is from an ESG point of view.

<img width="940" alt="Screenshot 2024-06-27 at 4 00 59 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/ed778068-b9d0-4bfb-943e-cd496b9d09f4">

### ESG STRATEGIES
For building strategies focused on ESG, it is important to consider how to incorporate ESG in the process to construct a robust and adaptable portfolio. For this, different levels of importance are placed on ESG Risk score in the portfolio construction process. 

<img width="396" alt="Screenshot 2024-06-27 at 4 17 25 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/9b2c0690-bd96-4bc2-96ab-ba0b155169d5">

### PORTFOLIO OPTIMIZATION ANALYSIS
Past one year daily returns data of the companies in the current point in time universe is used to calculate the mean and covariance matrix for optimization. For ESG parameters in the optimization, ESG Risk score from the past month is used. The portfolios are rebalanced monthly and out of sample returns and ESG scores are calculated to compare all the strategies. <br>

Two optimization methods are considered: 
1) Classical Markowitz and
2) Index Tracking Markowitz.

To test and compare the performance of all these strategies, following performance metrics are used:
- **Return**: Compounded annual growth rate of daily returns
- **Volatility**: Annualized standard deviation from daily returns
- **Sharpe Ratio**: Annualized risk-adjusted returns
- **Tracking Error**: Annualized standard deviation of excess returns to benchmark
- **Information Ratio**: Annualized excess returns compared to volatility of excess returns
- **Max Drawdown**: Maximum decline from previous peak
- **Calmer Ratio**: Annualized returns given maximum drawdowns
- **Sortino Ratio**: Annualized downside risk adjusted returns
- **95% Var**: Maximum annual loss with 95% confidence
- **ESG Risk score**: Average ESG Risk score
- **Sharpe/ESG Risk Score**: Annualized Sharpe ratio for a unit ESG Risk score

### CLASSICAL MARKOWITZ OPTIMIZATION

Classical Markowitz portfolio optimization for long only portfolio solves the following optimization problem to allocate weights to companies within the portfolio, where γ is the risk aversion parameter and the optimal weights selected are corresponding to the γ that gives the highest Sharpe Ratio:

$\max \mu^Tw - \gamma w^T\Sigma w \quad   st. 1^Tw = 1, w > 0, w \in W  \qquad (1) \$

1. No ESG: <br>
   Use (1) on the entire point in time universe to find optimal weights.
2. Semi ESG Strategies:
   - Top 25:
     filter current point in time universe based on last month's ESG Risk score and run this optimization problem on the selected universe of the top 25 companies with the least ESG Risk score
   - Bottom 25:
     filter current point in time universe based on last month's ESG Risk score and run this optimization problem on the selected universe of the bottom 25 companies with most ESG Risk score
3. Full ESG Scaling:
   - Tilt Scale:
   Use (1) on the entire point in time universe to find optimal weights. Then scale the weights by multiplying them with the ESG Risk score and dividing by the sum of weights to make them sum up to 1.
   - Momentum Scale:
   Use (1) on the entire point in time universe to find optimal weights. Then scale the weights by multiplying them with the ESG Trend score and dividing by the sum of weights to make them sum up to 1.
4. Full ESG Constraint: <br>
   Use the below optimization on the entire point in time universe:
   
   $\max \mu^Tw - \gamma w^T\Sigma w \quad   st. 1^Tw = 1, w > 0, ESG Risk score < k.esg, w \in W \$ 

   _esg_ = ESG Risk score of the No ESG portfolio <br>
   _k_ = 0.95
5. Full ESG Objective: <br>
   Use the below optimization on the entire point in time universe:

   $\min ESG Risk score \quad st. 1^Tw = 1, w > 0, \mu ^ T w  > k_1.ret, w^T \Sigma w < k_2.var \$

   k_1 = 0.95 (i.e. allow 5% less returns of No ESG portfolio) <br>
   k_2 = 1.05 (i.e. allow 5% more risk of No ESG portfolio)

The performance of all the above-mentioned strategies is as shown:

<img width="1180" alt="Screenshot 2024-06-27 at 8 12 19 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/f12be3a8-42cf-46bf-9868-561302771b1d">

<br>

The cumulative returns of all strategies using Classical Markowitz optimization are as follows:

<img width="1347" alt="Screenshot 2024-06-27 at 7 12 16 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/d79bb5b2-6236-41e8-a4a0-c92b23a485da">

The ESG Risk scores of each strategy are:

<img width="1100" alt="Screenshot 2024-06-27 at 8 18 14 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/9bfbe475-838f-44fa-8937-6e59c121da38">

<br>
<br>

**Key Observations**:
- Volatility is almost constant for all strategies.
- Pre covid, all strategies underperform the index and a sudden boom is observed post covid.
- The Semi ESG Bottom strategy experiences the maximum boom and outperforms all others.
- The Semi ESG Top strategy has the lowest ESG Risk score (A grade).
- The Semi ESG Bottom strategy has the highest ESG Risk score (C grade).
- ESG Risk score is very volatile for all the strategies.

### INDEX TRACKING MARKOWITZ OPTIMIZATION 

Index tracking Markowitz portfolio optimization for long only portfolio solves the following optimization problem to allocate weights to companies within the portfolio, where β is the list of betas (the covariance of the return of a company with the return of the index divided by the variance of the return of the index over past one year) of all companies in the point in time universe with respect to the index S&P 100.

$\min w^T\Sigma w \quad st. 1^Tw = 1, \beta ^Tw = 1, w > 0, w \in W \qquad (2) \$

1. No ESG: <br>
   Use (2) on the entire point in time universe to find optimal weights.
2. Semi ESG Strategies:
   - Top 25:
     filter current point in time universe based on last month's ESG Risk score and run this optimization problem on the selected universe of the top 25 companies with the least ESG Risk score
   - Bottom 25:
     filter current point in time universe based on last month's ESG Risk score and run this optimization problem on the selected universe of the bottom 25 companies with most ESG Risk score
3. Full ESG Scaling:
   - Tilt Scale:
   Use (1) on the entire point in time universe to find optimal weights. Then scale the weights by multiplying them with the ESG Risk score and dividing by the sum of weights to make them sum up to 1.
   - Momentum Scale:
   Use (1) on the entire point in time universe to find optimal weights. Then scale the weights by multiplying them with the ESG Trend score and dividing by the sum of weights to make them sum up to 1.
4. Full ESG Constraint: <br>
   Use the below optimization on the entire point in time universe:
   
   $\max \mu^Tw - \gamma w^T\Sigma w \quad   st. 1^Tw = 1,\beta ^Tw = 1, w > 0, ESG Risk score < k.esg, w \in W \$ 

   _esg_ = ESG Risk score of the No ESG portfolio <br>
   _k_ = 0.95
5. Full ESG Objective: <br>
   Use the below optimization on the entire point in time universe:

   $\min ESG Risk score \quad st. 1^Tw = 1, \beta ^Tw = 1, w > 0, \mu ^ T w  > k_1.ret, w^T \Sigma w < k_2.var \$

   k_1 = 0.95 (i.e. allow 5% less returns of No ESG portfolio) <br>
   k_2 = 1.05 (i.e. allow 5% more risk of No ESG portfolio)

The performance of all the above-mentioned strategies is as shown:

<img width="1006" alt="Screenshot 2024-06-27 at 10 34 25 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/fb66c5e4-08ab-4ade-a953-eaa83db0696c">


The cumulative returns of all strategies using Classical Markowitz optimization are as follows:

<img width="943" alt="Screenshot 2024-06-27 at 10 39 36 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/db70d1e1-a293-4eb9-9b9c-3e695ca07ac0">

The ESG Risk scores of each strategy are:

<img width="1118" alt="Screenshot 2024-06-27 at 10 40 29 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/072f4429-097f-4432-8696-4ca6f9efa738">

<br>
<br>

**Key Observations**:
- Semi ESG Bottom strategy performs the best with the highest Sharpe Ratio but has the worst ESG Risk score.
- Semi ESG Top strategy underperforms the index but has the best ESG Risk score.
- All the ESG favoring strategies have lesser ESG risk scores than No ESG strategy.
- Unlike the Classical Markowitz optimization results, their ESG Risk scores are less volatile.

### RETURNS/RISK VS. ESG PERFORMANCE TRADEOFF
Conducted the analysis by taking the minimum returns needed as a certain percentage of No ESG portfolio return and by taking the maximum risk allowed as a certain percentage of No ESG portfolio risk. <br>
Performance tradeoff by varying minimum returns needed or maximum risk allowed in Full ESG Objective strategies (Obj1 and Obj2) is as follows:

<img width="1009" alt="Screenshot 2024-06-27 at 11 06 56 PM" src="https://github.com/DJ3010/Optimzing-Portfolio-Construction-with-ESG/assets/171126740/8eab0802-2eb8-48d7-967c-8c14c169e284">

<br>

**Key Observations**:
- Small Trade-Off in Returns for Significant ESG Improvement: Allowing for 5% less returns or 5% more risk results in a minimal drop in returns but a substantial decrease in the ESG Risk score, upgrading the portfolio from a B grade to almost an A grade.
- Balancing Performance and ESG Appeal: The enhancement in ESG rating makes the portfolio attractive to investors willing to sacrifice a small amount of performance for better ESG compliance.
- Optimizing Portfolios Based on Preferences: Investors can utilize this trade-off analysis to adjust their portfolios, balancing between their desired risk/return profile and ESG goals.


### CONCLUSION
This study presented several strategies of incorporating ESG considerations in the classical as well as index tracking Markowitz portfolio optimization process and compared performances. Analysis on tradeoff between better risk/return performance and better ESG performance is shown that can be useful to investors for choosing an ideal portfolio based on their risk/returns preferences. <br>
To sum it all up, ESG strategies do seem to have a bright future with investor interest in ESG based portfolios growing rapidly over time, especially after the importance of taking into account environmental, social and governance factors in investment decisions has become important with the covid crisis acting as a catalyst for development of ESG focused strategies. Over time companies will start taking ESG considerations more and more seriously, allowing investors to hold portfolios with purpose

### References
1. https://www.researchgate.net/publication/342132005_A_Framework_for_ESG_Portfolio_Construction
2. https://www.stern.nyu.edu/sites/default/files/assets/documents/MSCI_ESG_Research_Insight_Optimizing_ESG_Factors_in_Portfolio_Construction_February_2013.pdf
3. https://www.fool.com/investing/stock-market/market-sectors/
4. https://www.amundi.com/institutional/files/nuxeo/dl/c22a9bd5-ad8b-4a13-a932-1bdb524a238b
5. https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html




