---
title: Time-series forecasting — methods, libraries, and trade-offs
main_link: https://otexts.com/fpp3/
keywords: [time-series, forecasting, arima, sarimax, lstm, prophet, transformer, statsmodels, sktime, darts, neuralforecast]
status: reviewed
review_date: 2026/05/03
---

# Time-series forecasting — methods, libraries, and trade-offs

**Main link:** <https://otexts.com/fpp3/> (Hyndman & Athanasopoulos, *Forecasting: Principles and Practice*, 3rd ed.)

## Summary

Time-series forecasting predicts the next *N* values of a sequence indexed by time. The space breaks into three rough families: **statistical** models (ARIMA / SARIMA / SARIMAX, ETS, Theta) which dominate small-and-many univariate series; **classical ML** (gradient-boosted trees on lag/rolling features) which usually wins tabular forecasting competitions; and **deep learning** (RNN/LSTM, Transformer variants, N-BEATS, NHiTS, TSMixer, TFT, and the new "foundation-model-for-TS" wave: TimesFM, Lag-Llama, Chronos, Moirai). Pick by data shape — number of series, length per series, exogenous features, seasonality, latency budget — not by what's fashionable.

## Insight

A few things the literature keeps re-discovering:

- **The M-competitions are sobering.** M4 (2018) was won by a hybrid statistical-ML ensemble; M5 (2020) was won by LightGBM with hand-rolled features. Pure DL models often *lose* to ARIMA + ETS ensembles on classic univariate benchmarks. Always run the boring baseline first.
- **Walk-forward validation, not a single hold-out.** Fit on `[0..t]`, predict `[t+1..t+h]`, slide forward. A single train/test split lies — performance varies wildly across regimes.
- **Stationarity matters for ARIMA and friends.** Difference (`d`), seasonally difference (`D`), check ADF / KPSS. `pmdarima.auto_arima` can pick `(p,d,q)(P,D,Q)s` for you but choose the seasonality length carefully.
- **Exogenous features (the X in SARIMAX) are often the win.** Holidays, promotions, weather, regime indicators. Prophet bakes some of this in (changepoints + holiday calendars + Fourier seasonality) which is why it stays popular for business forecasting despite well-known weaknesses.
- **For DL on TS, prefer the *modern* baselines.** N-BEATS (2020), NHiTS (2022), TSMixer (2023), iTransformer (2024), and DLinear (2022 — the "are Transformers effective for TS forecasting?" paper that showed a tiny linear model can match Informer/Autoformer/FEDformer) are all stronger starting points than vanilla LSTM or off-the-shelf Transformer.
- **Foundation models for TS are real now.** TimesFM (Google), Chronos (Amazon), Lag-Llama, Moirai (Salesforce) — pretrained on huge corpora of time-series, do zero-shot forecasting. Useful for cold-start; not usually best on a series with sufficient history.
- **Library landscape (Python):**
  - `statsmodels` — ARIMA, SARIMAX, ETS, state-space models. The reference implementation.
  - `pmdarima` — `auto_arima` wrapper (Hyndman's R `forecast` package, ported).
  - `Prophet` (Meta) — additive model with trend changepoints + Fourier seasonality + holidays.
  - `sktime` — scikit-learn-style API across all paradigms; great for benchmarking.
  - `darts` (Unit8) — unified API for ~30 models including DL; clean, batteries-included.
  - **Nixtla family** — `statsforecast` (fast classical), `mlforecast` (LightGBM/XGBoost wrapper), `neuralforecast` (DL), `hierarchicalforecast` (reconciliation), `utilsforecast`. Currently the most-active TS ecosystem.
  - `GluonTS` (Amazon) — DL-first, MXNet/PyTorch.
  - `AutoTS` — automated model selection and ensembling.
  - `pycaret.time_series` — low-code wrapper.
- **Don't reach for DL when you have <1000 points per series.** ARIMA/ETS or GBDT on lag features will outperform.

## Similar / related topics

- [[time_series_transformer]] — the Transformer-architecture-for-TS lineage (Informer, Autoformer, FEDformer, PatchTST, iTransformer)
- [[kan]] — KAN-based forecasters (alternative to MLP backbones in N-BEATS-style models)
- [[time_sieve]] — wavelet/information-bottleneck non-Transformer architecture
- [[linear_regression]] — the baseline you should always run first
- [[missing_data]] — pre-processing gotchas before fitting any forecaster
- [[../../trading/folbrecht_algo_trading_series|trading: forecasting for algo-trading]] — applied context

## Internal links

- [[time_series_transformer]]
- [[kan]]
- [[../fundamentals/kan|fundamentals/kan]] — the canonical KAN article
- [[time_sieve]]
- [[linear_regression]]
- [[missing_data]]
- [[ml/time_series/papers|papers]]
- [[ml/time_series/tutorials|tutorials]]
- [[../../db/timeseries/README|db/timeseries]] — substrate (InfluxDB, QuestDB, GreptimeDB)
- [[README]]

## Keywords

`#time-series` `#forecasting` `#arima` `#sarimax` `#prophet` `#lstm` `#transformer` `#nbeats` `#nhits` `#nixtla` `#darts` `#sktime` `#statsmodels`

## References / raw notes

Two practitioner blog posts informed the snippets below:

- *Time Series Forecasting: A Comparative Analysis of SARIMAX, RNN, LSTM, Prophet, and Transformer Models* — <https://medium.datadriveninvestor.com/time-series-forecasting-a-comparative-analysis-of-sarimax-rnn-lstm-prophet-and-transformer-aed4930ae187>
- *Predicting Stock Prices: using ARIMA, Fourier Transformation and Deep Learning* — <https://medium.com/shikhars-data-science-projects/predicting-stock-prices-using-arima-fourier-transformation-and-deep-learning-e5fb4f693c85>
- *Combining ARIMA, LSTM and Fourier Models with Technical Indicators for JPM Price Prediction and Backtesting* (Alexzap) — see also <https://www.kaggle.com/code/nageshsingh/stock-market-forecasting-arima>

Reference texts:

- Hyndman & Athanasopoulos, *Forecasting: Principles and Practice* — <https://otexts.com/fpp3/>
- Makridakis, Spiliotis, Assimakopoulos, *The M4 Competition: 100,000 time series and 61 forecasting methods* (2020)
- Zeng et al., *Are Transformers Effective for Time Series Forecasting?* (AAAI 2023) — DLinear paper

### Worked example — ARIMA + Fourier + technical indicators + small Keras MLP

The following is a condensed, properly-fenced version of the Goldman Sachs price-prediction walkthrough. It is a teaching example, not a recommendation: predicting daily close prices is a hard problem and the methodology shown here would not survive proper out-of-sample evaluation in a trading context.

Download the data with `yfinance`:

```python
import yfinance as yf

# Download data
gs = yf.download("GS", start="2011-01-01", end="2021-01-01")
```

Reduce to a single Close-price series:

```python
import pandas as pd

# Preprocess data
dataset_ex_df = gs.copy()
dataset_ex_df = dataset_ex_df.reset_index()
dataset_ex_df['Date'] = pd.to_datetime(dataset_ex_df['Date'])
dataset_ex_df.set_index('Date', inplace=True)
dataset_ex_df = dataset_ex_df['Close'].to_frame()
```

Pick ARIMA orders automatically:

```python
from pmdarima.arima import auto_arima

# Auto ARIMA to select optimal ARIMA parameters
model = auto_arima(dataset_ex_df['Close'], seasonal=False, trace=True)
print(model.summary())
# Best model on this data: ARIMA(0,1,2)(0,0,0)[0]
```

Walk-forward validation with a refit at every step (slow but honest):

```python
from statsmodels.tsa.arima.model import ARIMA
import numpy as np

# Define the ARIMA model
def arima_forecast(history):
    # Fit the model
    model = ARIMA(history, order=(0, 1, 0))
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
predictions = []
for t in range(len(test)):
    # Generate a prediction
    yhat = arima_forecast(history)
    predictions.append(yhat)
    # Add the predicted value to the training set
    obs = test[t]
    history.append(obs)
```

Plot ARIMA vs actual:

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6), dpi=100)
plt.plot(dataset_ex_df.iloc[size:, :].index, test, label='Real')
plt.plot(dataset_ex_df.iloc[size:, :].index, predictions, color='red', label='Predicted')
plt.title('ARIMA Predictions vs Actual Values')
plt.xlabel('Date')
plt.ylabel('Stock Price')
plt.legend()
plt.show()
```

Add Fourier components as features (decompose the price into a low-frequency trend + higher-frequency cycles):

```python
# Calculate the Fourier Transform
data_FT = dataset_ex_df[['Close']]
close_fft = np.fft.fft(np.asarray(data_FT['Close'].tolist()))
fft_df = pd.DataFrame({'fft': close_fft})
fft_df['absolute'] = fft_df['fft'].apply(lambda x: np.abs(x))
fft_df['angle'] = fft_df['fft'].apply(lambda x: np.angle(x))

