Example Usage:

    ticker = 'AAPL'
    start_date = '2000-01-01'
    end_date = '2022-12-31'
    forecast_start_date = '2023-01-01'
    forecast_end_date = '2023-12-31'
    
    predictor = StockPricePredictor(ticker=ticker, start_date=start_date, end_date=end_date)
    data = predictor.download_data()
    scaled_data = predictor.preprocess_data(data.values.astype('float32'))
    X, y = predictor.create_sequences(scaled_data)
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
    predictor.build_model()
    predictor.train_model(X_train, y_train)
    predictor.evaluate_model(X_train, y_train, X_test, y_test)
    
    forecast_data = yf.download(ticker, start=forecast_start_date, end=forecast_end_date)
    scaled_forecast_data = predictor.preprocess_data(forecast_data[['Close']].values.astype('float32'))
    predictions = predictor.predict_future(scaled_forecast_data)
    
    forecast_data['Predicted_Close'] = np.concatenate((data['Close'][-predictor.sequence_length:].values, predictions))
    print(forecast_data[['Close', 'Predicted_Close']])

    simulator = TradingStrategySimulator()
    transition_distribution, portfolio_value, optimal_buy_indices = simulator.simulate(forecast_data['Close'])

    print("Transition Distribution:")
    print(pd.DataFrame(transition_distribution, index=['Bear', 'Flat', 'Bull'], columns=['Bear', 'Flat', 'Bull']))
    print("\nPortfolio Value:", portfolio_value)
    print("Optimal Buy Indices:", optimal_buy_indices)

    simulator = TradingStrategySimulator()
    transition_distribution, portfolio_value, optimal_buy_indices = simulator.simulate(forecast_data['Predicted_Close'])

    print("\n\nTransition Distribution:")
    print(pd.DataFrame(transition_distribution, index=['Bear', 'Flat', 'Bull'], columns=['Bear', 'Flat', 'Bull']))
    print("\nPortfolio Value:", portfolio_value)
    print("Optimal Buy Indices:", optimal_buy_indices)
