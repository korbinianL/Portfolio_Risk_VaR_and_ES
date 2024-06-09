# Portfolio_Risk_VaR_and_ES

### Intro
This analysis focuses on the risk assessment of a diversified stock portfolio using two prominent risk metrics: Value at Risk (VaR) and Expected Shortfall (ES). VaR is a widely used risk measure that estimates the maximum potential loss of a portfolio over a specified time period within a given confidence level. While VaR provides a clear threshold for potential losses, it does not capture the extent of losses beyond this threshold. To address this limitation, Expected Shortfall (ES), also known as Conditional VaR, is employed. ES provides an average of the potential losses exceeding the VaR, offering a more comprehensive view of tail risk.

### Portfolio Creation
I arbitrarily selected 11 stocks, one from each of the 11 GICS sectors outlined by [MSCI](https://www.msci.com/our-solutions/indexes/gics), and got daily adjested close prices for the last 10 years from 01/04/2014 to 28/04/2024. Then I calculated the efficent frontier for these stocks and used the weights of the optimal portfolio to get daily returns for the portfolio. Two assumptions I made are that short selling is allowed, hence negative weights and for simplicity the risk free rate is 0. 

| Stock | Weight |
|----------|----------|
| BA  | -13.98%  |
| BHP  | -8.7%  |
| JNJ  | 12.79%  |
| JPM  | 46.04%  |
| KO  | -1.65%  |
| NEE  | 33.28%  |
| PLD  | 24.72%  |
| SAP  | 13.01%  |
| SHEL  | -5.75%  |
| T  | -26.22%  |
| TJX  | 26.48%  |