# Plot the Fourier Transforms
plt.figure(figsize=(14, 7), dpi=100)
plt.plot(np.asarray(data_FT['Close'].tolist()), label='Real')
for num_ in [3, 6, 9]:
    fft_list_m10 = np.copy(close_fft)
    fft_list_m10[num_:-num_] = 0
    plt.plot(np.fft.ifft(fft_list_m10), label=f'Fourier transform with {num_} components')
plt.xlabel('Days')
plt.ylabel('USD')
plt.title('Goldman Sachs (close) stock prices & Fourier transforms')
plt.legend()
plt.show()
```

Standard technical indicators (EMA, RSI, MACD, OBV) as additional features:

```python
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
    return 100.0 - (100.0 / (1.0 + rs))

# Calculate MACD
def macd(close, fast_period=12, slow_period=26, signal_period=9):
    fast_ema = close.ewm(span=fast_period, adjust=False).mean()
    slow_ema = close.ewm(span=slow_period, adjust=False).mean()
    macd_line = fast_ema - slow_ema
    # signal_line = macd_line.ewm(span=signal_period, adjust=False).mean()
    return macd_line

# Calculate OBV
def obv(close, volume):
    return np.where(
        close > close.shift(), volume,
        np.where(close < close.shift(), -volume, 0)
    ).cumsum()
