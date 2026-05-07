---
title: "MQL5 + Python: socket bridge and MetaTrader5 package"
main_link: https://www.mql5.com/en/articles/5691
keywords: [mql5, mt5, python, metatrader5, sockets, trading, forex, expert-advisor]
status: reviewed
---

# MQL5 + Python: socket bridge and MetaTrader5 package

**Main link:** <https://www.mql5.com/en/articles/5691> (Maxim Dmitrievsky, "Connecting MetaTrader 5 and Python via sockets")

Other useful entry points:

- MQL5 language reference: <https://www.metatrader5.com/en/automated-trading/mql5>
- MQL5 book / intro: <https://www.mql5.com/en/book/intro/edit_compile_run>
- `MetaTrader5` Python package: <https://pypi.org/project/MetaTrader5/>

## Summary

Two complementary ways to use Python alongside [MetaTrader 5](https://www.metatrader.com/) / [MQL5](https://www.mql5.com/en/docs):

1. **Socket bridge** — MQL5's standard library has TCP `Socket*` functions but only as a *client*. The pattern is to run a Python TCP server (using the stdlib `socket` package) and have an Expert Advisor connect to it from `OnTick`, send a payload (e.g. recent close prices), and consume the response (e.g. coordinates of a regression line to draw on the chart).
2. **`MetaTrader5` Python package** — pip-installable, lets a Python process talk *directly* to a running MT5 terminal: `MT5Initialize()`, `MT5CopyRatesFromPos(...)`, `MT5CopyTicksFrom(...)`, `MT5Shutdown()`. From there it's normal pandas + matplotlib / plotly territory: correlation matrices, cointegration tests with `statsmodels`, candlestick charts, bid/ask scatter plots.

## Insight

The socket bridge is genuinely useful as a pattern even outside MQL5 — any time you have a fast/restricted DSL host (here MQL5; could equally be a CUDA host or a sandboxed scripting engine) and want to offload heavy work (ML inference, scikit-learn, anything) to a richer language, a localhost TCP socket is the cheapest interop. The downside in MQL5 specifically is that the article admits the socket client doesn't run in the **Strategy Tester**, which means you can't backtest strategies that depend on the bridge. That's a fairly serious limitation if you care about pre-trade validation.

The `MetaTrader5` Python package is the easier path for *analysis* (pull rates, do stats, draw chart) but doesn't replace the EA, since strategy execution still happens in the terminal. So in practice the split tends to be: MT5 for execution + EA for tick-level reactions; Python for offline backtesting, indicator research, and ML. If you find yourself wanting Python on both sides of that line, look at [[hummingbot_python]] (crypto only) or step out of the MT5 ecosystem altogether toward a Rust framework like [[barter]].

A tactical warning about the article's code: most of the embedded Python and MQL5 snippets are pasted *unfenced*, so what looks like an H1 heading (`# Initializing MT5 connection`, `# Copying data to dataframe`, `# sbcapitalfx Heat Map - version 1.0`, ...) is actually a Python comment. The section below preserves the snippets but wraps them in proper code fences — keep them fenced if you ever copy this file again.

## Similar / related topics

- [[barter]] — Rust trading framework; full alternative if you can leave MT5.
- [[hummingbot_python]] — Python framework for crypto exchanges (different broker world, same "let's automate trading from Python" goal).
- [[folbrecht_algo_trading_series]] — Rust trading system using Tradier, not MT5; useful contrast.
- [[trade_aggregation_rust_candles]] — candle aggregation in Rust if you want to leave MT5's bar engine.
- `statsmodels` cointegration / `scikit-learn` linear regression — what the bridged Python code is doing.

## Internal links

- [[barter]] — Rust trading engine alternative
- [[hummingbot_python]] — Python framework for crypto exchanges
- [[folbrecht_algo_trading_series]] — Rust + Tradier instead of MQL5 + MT5
- [[trade_aggregation_rust_candles]] — alternative bar aggregation

## Keywords

`#trading` `#mql5` `#mt5` `#metatrader5` `#python` `#sockets` `#forex` `#expert-advisor`

## References / raw notes

Original article: *Connecting MetaTrader 5 and Python via sockets* — <https://www.mql5.com/en/articles/5691> (translated from <https://www.mql5.com/ru/articles/5691>). Code samples below are reproduced from that article and from comments by readers; they are **kept here verbatim and fenced** so they don't trigger the article-splitter heuristics again.

### Why bridge MQL5 to Python?

> Comprehensive data processing requires extensive tools and is often beyond the sandbox of one single application. Specialized programming languages are used for processing and analyzing data, statistics and machine learning. One of the leading programming languages for data processing is Python.

Sockets are picked because they're the lowest-common-denominator IPC — TCP/IP works inside one box, across a LAN, or across the internet, and MQL5's standard library exposes `SocketCreate`, `SocketConnect`, `SocketIsReadable`, `SocketRead`, `SocketSend`, `SocketClose`. Since MQL5 only ships a client, the *server* must live on the Python side.

### Python TCP server (regression on close prices)

```python
import socket
import numpy as np
from sklearn.linear_model import LinearRegression

class socketserver:
    def __init__(self, address='', port=9090):
        self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.address = address
        self.port = port
        self.sock.bind((self.address, self.port))
        self.cummdata = ''

    def recvmsg(self):
        self.sock.listen(1)
        self.conn, self.addr = self.sock.accept()
        print('connected to', self.addr)
        self.cummdata = ''

        while True:
            data = self.conn.recv(10000)
            self.cummdata += data.decode("utf-8")
            if not data:
                break
            self.conn.send(bytes(calcregr(self.cummdata), "utf-8"))
            return self.cummdata

    def __del__(self):
        self.sock.close()

def calcregr(msg=''):
    chartdata = np.fromstring(msg, dtype=float, sep=' ')
    Y = np.array(chartdata).reshape(-1, 1)
    X = np.array(np.arange(len(chartdata))).reshape(-1, 1)

    lr = LinearRegression()
    lr.fit(X, Y)
    Y_pred = lr.predict(X)
    P = Y_pred.astype(str).item(-1) + ' ' + Y_pred.astype(str).item(0)
    print(P)
    return str(P)

serv = socketserver('127.0.0.1', 9090)
while True:
    msg = serv.recvmsg()
```

The price stream comes in as space-separated floats, gets reshaped into the `(n, 1)` column shape that scikit-learn wants for `X` and `Y`, and the predicted endpoints of the regression line are returned as `"<right_y> <left_y>"`.

### MQL5 socket client (Expert Advisor)

```mql5
bool socksend(int sock, string request) {
    char req[];
    int len = StringToCharArray(request, req) - 1;
    if (len < 0) return false;
    return SocketSend(sock, req, len) == len;
}

string socketreceive(int sock, int timeout) {
    char rsp[];
    string result = "";
    uint len;
    uint timeout_check = GetTickCount() + timeout;
    do {
        len = SocketIsReadable(sock);
        if (len) {
            int rsp_len = SocketRead(sock, rsp, len, timeout);
            if (rsp_len > 0) {
                result += CharArrayToString(rsp, 0, rsp_len);
            }
        }
    } while ((GetTickCount() < timeout_check) && !IsStopped());
    return result;
}

void drawlr(string points) {
    string res[];
    StringSplit(points, ' ', res);
    if (ArraySize(res) == 2) {
        datetime temp[];
        CopyTime(Symbol(), Period(), TimeCurrent(), lrlenght, temp);
        ObjectCreate(0, "regrline", OBJ_TREND, 0,
                     TimeCurrent(),
                     NormalizeDouble(StringToDouble(res[0]), _Digits),
                     temp[0],
                     NormalizeDouble(StringToDouble(res[1]), _Digits));
    }
}

void OnTick() {
    int socket = SocketCreate();
    if (socket != INVALID_HANDLE) {
        if (SocketConnect(socket, "localhost", 9090, 1000)) {
            double clpr[];
            int copyed = CopyClose(_Symbol, PERIOD_CURRENT, 0, lrlenght, clpr);
            string tosend;
            for (int i = 0; i < ArraySize(clpr); i++)
                tosend += (string)clpr[i] + " ";
            string received = socksend(socket, tosend) ? socketreceive(socket, 10) : "";
            drawlr(received);
        } else {
            Print("Connection error ", GetLastError());
        }
        SocketClose(socket);
    } else {
        Print("Socket creation error ", GetLastError());
    }
}
```

### Direct quote access via the `MetaTrader5` Python package

```bash
pip install MetaTrader5
```

FX correlation heatmap example:

```python
from MetaTrader5 import *
from datetime import date
import pandas as pd
import matplotlib.pyplot as plt

# Initializing MT5 connection
MT5Initialize()
MT5WaitForTerminal()
print(MT5TerminalInfo())
print(MT5Version())

sym = ['EURUSD', 'GBPUSD', 'USDJPY', 'USDCHF', 'AUDUSD', 'GBPJPY']

# Pull 1000 M1 bars per symbol into a DataFrame
d = pd.DataFrame()
for i in sym:
    rates = MT5CopyRatesFromPos(i, MT5_TIMEFRAME_M1, 0, 1000)
    d[i] = [y.close for y in rates]

MT5Shutdown()

rets = d.pct_change()
corr = rets.corr()

plt.figure(figsize=(10, 10))
plt.imshow(corr, cmap='RdYlGn', interpolation='none', aspect='auto')
plt.colorbar()
plt.xticks(range(len(corr)), corr.columns, rotation='vertical')
plt.yticks(range(len(corr)), corr.columns)
plt.suptitle('FOREX Correlations Heat Map', fontsize=15, fontweight='bold')
plt.show()
```

Cointegration test on a strongly-correlated pair (`GBPUSD` / `GBPJPY`):

```python
from statsmodels.tsa.stattools import coint

x = d['GBPUSD']
y = d['GBPJPY']
x = (x - min(x)) / (max(x) - min(x))
y = (y - min(y)) / (max(y) - min(y))

score = coint(x, y)
print('t-statistic:', score[0], ' p-value:', score[1])

# Z-score of the spread
diff_series = (x - y)
zscore = (diff_series - diff_series.mean()) / diff_series.std()
plt.plot(zscore)
plt.axhline(2.0, color='red', linestyle='--')
plt.axhline(-2.0, color='green', linestyle='--')
plt.show()
```

Plotly OHLC chart from EURUSD M1:

```python
from MetaTrader5 import *
import pandas as pd
import plotly.graph_objs as go
from plotly.offline import plot

MT5Initialize()
MT5WaitForTerminal()

stockdata = pd.DataFrame()
rates = MT5CopyRatesFromPos("EURUSD", MT5_TIMEFRAME_M1, 0, 5000)
MT5Shutdown()

stockdata['Open']  = [y.open  for y in rates]
stockdata['Close'] = [y.close for y in rates]
stockdata['High']  = [y.high  for y in rates]
stockdata['Low']   = [y.low   for y in rates]
stockdata['Date']  = [y.time  for y in rates]

trace = go.Ohlc(x=stockdata['Date'],
                open=stockdata['Open'],
                high=stockdata['High'],
                low=stockdata['Low'],
                close=stockdata['Close'])
plot([trace])
```

Bid/ask history scatter:

```python
from MetaTrader5 import *
from datetime import datetime
import plotly.graph_objs as go
from plotly.offline import plot

MT5Initialize()
MT5WaitForTerminal()

rates = MT5CopyTicksFrom("EURUSD", datetime(2019, 3, 14, 13), 1000, MT5_COPY_TICKS_ALL)
bid = [x.bid for x in rates]
ask = [x.ask for x in rates]
time = [x.time for x in rates]
MT5Shutdown()

plot([go.Scatter(x=time, y=bid), go.Scatter(x=time, y=ask)])
```

### Caveats

- **No Strategy Tester for socket-bridged EAs.** This is the article's own conclusion: the MT5 socket client API doesn't work inside the Strategy Tester, so the regression-line strategy can't be backtested through the bridge. For backtesting you have to either keep all the logic in MQL5, or pull data into Python (via the `MetaTrader5` package) and backtest entirely outside MT5.
- **Newer API names.** The `MetaTrader5` package has since shifted to snake_case (e.g. `mt5.copy_rates_from_pos`, `mt5.initialize`, `mt5.shutdown`); the camelCase names (`MT5CopyRatesFromPos`, `MT5Initialize`) above are the originals from the 2019 article and may need translating if you adapt this code today. A reader comment in the original article (Sioux Gougui, Oct 2023) shows exactly this version mismatch biting in practice.
