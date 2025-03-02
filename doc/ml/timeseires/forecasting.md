# forecasting


## Time Series Forecasting: A Comparative Analysis of SARIMAX, RNN, LSTM, Prophet, and Transformer Models



https://medium.datadriveninvestor.com/time-series-forecasting-a-comparative-analysis-of-sarimax-rnn-lstm-prophet-and-transformer-aed4930ae187



Assessing the Efficiency and Efficacy of Leading Forecasting Algorithms Across Diverse Datasets


Photo by Hal Gatewood on Unsplash
#1 Purpose and Overview
Time series forecasting predicts future events based on past patterns. Our goal is to identify the best prediction methods, as different techniques excel under specific conditions. This post examines how various methods perform on different datasets, offering insights into choosing and fine-tuning the right forecasting method for any scenario.

We’ll explore five key methods:

· SARIMAX: Detects recurring patterns and accounts for various external influences.

· RNN: Analyzes sequential data, ideal for chronological information.

· LSTM: Enhances RNNs by retaining data over extended periods.

· Prophet: Developed by Facebook, robust against data gaps and significant trend shifts.

· Transformer: Utilizes self-attention to identify intricate patterns effectively.

We put these methods to the test on different kinds of data:

· Electric Production: Analyzing industry energy consumption trends over time. Kaggle Dataset


## Combining ARIMA, LSTM and Fourier Models with Technical Indicators for JPM Price Prediction and Backtesting
Alexzap


![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*rVPkDZ0IQeGpx1QnvLFDmQ.png)




The objective of this post is to combine autoregressive, Fourier and deep learning models with Technical Indicators (TI) for robust stock price prediction and backtesting, focusing specifically on JPM.
Business Motivation: Banks have always been vulnerable to quickly shifting sentiment among investors, depositors, and regulators. In the year since authorities in the U.S. and Switzerland stepped in to quell contagion risk after the collapse of SVB, Signature, and Credit Suisse, regulators remain acutely aware of the sector’s sensitivities.
Unlike other papers that concentrate on a one-size-fits-all technique, in this paper we follow a more holistic strategy that integrates predictive analytics and TI into a single decision-making framework aimed at detecting and predicting financial asset price breakouts and trends to make informed decisions.
The key approach that influenced the present study under the umbrella of algo-trading (AT) is the integrated time-series forecasting analysis successfully demonstrated on the GS data.
The ultimate business goal of AT is the systematic application of Financial Risk Management to stock (algorithmic) trading and investments. In fact, traders always…

https://www.kaggle.com/code/nageshsingh/stock-market-forecasting-arima


https://www.investopedia.com/terms/f/fourieranalysis.asp

https://www.nature.com/articles/s41599-024-02807-x

https://www.incrediblecharts.com/indicators/technical-indicators.php





## Predicting Stock Prices: using ARIMA, Fourier Transformation and Deep Learning

https://medium.com/shikhars-data-science-projects/predicting-stock-prices-using-arima-fourier-transformation-and-deep-learning-e5fb4f693c85


Importance of Predicting Stock Prices:
Predicting stock prices has become increasingly important in the finance industry over the last decade due to several reasons:

Improved computational power: With the advances in technology, computational power has increased exponentially over the years. This has allowed for the development of more sophisticated algorithms and models that can analyze large amounts of data and make predictions with greater accuracy.

Availability of data: The finance industry has access to more data than ever before, including financial reports, economic indicators, and news articles. This has made it possible to develop more comprehensive models that take into account a wide range of factors that can impact stock prices.

Increased competition: As the financial markets have become more competitive, the ability to predict stock prices has become a key advantage for investors and traders. By accurately predicting stock prices, investors can make more informed decisions about which stocks to buy or sell, potentially giving them an edge over their competitors.

Changing market conditions: The financial markets are constantly evolving, and predicting stock prices can help investors and traders stay ahead of the curve. For example, by accurately predicting a market downturn, investors can adjust their portfolios accordingly, potentially minimizing their losses.

Downloading Goldman Sachs’ stock prices using yfinance library:
yfinance is a Python library that allows users to download historical market data, as well as real-time data, from Yahoo Finance. It provides an easy-to-use interface to fetch financial data such as stock prices, dividends, splits, and trading volumes.