```

Assemble the feature matrix:

```python
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
```

Train/test split + scaling (note: scaler is fit on train only, then applied to test):

```python
from sklearn.preprocessing import MinMaxScaler

# Separate in Train and Test Dfs
train_size = int(len(merged_df) * 0.8)
train_df, test_df = merged_df.iloc[:train_size], merged_df.iloc[train_size:]

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
```

A small Keras MLP with early stopping. Crucially, `shuffle=False` — never shuffle a time-series train set:

```python
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
history = model.fit(
    X_train, y_train,
    epochs=1000, batch_size=32, verbose=1,
    validation_data=(X_test, y_test),
    callbacks=[early_stop],
    shuffle=False,
)
```

Regression metrics:

```python
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

print(f"MSE:  {mse}")
print(f"MAE:  {mae}")
print(f"R2:   {r2}")
print(f"EVS:  {evs}")
print(f"MAPE: {mape}")
print(f"MPE:  {mpe}")
```

Plot final predictions:

```python
import matplotlib.pyplot as plt

# Plot final Predictions
plt.figure(figsize=(12, 6), dpi=100)
plt.plot(test_scaled_df.index, y_test, label='Real')
plt.plot(test_scaled_df.index, y_pred, color='red', label='Predicted')
plt.title('Model Predictions vs Real Prices')
plt.xlabel('Date')
plt.ylabel('Stock Price')
plt.legend()
plt.show()
```

### Further reading

- <https://towardsdatascience.com/a-quick-deep-learning-recipe-time-series-forecasting-with-keras-in-python-f759923ba64>
- <https://machinelearningmastery.com/arima-for-time-series-forecasting-with-python/>
- <https://algotrading101.com/learn/yfinance-guide/>
- <https://www.investopedia.com/terms/f/fourieranalysis.asp>
- <https://www.incrediblecharts.com/indicators/technical-indicators.php>
