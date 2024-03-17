# Sigma-Internship-Coding-Challenge

### PortfolioOptimizer README

The `PortfolioOptimizer` class is designed to optimize a trading portfolio based on historical stock price data. It utilizes the QuantRocket API for fetching historical price data and Pandas/Numpy for data manipulation and analysis. The class aims to demonstrate a basic portfolio optimization strategy based on predefined market states.

#### Class Initialization

__init__(ticker, start_date, end_date)

- **ticker**: Stock ticker symbol (e.g., 'FIBBG000B9XRY4').
- **start_date**: Start date for historical data (e.g., '2023-01-01').
- **end_date**: End date for historical data (e.g., '2023-12-31').

This method initializes the `PortfolioOptimizer` class with the specified stock ticker and date range.

- Data Download `download_data()`: Fetches historical stock prices from QuantRocket.
- Returns Calculation `calculate_returns()`: Calculates daily returns based on closing prices.
- State Calculation `calculate_states()`: Classifies market states as Bull, Flat, or Bear.
- Portfolio Optimization `optimize_portfolio()`: Implements a basic portfolio optimization strategy.
- Results Printing `print_results()`: Prints portfolio value, optimal buy indices, and transition distribution.

#### Example usage of the model:

```python
if __name__ == "__main__":
    portfolio_optimizer = PortfolioOptimizer('FIBBG000B9XRY4', '2023-01-01', '2023-12-31')
    portfolio_optimizer.download_data()
    portfolio_optimizer.calculate_returns()
    portfolio_optimizer.calculate_states()
    portfolio_optimizer.optimize_portfolio()
    portfolio_optimizer.print_results()
```

### Output:

```python
    Portfolio Value: 17
    Optimal Index: [7, 11, 15, 20, 27, 29, 40, 49, 51, 58, 60, 68, 78, 84, 87, 93, 99, 102, 107, 109, 112, 116, 119, 122, 132, 141, 159, 163, 176, 186, 190, 206, 208, 211, 215, 217, 231, 233, 237]
    Length: 39
    Transition Distribution:
              Bear      Flat      Bull
    Bear  0.138889  0.722222  0.138889
    Flat  0.146497  0.598726  0.254777
    Bull  0.125000  0.678571  0.196429
```
### Algorithm to find the Maximal optimal index:

To find the most optimal [index, portfolio] values using dynamic programming, you can follow these steps:

Define the problem:
- Define a function OptimalPortfolio(i), which represents the maximum portfolio value achievable up to index i.
- Goal: Maximize the portfolio value, considering both increasing and decreasing sequences.

Initialize:
- Initialize an array DP of size equal to the number of elements in the input list.
- Set DP[0] to the portfolio value at index 0.

Recurrence relation:
- For each index i from 1 to n-1 (where n is the number of elements in the input list):
  - Iterate over all indices j < i.
  - If the portfolio value at index i is greater than or equal to the portfolio value at index j, update DP[i] to the maximum of DP[i] and DP[j] + portfolio value at index i.

Trace back the optimal solution:
- After computing DP for all indices:
  - Trace back the optimal solution by starting from the index with the highest DP value.
  - Keep track of the indices in the optimal solution.