The yfinance library is built on top of the Yahoo Finance API, which is a popular source of financial data for traders, investors, and financial analysts. By using the library, users can download financial data for any publicly-traded company or index that is available on Yahoo Finance.

Some of the features of the yfinance library include the ability to download data for a specific time period, adjust historical prices for splits and dividends, and retrieve real-time data for individual securities. The library also supports multi-threading, making it possible to download data for multiple securities simultaneously.

Overall, yfinance is a useful tool for anyone who needs to access financial data from Yahoo Finance in their Python code.

import yfinance as yf

# Download data
gs = yf.download("GS", start="2011-01-01", end="2021-01-01")

Understanding the data -
In finance stocks, Open, High, Low, Close, Adjusted Close, and Volume are commonly used terms to describe different aspects of a stock’s performance on a particular day or over a certain period of time. Here’s what each term means:

Open: The opening price of a stock is the price at which it starts trading for the day. This is usually based on the closing price from the previous day, but can also be affected by after-hours trading or news events that occur before the market opens.

High: The high price of a stock is the highest price that it reached during the trading day. This can be an important indicator of the stock’s performance, as it shows how high investors were willing to bid for the stock.

Low: The low price of a stock is the lowest price that it reached during the trading day. This can be an important indicator of the stock’s performance, as it shows how low investors were willing to bid for the stock.

Close: The closing price of a stock is the price at which it ends trading for the day. This is usually based on the last trade of the day, but can also be affected by after-hours trading or news events that occur after the market closes.

Adjusted Close: The adjusted close price of a stock takes into account any corporate actions that might affect the stock’s value, such as stock splits or dividend payments. This is important because it allows investors to track the actual performance of the stock, rather than just the price movements.

Volume: The volume of a stock is the number of shares that were traded during the trading day. This can be an important indicator of the stock’s liquidity, as it shows how many shares were available for investors to buy or sell. High volume can also indicate increased investor interest in the stock.


import pandas as pd
# Preprocess data
dataset_ex_df = gs.copy()
dataset_ex_df = dataset_ex_df.reset_index()
dataset_ex_df['Date'] = pd.to_datetime(dataset_ex_df['Date'])
dataset_ex_df.set_index('Date', inplace=True)
dataset_ex_df = dataset_ex_df['Close'].to_frame()
Using ARIMA for forecasting:
ARIMA is a statistical model used for time series analysis and forecasting. It can be used for predicting stock prices by analyzing historical data to identify trends and patterns, and applying the ARIMA model to forecast future prices based on identified parameters. ARIMA can capture both short-term and long-term trends, as well as seasonal patterns, but it should be used in combination with other methods to make well-informed investment decisions.

from pmdarima.arima import auto_arima

# Auto ARIMA to select optimal ARIMA parameters
model = auto_arima(dataset_ex_df['Close'], seasonal=False, trace=True)
print(model.summary())

The best model is ARIMA(0,1,2)(0,0,0)[0]
After identifying the ideal parameters, we can apply the ARIMA model to the data and make forecasts. To accomplish this, we will use a rolling fit approach, which entails training the model on a set of data and utilizing it to estimate the next value in the test set. Following this, we will append the anticipated value to the training set and continue the process until we have made predictions for the entire test set.

from statsmodels.tsa.arima.model import ARIMA
import numpy as np

# Define the ARIMA model
def arima_forecast(history):
# Fit the model
model = ARIMA(history, order=(0,1,0))
model_fit = model.fit()

    # Make the prediction
    output = model_fit.forecast()
    yhat = output[0]
    return yhat

# Split data into train and test sets
X = dataset_ex_df.values
size = int(len(X) * 0.8)
train, test = X[0:size], X[size:len(X)]

# Walk-forward validation
history = [x for x in train]
predictions = list()
for t in range(len(test)):
# Generate a prediction
yhat = arima_forecast(history)
predictions.append(yhat)
# Add the predicted value to the training set
obs = test[t]
history.append(obs)
Comparing the performance of ARIMA model against the actual Test data to understand how the model is working

import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6), dpi=100)
plt.plot(dataset_ex_df.iloc[size:,:].index, test, label='Real')
plt.plot(dataset_ex_df.iloc[size:,:].index, predictions, color='red', label='Predicted')
plt.title('ARIMA Predictions vs Actual Values')
plt.xlabel('Date')
plt.ylabel('Stock Price')
plt.legend()
plt.show()

