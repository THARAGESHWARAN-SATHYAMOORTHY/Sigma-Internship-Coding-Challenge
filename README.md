# Sigma-Internship-Coding-Challenge

# MarketModel AAPL

### Overview

The `MarketModel` class is designed to perform market analysis and simulate a basic trading strategy based on historical stock price data. The `MarketModel` class utilizes the QuantRocket API for fetching historical price data and Pandas/Numpy for data manipulation and analysis. The primary goal is to demonstrate a simple trading strategy simulation based on predefined market states.

### Class Initialization

__init__(ticker, start_date, end_date)

- **ticker**: Stock ticker symbol (e.g., 'FIBBG000B9XRY4').
- **start_date**: Start date for historical data (e.g., '2023-01-01').
- **end_date**: End date for historical data (e.g., '2023-12-31').

This method initializes the `MarketModel` class with the specified stock ticker and date range. It also sets up essential variables for subsequent analysis.

- Data Fetching `fetch_data()` : Fetches historical stock prices from QuantRocket.
- Returns Calculation `calculate_returns()` : Calculates daily returns based on closing prices.
- State Classification `classify_states()` : Classifies market states as Bear, Flat, or Bull.
- Value Function Implementation `implement_value_function()` : Implements a basic trading strategy value function.
- Transition Distribution Calculation `calculate_transition_distribution()` : Calculates transition distributions between market states.
- Results Printing `print_results()` : Prints portfolio value, optimal buy indices, and transition distribution.

### Example usage of the model:

    model = MarketModel('FIBBG000B9XRY4', '2023-01-01', '2023-12-31')
    model.fetch_data()
    model.calculate_returns()
    model.classify_states()
    model.implement_value_function()
    model.calculate_transition_distribution()
    model.print_results()

### Output:

    Portfolio Value V(d): 17
    Optimal Index: [5, 7, 11, 15, 20, 27, 29, 40, 49, 51, 58, 60, 68, 78, 84, 87, 93, 99, 102, 107, 109, 112, 116, 119, 122, 132, 141, 159, 163, 176, 186, 190, 206, 208, 211, 215, 217, 231, 233, 237]
    Transition Distribution:
              Bear      Flat      Bull
    Bear  0.138889  0.722222  0.138889
    Flat  0.146497  0.598726  0.254777
    Bull  0.125000  0.678571  0.196429

### ALGORITHM USING DP TO FIND MAXIMAL OPTIMAL INDEX:

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

