# Tutorial 1: Using Centaur Technical Indicators with pandas

> Leverage Rust-powered speed inside familiar pandas workflows.

This tutorial is the first in a series of tutorials:

- 01 - Using Centaur Technical Indicators with pandas
- [02 - Using Centaur Technical Indicators with Plotly](./plotly.md)
- [03 - More advanced use cases for Centaur Technical Indicators](./advanced.md)
- [04 - Connecting to an API](./api-connection.md)

---

## 🎯 Goal

In this tutorial you will learn how to:

- Install and import `centaur_technical_indicators`
- Load OHLCV price data into a pandas `DataFrame`
- Convert columns to the list inputs expected by the library
- Call both `single` (scalar) and `bulk` (rolling / series) indicator functions
- Add indicator outputs back into your `DataFrame`
- Understand parameter and model choices (e.g. different moving average types)
- Handle common pitfalls (NaNs, alignment, window offsets)

---

## 📦 Prerequisites

| Requirement | Notes |
|-------------|-------|
| Python 3.10+ | (Match your environment; verify with `python --version`) |
| pip / virtualenv | Recommended to isolate dependencies |
| pandas | For data handling |

Install basics:

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install pandas
```

Install Centaur Technical Indicators (assuming published wheel / build locally):

```bash
pip install centaur_technical_indicators
# OR (from source after cloning)
# pip install maturin
# maturin develop
```

---

## 🧱 Library Structure (Quick Recap)

Modules are grouped by analysis area:

- `momentum_indicators`
- `trend_indicators`
- `strength_indicators`
- `volatility_indicators`
- `candle_indicators`
- `chart_trends`
- `correlation_indicators`
- `other_indicators`

Each module has two namespaces:

- `single`: operates on the entire list (returns one value or a tuple)
- `bulk`: rolling / windowed computation (returns a list aligned to the trailing end of each window)

Example:
```python
from centaur_technical_indicators import moving_average

prices = [1234, 1278, 1295, ... , 1300]
moving_average.single.moving_average(prices, moving_average_type="simple")
moving_average.bulk.moving_average(prices, moving_average_type="simple", period=20)
```

---

## 📥 Getting Some Data 

If you already have a CSV point to your CSV, we will be using `prices.csv` for our tutorials.

You can find a copyable sample `prices.csv` file [here](../examples/index.md).

```python
import pandas as pd 

df = pd.read_csv("prices.csv", parse_dates=["Date"])
df = df.sort_values("Date").reset_index(drop=True)
```

---

## 🔄 Converting DataFrame Columns to Lists

Most functions expect Python `list[float]` (not Series, not NumPy arrays). Convert cleanly:

```python
close = df["Close"].astype(float).tolist()
high  = df["High"].astype(float).tolist()
low   = df["Low"].astype(float).tolist()
open_ = df["Open"].astype(float).tolist()
```

> You can keep naming consistent; `open_` avoids shadowing Python's built-in `open`

---

## ✅ Example 1: Simple Moving Average (SMA)

[API Reference](../api/moving-average.md)

### Single (all data as one window)
```python
from centaur_technical_indicators import moving_average as ma

sma_all = ma.single.moving_average(close, moving_average_type="simple")
print("Full-series SMA:", sma_all)
```

### Bulk (rolling window)
```python
period = 20
sma_series = ma.bulk.moving_average(close, period=period, moving_average_type="simple")
# Align back to DataFrame: last len(sma_series) rows correspond to rolling results
df.loc[df.index[-len(sma_series):], f"SMA_{period}"] = sma_series
```

Tip: The first `(period - 1)` rows have no value because a full window was not yet available. You can forward fill or leave as NaN depending on downstream needs.

---

## ✅ Example 2: Moving Constant Bands (Generic Bollinger Bands)

[API Reference](../api/candle-indicators.md)

`single` version returns a tuple `(lower, middle, upper)`; `bulk` returns a list of such tuples.

```python
lower, ema, upper = ci.single.moving_constant_bands(
        close[-20:],
        constant_model_type="exponential_moving_average",
        deviation_model="standard_deviation",
        deviation_multiplier=2.0,
)
print(f"lower band: {lower}, EMA: {ema}, upper band: {upper}")
```

Bulk usage:

```python
bands = ci.bulk.moving_constant_bands(
    close,
    constant_model_type="exponential_moving_average",
    deviation_model="standard_deviation",
    deviation_multiplier=2.0,
    period=20
)