We can notice that the ARIMA forecasts bear a striking resemblance to the factual closing prices. This implies that the model is performing effectively by identifying the fundamental patterns in the data and generating precise predictions.
Fourier Transformation
Fourier Transformation is a mathematical technique that can be used in stock price prediction to analyze and identify underlying patterns in the data. Fourier Transformation is based on the principle that any periodic function can be represented as a sum of simple sine and cosine waves with different frequencies and amplitudes.

By applying Fourier Transformation to stock market data, we can decompose the data into its component frequencies and examine how they contribute to the overall pattern. This can help us to identify trends, cycles, and other patterns in the data that may not be apparent by simply looking at the raw stock prices.

One common approach to using Fourier Transformation for stock price prediction is to decompose the data into different frequency bands and then use each band to make a separate prediction. For example, low-frequency components may be used to predict long-term trends, while high-frequency components may be used to predict short-term fluctuations.

# Calculate the Fourier Transform
data_FT = dataset_ex_df[['Close']]
close_fft = np.fft.fft(np.asarray(data_FT['Close'].tolist()))
fft_df = pd.DataFrame({'fft':close_fft})
fft_df['absolute'] = fft_df['fft'].apply(lambda x: np.abs(x))
fft_df['angle'] = fft_df['fft'].apply(lambda x: np.angle(x))

# Plot the Fourier Transforms
plt.figure(figsize=(14, 7), dpi=100)
plt.plot(np.asarray(data_FT['Close'].tolist()),  label='Real')
for num_ in [3, 6, 9]:
fft_list_m10= np.copy(close_fft); fft_list_m10[num_:-num_]=0
plt.plot(np.fft.ifft(fft_list_m10), label='Fourier transform with {} components'.format(num_))
plt.xlabel('Days')
plt.ylabel('USD')
plt.title('Goldman Sachs (close) stock prices & Fourier transforms')
plt.legend()
plt.show()

The resulting plot shows the actual closing prices of the Goldman Sachs stock alongside the Fourier transforms with 3, 6, and 9 components.
The three-component Fourier transform seems to grasp the overall direction of the closing prices, while the six and nine-component transforms identify additional high-frequency components. Essentially, this means that the Fourier transformation method extracts both long and short-term trends. The three-component transform is regarded as the long-term trend component.

Technical Indicators
In time series modeling, technical indicators can be added to provide additional information about the stock price behavior. Technical indicators are mathematical calculations based on historical stock market data that can be used to identify patterns and trends in the data.

It’s also important to select the appropriate technical indicators for the specific stock or market being analyzed, as different indicators may be more effective for different types of data. As with any modeling technique, it’s important to evaluate the performance of the model using historical data and adjust the parameters as necessary to improve the accuracy of the predictions.

Specifically, we will be utilizing several indicators including the Exponential Moving Average (EMA) with period lengths of 20, 50, and 100, the Relative Strength Index (RSI), the Moving Average Convergence Divergence (MACD), and the On-Balance Volume (OBV).

EMA, or exponential moving average, is a type of moving average that gives more weight to recent data points, making it more responsive to short-term changes in the data. EMA can be used to identify trends and potential trend reversals in the stock price.

RSI, or relative strength index, is a momentum oscillator that measures the strength of a stock’s price relative to its own past performance. RSI can be used to identify overbought or oversold conditions in the stock price, which can indicate a potential trend reversal.

MACD, or moving average convergence divergence, is a trend-following momentum indicator that uses two moving averages to identify changes in the stock price trend. MACD can be used to generate buy and sell signals based on crossovers of the two moving averages.

OBV, or on-balance volume, is a volume-based technical indicator that measures buying and selling pressure in the market. OBV can be used to confirm trends and identify potential trend reversals by comparing the volume of buying and selling activity.

Incorporating these technical indicators into time series analysis can help to provide additional insights into the behavior of stock prices, allowing for more informed investment decisions. However, it’s important to note that no single indicator can predict future stock price movements with complete accuracy, and technical analysis should be used in conjunction with other methods and factors to make well-informed investment decisions.

# Calculate EMA
def ema(close, period=20):
return close.ewm(span=period, adjust=False).mean()

