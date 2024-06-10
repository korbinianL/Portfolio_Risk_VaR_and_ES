# Portfolio_Risk_VaR_and_ES

### Intro
This analysis focuses on the risk assessment of a diversified stock portfolio using two prominent risk metrics: Value at Risk (VaR) and Expected Shortfall (ES). VaR is a widely used risk measure that estimates the maximum potential loss of a portfolio over a specified time period within a given confidence level. While VaR provides a clear threshold for potential losses, it does not capture the extent of losses beyond this threshold. To address this limitation, Expected Shortfall (ES), also known as Conditional VaR, is employed. ES provides an average of the potential losses exceeding the VaR, offering a more comprehensive view of tail risk.

### Portfolio Creation
I arbitrarily selected 11 stocks, one from each of the 11 GICS sectors outlined by [MSCI](https://www.msci.com/our-solutions/indexes/gics) and got daily adjusted close prices for the last 10 years from 01/04/2014 to 28/04/2024. Then I calculated the efficient frontier for these stocks and used the weights of the optimal portfolio to get daily returns for the portfolio. Two assumptions I made are that short selling is allowed, hence negative weights and for simplicity the risk-free rate is 0. The images show the efficient frontier, portfolio returns, the cumulative returns of each stock and the portfolio and how the returns are distributed respectively.


| Stock  | Weight |
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

<p align="center">
  <img src="images/ef.png" alt="Efficient Frontier" width="400"/>
  <img src="images/p_rets.png" alt="Portfolio Returns" width="400"/>
  <img src="images/cum_rets.png" alt="Cumulitative Returns" width="400"/>
  <img src="images/dist.png" alt="Portfolio Distribution" width="400"/>
</p>

### Risk Calculations

These methods were used to calculate 1% 1-day VaR for a $1,000 portfolio value.

- Historic Simulation
- VaR assuming normal distribution
- VaR assuming t-distribution
- Age Weighted VaR
- Modified VaR

Results can be interpreted as: Due to market movements, we are 99% certain losses will not exceed {Insert VaR Value} in one day.

|            | **Historic Simulation** | **Normal Dist.** | **T-Dist.** | **Age Weighted** | **Modified** |
|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|
| **1% 1-Day VaR** | $35.54  | $31.35  | $36.55  | $25.52  | $78.68  |

For the expected shortfall I only looked at 3 methods:

- Historic Simulation
- VaR assuming normal distribution
- VaR assuming t-distribution

These results can be interpreted as: If the loss exceeds the {Insert VaR Value} threshold, the average loss will be {Insert ES Value}.

|            | **Historic Simulation** | **Normal Dist.** | **T-Dist.** |
|:------------:|:------------:|:------------:|:------------:|
| **ES** | $48.41  | $35.92  | $116.26  |

### Testing effectiveness of VaR model

To test the performance of the different VaR models I used a rolling window approach with an initial window of 500 observations and here the portfolio value is $1. Four tests were used to measure and compare the VaR models:

- Violation ratio: Measures the number of times losses were greater than VaR. A good model will have a ratio close to one. For a 1% 1-day VaR with 500 observations you'd expect losses to be greater than the VaR on 5 occasions.
- [Unconditional coverage test](https://www.scribd.com/document/305462693/Kupiec-Paul-Techniques-for-Verifying-the-Accuracy-of-Risk-Measurement-Models): ensures that the theoretical confidence level p matches the empirical probability of violation. Similar to the violation ratio, but we draw our conclusion from a p-value.
- [Independence test](https://www.jstor.org/stable/2527341): requiring any two observations in the hit sequence to be independent of each other. Intuitively, the fact that a violation has been observed today should not convey any information about the likelihood of observing a violation tomorrow.
- [Quadratic probability score (QPS)](https://elsevier-ssrn-document-store-prod.s3.amazonaws.com/06/11/08/ssrn_id943498_code658905.pdf?response-content-disposition=inline&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJGMEQCIBAUGZwXuhHyjclplqGxg32ACZoNV1Ct%2FbXbhMuUxVO%2FAiAGMZdo4oswn9ruWGC8CHUMaHte0og%2FEEiDG1jhGIB4ICq9BQhKEAQaDDMwODQ3NTMwMTI1NyIMmSw15YYiDx8Th1WwKpoFMXrO0ox0%2F6LxQeI20mr0d8hVMnLUKoM9FGNo9aYG%2F4XwZedM%2Bbol42WurvcQ0qVuEuamCFw9XZV5CIgDq7Pf0YSfJleUfQGeJMiHtjtxaX6CE0Vd2xg9YbHkMnVr7f6tnlJ6eCmW82DUhr1EQHLrLuxgsw%2Fh8NGjL%2FIyx9FtQbvSZgYx8vbnGsL1%2FBrpnR6M91x%2BFBrGRsMK2EzVbAWw7uginHK4PkZ%2F1SUoftyJ99ltjqX%2B9UT0APy%2BrPftxWfb5oNbbBxocmMDwCxQebof5lowaXKkW4cowifGat05gFJL5OXlkzh%2B502gYSd%2BK0MGAJv%2FkOY%2BxpiSVImRf%2F%2FJ3v%2F6Ljdx5ZHYpCFEkd4UZQVRxUd8k1kmDlfQr4cOESG%2Bnvt0DG2BN%2Bmk774O2e3uNEuUeTYQZPkDr9NMPV0M1ROdA5S5BXsDu9IhHxMlCTA97P0nrDLiTYK%2BpPZOnv90dGnmSs6GfV%2B29gTw7ro%2FHu1IC4vPHY7Vk%2Bfez4WL1AP0KNNEARP1D0%2BnrufZDRPZyUg5UlzEGuCtxwBG5k2ayIHXptFCq%2FtE8asiwrwWAu192EbE%2BUKqhIqdhaN48qeDWif4KruDWie7VhUitLZVP8dOKmeO1Fy3YIHH2TFFbHpF7EQ0i58nyiiFpqzfNl6vBsUc%2BGZRZyL9kf9fmz2P3axeCSkXLYZiwZts3sy1xfCUXBBBise20d0K0WQIp89qeqiYMzpHVz454msKuDwjDhMgwbwNfhz5SS8sn2BLFTHPqugHAaj%2BtUvCTZACyDiW%2BEqh6DB0GTZW4PxtH71Z9KyNiqS1adPf%2FnLvRW6pD6ylA2CrJfE9HESU9BD%2Fzbi6ZGeJ%2FTLDARI%2B6M4QH%2F0B5bvxc02MHU5DbnE9MObVnLMGOrIB0BLNiFc5XHBOu9AShDLXnBwFp63ULUcW5u8jUPTYdarogbc9qL7AAR6xRmU2EC9ZppsTPDiJAq5nUm0k6GbKMY7QqmjpPU2prJnRTpiQz48xgZ79aBAUl8Auooy5jDc7vT7MZlraDzbkLEwKepYI9hD0Azn50dujkPJXRyCOzHzqREIYP0qctQBs7%2BmVxDuxw%2BnXIrSwjDPQp18VhKdWJeLwctfZuz31KyztZVpbhEQjzQ%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240610T175606Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAUPUUPRWEYTUASX7I%2F20240610%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=d499813a2905c220709fbd2e73c1a15fe61641ad048e9fe70fd5b466aad2e740): The QPS takes values in the range of [0, 2], and models with lower QPS scores (closer to zero) are considered to be better performing.

|            | **Historic Simulation** | **Normal Dist.** | **T-Dist.** | **Age Weighted** | **Modified** |
|:------------:|:------------:|:------------:|:------------:|:------------:|:------------:|
| **1% 1-Day VaR** | $35.54  | $31.35  | $36.55  | $25.52  | $78.68  |
| **Violation Ratio** | 1.3889  | 1.9345  | 1.2897  | 1.1409  | 0.3472  |
| **Unconditional P-value** | 0.097*  | 0.000***  | 0.211  | 0.534  | 0.001***  |
| **Independance P-value** | 0.405  | 0.784  | 0.346  | 0.000***  | 0.000***  |
| **QPS** | 0.027  | 0.038  | 0.025  | 0.023  | 0.007  |

<p align="center">
  <img src="images/ef.png" alt="Efficient Frontier" width="400"/>
  <img src="images/p_rets.png" alt="Portfolio Returns" width="400"/>
  <img src="images/cum_rets.png" alt="Cumulitative Returns" width="400"/>
  <img src="images/dist.png" alt="Portfolio Distribution" width="400"/>
  <img src="images/dist.png" alt="Portfolio Distribution" width="400"/>
</p>
