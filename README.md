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

### 1. Algorithm: Viterbi Portfolio Optimization (HMM)

The algorithm utilizes historical market data and current trends to make structured buy order decisions. Employing dynamic programming techniques, it efficiently navigates a Hidden Markov Model to determine the most profitable sequence of trades while avoiding redundant calculations. Operating in a streaming manner, it provides real-time decision support for buy orders. Continuously updating transition distribution enables adaptation to changing market conditions, offering timely insights into optimal buying opportunities. By accurately identifying transitions between market states, it aids in informed investment decisions aimed at maximizing portfolio value.

### Pseudocode:

```python
function ViterbiOptimalIndex(states, init, trans, emit, obs) is
    input states: S hidden states
    input init: initial probabilities of each state
    input trans: S × S transition matrix
    input emit: S × O emission matrix
    input obs: sequence of T observations

    prob ← T × S matrix of zeroes
    prev ← empty T × S matrix
    portfolio_value ← 0
    optimal_buy_indices ← empty array
    
    for each state s in states do
        prob[0][s] = init[s] * emit[s][obs[0]]
    end
    
    for t = 1 to T - 1 do
        for each state s in states do
            max_prob ← 0
            max_state ← -1
            for each state r in states do
                new_prob ← prob[t - 1][r] * trans[r][s] * emit[s][obs[t]]
                if new_prob > max_prob then
                    max_prob ← new_prob
                    max_state ← r
                end
            end
            prob[t][s] ← max_prob
            prev[t][s] ← max_state
        end
    end
    
    max_prob_last_state ← 0
    max_state_last_state ← -1
    for each state s in states do
        if prob[T - 1][s] > max_prob_last_state then
            max_prob_last_state ← prob[T - 1][s]
            max_state_last_state ← s
        end
    end
    
    path ← empty array of length T
    path[T - 1] ← max_state_last_state
    for t = T - 1 to 1 do
        path[t - 1] ← prev[t][path[t]]
    end
    
    state_transitions ← S × S matrix of zeroes
    for i = 1 to T - 1 do
        if path[i] == 1 and path[i - 1] == 0 and trans[1][2] > trans[1][0] then
            portfolio_value ← portfolio_value + 1
            optimal_buy_indices.append(i - 1)
            state_transitions[path[i - 1] + 1][path[i] + 1] ← state_transitions[path[i - 1] + 1][path[i] + 1] + 1
        elseif path[i] == -1 and path[i - 1] == 0 and trans[0][1] > trans[0][2] then
            portfolio_value ← portfolio_value - 1
            state_transitions[path[i - 1] + 1][path[i] + 1] ← state_transitions[path[i - 1] + 1][path[i] + 1] + 1
        else
            state_transitions[path[i - 1] + 1][path[i] + 1] ← state_transitions[path[i - 1] + 1][path[i] + 1] + 1
            continue
        end
    end
    
    transition_distribution ← S × S matrix of zeroes
    for i = 0 to S - 1 do
        total_sum ← sum of elements in state_transitions[i]
        for j = 0 to S - 1 do
            transition_distribution[i][j] ← state_transitions[i][j] / total_sum
        end
    end
    
    return portfolio_value, optimal_buy_indices, transition_distribution
end
```
Note : The algorithm is modified according to our convience to calculate the optimal index and portfolio value.

### 2. Algorithm to find the Maximal optimal index:

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

### Pseudocode:
```python
Algorithm find_optimal_index(lst):
    Input: lst - a list of tuples where each tuple contains the index and the portfolio value
    Output: optimal_indices - a list of indices representing the maximal optimal index

    n <- length of lst
    DP <- array of size n initialized with zeros
    indices <- array of n lists, each list initialized with an empty list

    for i from 0 to n-1 do:
        DP[i] <- portfolio value at index i
        indices[i].append(i)
        
        for j from 0 to i-1 do:
            if portfolio value at index i > portfolio value at index j then:
                if DP[j] + portfolio value at index i > DP[i] then:
                    DP[i] <- DP[j] + portfolio value at index i
                    indices[i] <- copy of indices[j] appended with i
    
    max_index <- index of the maximum value in DP
    optimal_indices <- indices[max_index]
    
    return optimal_indices
```