# Calculate RSI
def rsi(close, period=14):
delta = close.diff()
gain, loss = delta.copy(), delta.copy()
gain[gain < 0] = 0
loss[loss > 0] = 0
avg_gain = gain.rolling(period).mean()
avg_loss = abs(loss.rolling(period).mean())
rs = avg_gain / avg_loss
rsi = 100.0 - (100.0 / (1.0 + rs))
return rsi

# Calculate MACD
def macd(close, fast_period=12, slow_period=26, signal_period=9):
fast_ema = close.ewm(span=fast_period, adjust=False).mean()
slow_ema = close.ewm(span=slow_period, adjust=False).mean()
macd_line = fast_ema - slow_ema
signal_line = macd_line.ewm(span=signal_period, adjust=False).mean()
histogram = macd_line - signal_line
return macd_line

# Calculate OBV
def obv(close, volume):
obv = np.where(close > close.shift(), volume, np.where(close < close.shift(), -volume, 0)).cumsum()
return obv
Building training and test dataset by merging all the other features created above
# Add technical indicators to dataset DF
dataset_ex_df['ema_20'] = ema(gs["Close"], 20)
dataset_ex_df['ema_50'] = ema(gs["Close"], 50)
dataset_ex_df['ema_100'] = ema(gs["Close"], 100)

dataset_ex_df['rsi'] = rsi(gs["Close"])
dataset_ex_df['macd'] = macd(gs["Close"])
dataset_ex_df['obv'] = obv(gs["Close"], gs["Volume"])

# Create arima DF using predictions
arima_df = pd.DataFrame(history, index=dataset_ex_df.index, columns=['ARIMA'])

# Set Fourier Transforms DF
fft_df.reset_index(inplace=True)
fft_df['index'] = pd.to_datetime(dataset_ex_df.index)
fft_df.set_index('index', inplace=True)
fft_df_real = pd.DataFrame(np.real(fft_df['fft']), index=fft_df.index, columns=['Fourier_real'])
fft_df_imag = pd.DataFrame(np.imag(fft_df['fft']), index=fft_df.index, columns=['Fourier_imag'])

# Technical Indicators DF
technical_indicators_df = dataset_ex_df[['ema_20', 'ema_50', 'ema_100', 'rsi', 'macd', 'obv', 'Close']]

# Merge DF
merged_df = pd.concat([arima_df, fft_df_real, fft_df_imag, technical_indicators_df], axis=1)
merged_df = merged_df.dropna()
merged_df

Splitting data into train and test (80:20):

# Separate in Train and Test Dfs
train_size = int(len(merged_df) * 0.8)
train_df, test_df = merged_df.iloc[:train_size], merged_df.iloc[train_size:]
Performing MinMax Scaler to standardize the feature values, ensuring that they have similar scales and distributions. This can prevent certain features from dominating the model’s learning process and causing inaccurate predictions. MinMax Scaler is especially important when using neural networks, as these models are sensitive to the scale and distribution of the input data. By using scaler, the model can be trained more efficiently and produce more accurate results.

from sklearn.preprocessing import MinMaxScaler

# Scale the features
scaler = MinMaxScaler()
train_scaled = scaler.fit_transform(train_df.drop('Close', axis=1))
test_scaled = scaler.transform(test_df.drop('Close', axis=1))

# Convert the scaled data back to a DataFrame
train_scaled_df = pd.DataFrame(train_scaled, columns=train_df.columns[:-1], index=train_df.index)
test_scaled_df = pd.DataFrame(test_scaled, columns=test_df.columns[:-1], index=test_df.index)

# Merge the scaled features with the target variable
train_scaled_df['Close'] = train_df['Close']
test_scaled_df['Close'] = test_df['Close']

# Split the scaled data into Features and Label
X_train = train_scaled_df.iloc[:, :-1].values
y_train = train_scaled_df.iloc[:, -1].values
X_test = test_scaled_df.iloc[:, :-1].values
y_test = test_scaled_df.iloc[:, -1].values
Training a Deep Learning Model for Prediction:
Using MSE (Mean Sq. Error) as the model loss function. Incorporating early stoppage criteria which enables the training process to stop when the validation loss does not show any significant reduction. This ensures that the model does not overfit and reduces the computational resources required for training. Moreover, we have set the shuffle parameter to False in order to maintain the order of the data samples, which is crucial in time series analysis.

