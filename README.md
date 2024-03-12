# Sigma-Internship-Coding-Challenge

# MarketModel Readme

## Overview

The `MarketModel` class is designed to perform market analysis and simulate a basic trading strategy based on historical stock price data. The `MarketModel` class utilizes the QuantRocket API for fetching historical price data and Pandas/Numpy for data manipulation and analysis. The primary goal is to demonstrate a simple trading strategy simulation based on predefined market states.

### Class Initialization

```python
__init__(ticker, start_date, end_date)
```

- **ticker**: Stock ticker symbol (e.g., 'FIBBG000B9XRY4').
- **start_date**: Start date for historical data (e.g., '2023-01-01').
- **end_date**: End date for historical data (e.g., '2023-12-31').

This method initializes the `MarketModel` class with the specified stock ticker and date range. It also sets up essential variables for subsequent analysis.

## Data Fetching

```python
fetch_data()
```
This method fetches historical price data for the specified stock using QuantRocket. It retrieves daily closing prices from the 'usstock-free-1d' universe within the given date range.

## Returns Calculation

```python
calculate_returns()
```
Calculates daily returns based on the closing prices obtained from the fetched historical data. It uses the Pandas library to compute the percentage change in closing prices.

## State Classification

```python
classify_states()
```
Classifies market states as Bear, Flat, or Bull based on predefined thresholds. It uses Numpy to categorize states according to the calculated daily returns.

## Value Function Implementation

```python
implement_value_function()
```
Implements a basic value function to simulate a simple trading strategy. The function is executed over the classified market states, resulting in decisions such as buying or selling. The method also tracks the portfolio value and optimal buying indices.

## Transition Distribution Calculation

```python
calculate_transition_distribution()
```
Calculates transition distributions between different market states. It uses Numpy to compute the transition probabilities based on the occurrences of state transitions.

## Results Printing

```python
print_results()
```
Prints the final portfolio value, optimal buy indices, and the calculated transition distribution. It provides insights into the performance and behavior of the simulated trading strategy.

Example usage of the model:

    model = MarketModel('FIBBG000B9XRY4', '2023-01-01', '2023-12-31')
    model.fetch_data()
    model.calculate_returns()
    model.classify_states()
    model.implement_value_function()
    model.calculate_transition_distribution()
    model.print_results()

Output:

    Portfolio Value V(d): 17
    Optimal Index: [5, 7, 11, 15, 20, 27, 29, 40, 49, 51, 58, 60, 68, 78, 84, 87, 93, 99, 102, 107, 109, 112, 116, 119, 122, 132, 141, 159, 163, 176, 186, 190, 206, 208, 211, 215, 217, 231, 233, 237]
    Transition Distribution:
              Bear      Flat      Bull
    Bear  0.138889  0.722222  0.138889
    Flat  0.146497  0.598726  0.254777
    Bull  0.125000  0.678571  0.196429