# Unpack and assign (align to tail)
lower_vals, ema_vals, upper_vals = zip(*bands)
tail_index = df.index[-len(bands):]
df.loc[tail_index, "MCB_Lower"] = lower_vals
df.loc[tail_index, "MCB_EMA"] = ema_vals
df.loc[tail_index, "MCB_Upper"] = upper_vals
```

---

## ✅ Example 3: Relative Strength Index (RSI)

[API Reference](../api/momentum-indicators.md)

```python
rsi_values =  mi.bulk.relative_strength_index(
    close,
    constant_model_type="smoothed_moving_average",
    period=20
)

df.loc[df.index[-len(rsi_values):], "RSI"] = rsi_values
```

---

## ✅ Example 4: Average True Range (ATR)

[API Reference](../api/other-indicators.md)

Valid constant model strings (case-insensitive aliases):  
`simple`, `smoothed`, `exponential`, `median`, `mode`  
(Full forms also accepted: `simple_moving_average`, `smoothed_moving_average`, etc.)

```python
from centaur_technical_indicators import other_indicators

model = "exponential_moving_average"
period = 14

atr_series = other_indicators.bulk.average_true_range(
    close=close,
    high=high,
    low=low,
    constant_model_type=model,
    period=period
)
df.loc[df.index[-len(atr_series):], f"ATR_{period}"] = atr_series
```

---

## 🧪 Single vs Bulk: When to Use Which?

| Use Case | Pick |
|----------|------|
| One summary value for entire dataset | `single` |
| Rolling feature engineering for models | `bulk` |
| Signal snapshot at latest bar | `single` on a sliced tail |
| Chart overlays / time series indicators | `bulk` |

---

## 🛠️ Handling Alignment

Bulk outputs are shorter than the original price history when a window (`period`) is specified. Strategies:

```python
# Method 1: Right-align (as shown earlier)
df.loc[df.index[-len(series):], "Indicator"] = series

# Method 2: Pad with NaN at front
import numpy as np
padded = [np.nan] * (len(df) - len(series)) + series
df["Indicator"] = padded
```

---

## 🧪 Missing Data / NaNs

Before extracting lists:

```python
df = df.dropna(subset=["Open", "High", "Low", "Close", "Volume"])
```

If you forward-fill:

```python
df[["Open","High","Low","Close","Volume"]] = df[["Open","High","Low","Close","Volume"]].ffill()
```

Be cautious: forward-filling can create artificial continuity.

---

## ⚙️ Choosing Constant / Deviation Models

Many functions accept a `constant_model_type` or `deviation_model` to alter smoothing or dispersion behavior.  
Experiment:

Consult the project [API reference](../api/index.md) for model-specific effects:

---

## 🧩 Putting It All Together

A runnable example of the full code can be found in the [full pandas example](../examples/tutorial_pandas.md)



---

## 🚀 Performance Tips

- Convert Series to plain Python lists only once; reuse.
- Gather all indicators first, then assign to DataFrame to minimize index alignment overhead.
- For large backtests, consider chunking historical data and streaming features forward.

---

## 🧾 Common Pitfalls

| Issue | Cause | Fix |
|-------|-------|-----|
| ValueError for model type | Misspelled string | Use accepted aliases (`simple`, `exponential`, etc.) |
| Mismatched lengths when merging | Rolling window shrinks output | Right-align or pad with NaNs |
| NaNs propagate into indicators | Missing OHLC data | Clean with `dropna` or controlled fills |
| Confusing `single` vs `bulk` | Wrong namespace | Use `module.single.*` or `module.bulk.*` |

---

## 🛡️ Disclaimer

Educational example only. Not financial advice. Validate results independently before using in production or live trading.

---

## ✅ Next Step

You now know how to integrate Centaur Technical Indicators into a pandas workflow and engineer feature columns efficiently.

Next discover how to integrate [Plotly charts](./plotly.md)

---

Happy analyzing! 🦀🐍📈