# Import keras modules
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
from tensorflow.keras.callbacks import EarlyStopping

# Define model
model = Sequential()
model.add(Dense(32, activation='relu', input_dim=X_train.shape[1]))
model.add(Dense(16, activation='relu'))
model.add(Dense(1, activation='linear'))

# Compile the model
model.compile(optimizer='adam', loss='mse')


# Define the early stopping callback
early_stop = EarlyStopping(monitor='val_loss', patience=20, verbose=1, mode='min')

# Train the model with early stopping callback
history = model.fit(X_train, y_train, epochs=1000, batch_size=32, verbose=1, validation_data=(X_test, y_test), callbacks=[early_stop], shuffle=False)

Model Evaluation:
Evaluation metrics are essential in deep learning to assess the performance of the model and determine whether it is accurately predicting the target variable. Several evaluation metrics are commonly used in deep learning, including mean squared error (MSE), mean absolute error (MAE), R-squared (R2), explained variance, mean absolute percentage error (MAPE), and mean percentage error (MPE).

MSE measures the average squared difference between the predicted and actual values, giving higher weight to large errors. MAE, on the other hand, measures the average absolute difference between the predicted and actual values, giving equal weight to all errors.

R2 measures the proportion of the variance in the target variable that is explained by the model, while explained variance measures the variance in the target variable that is explained by the model relative to the total variance.

MAPE and MPE are percentage-based metrics that measure the average percentage difference between the predicted and actual values. MAPE is more suitable for smaller errors, while MPE is more appropriate for larger errors.

Choosing the appropriate evaluation metric depends on the specific problem and the desired outcome. For example, MSE and MAE are commonly used in regression problems, while classification problems may use metrics such as accuracy and F1 score. We will be using the regression evaluation metrics, as shown below.

from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score, explained_variance_score

import numpy as np
y_pred = model.predict(X_test)

# Calculate test metrics
mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
mae = mean_absolute_error(y_test, y_pred)

r2 = r2_score(y_test, y_pred)
evs = explained_variance_score(y_test, y_pred)

mape = np.mean(np.abs((y_test - y_pred) / y_test)) * 100
mpe = np.mean((y_test - y_pred) / y_test) * 100

print(f"Mean Squared Error (MSE): {mse}")
print(f"Mean Absolute Error (MAE): {mae}")
print(f"R2 Score: {r2}")
print(f"Explained Variance Score: {evs}")
print(f"Mean Absolute Percentage Error (MAPE): {mape}")
print(f"Mean Percentage Error (MPE): {mpe}")

Overall, the outcomes imply that the model has effectively predicted the target variable with minimal error and high accuracy on the test data. However, it is crucial to take into account other factors such as the dataset’s size, its representativeness, the hyperparameters selected, and the model’s ability to generalize to new data. The article concludes by providing a thorough explanation of the findings to aid in comprehension. Next, we will present a visualization of the model’s ultimate predictions, along with the closing prices of the test data.

# Plot final Predictions
plt.figure(figsize=(12, 6), dpi=100)
plt.plot(test_scaled_df.index, y_test, label='Real')
plt.plot(test_scaled_df.index, y_pred, color='red', label='Predicted')
plt.title('Model Predictions vs Real Prices')
plt.xlabel('Date')
plt.ylabel('Stock Price')
plt.legend()
plt.show()

Conclusion:
In this blog, we explored various techniques to predict the stock price of Goldman Sachs. Initially, ARIMA model was used, which is a time-series forecasting method that employs previous data to forecast future trends. We then moves on to Fourier Transforms that assist in decomposing the time series into its frequency components and use them as features in the model. Furthermore, the we illustrated how technical indicators such as RSI and MACD can produce additional features for the model. Eventually we assessed the performance of the models using different metrics such as MSE, MAE, R2 score, explained variance score, MAPE, and MPE. Ultimately, the article demonstrates how integrating various methods and techniques can result in a more accurate and robust predictive model.

Thank you so much for reading my article. Liked this article? follow my data science journey on Medium and linkedin.


References:
1. The AI Quant
2. https://towardsdatascience.com/a-quick-deep-learning-recipe-time-series-forecasting-with-keras-in-python-f759923ba64
3. https://algotrading101.com/learn/yfinance-guide/
4. https://machinelearningmastery.com/arima-for-time-series-forecasting-with-python/





