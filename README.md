# Sigma-Internship-Coding-Challenge

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
