# Using Centaur Technical Indicators with pandas example

A full example of using Centaur Technical Indicators with Pandas DataFrames 
from [the tutorial](../tutorials/pandas.md).

## Full code

```python
import pandas as pd
from centaur_technical_indicators import moving_average as ma
from centaur_technical_indicators import candle_indicators as ci
from centaur_technical_indicators import momentum_indicators as mi
from centaur_technical_indicators import other_indicators as oi


df = pd.read_csv("prices.csv", parse_dates=["Date"])
df = df.sort_values("Date").reset_index(drop=True)

close = df["Close"].astype(float).tolist()
high  = df["High"].astype(float).tolist()
low   = df["Low"].astype(float).tolist()
open_ = df["Open"].astype(float).tolist()

# Single simple moving average
sma_all = ma.single.moving_average(close, moving_average_type="simple")
print("Full-series SMA:", sma_all)

# Bulk simple moving average (20-period)
period = 20
sma_series = ma.bulk.moving_average(close, period=period, moving_average_type="simple")
# Align back to DataFrame: last len(sma_series) rows correspond to rolling results
df.loc[df.index[-len(sma_series):], f"SMA_{period}"] = sma_series

# Single Bollinger Bands
lower, ema, upper = ci.single.moving_constant_bands(
        close[-20:],
        constant_model_type="exponential_moving_average",
        deviation_model="standard_deviation",
        deviation_multiplier=2.0,
)
print(f"lower band: {lower}, EMA: {ema}, upper band: {upper}")

# Bulk Bollinger Bands (20-period)
bands = ci.bulk.moving_constant_bands(
    close,
    constant_model_type="exponential_moving_average",
    deviation_model="standard_deviation",
    deviation_multiplier=2.0,
    period=period
)

# Unpack and assign (align to tail)
lower_vals, ema_vals, upper_vals = zip(*bands)
tail_index = df.index[-len(bands):]
df.loc[tail_index, "MCB_Lower"] = lower_vals
df.loc[tail_index, "MCB_EMA"] = ema_vals
df.loc[tail_index, "MCB_Upper"] = upper_vals

# Bulk RSI
rsi_values =  mi.bulk.relative_strength_index(
    close,
    constant_model_type="smoothed_moving_average",
    period=20
)

df.loc[df.index[-len(rsi_values):], "RSI"] = rsi_values

# Bulk ATR
atr_series = oi.bulk.average_true_range(
    close=close,
    high=high,
    low=low,
    constant_model_type="exponential_moving_average",
    period=period
)
df.loc[df.index[-len(atr_series):], f"ATR_{period}"] = atr_series

```

## Run the code

```shell
python3 your_file_name.py
```

## Expected output

```shell
Full-series SMA: 5624.867410358566
5367.04826935573 5755.587345685254 6144.126422014779
          Date     Open     High      Low    Close     SMA_20     BB_Lower    BB_Middle     BB_Upper        RSI   ATR_20
246 2025-03-10  5705.37  5705.37  5564.02  5614.56  5956.2700  5579.906354  5877.649008  6175.391662  33.281207  84.9520
247 2025-03-11  5603.65  5636.30  5528.41  5572.07  5931.5515  5504.588851  5841.191158  6177.793465  36.744063  88.9195
248 2025-03-12  5624.84  5642.19  5546.09  5599.30  5908.0915  5451.413841  5811.173289  6170.932737  33.949163  92.0275
249 2025-03-13  5594.45  5597.78  5504.65  5521.52  5881.5690  5385.361698  5775.695387  6166.029075  33.043555  93.6840
250 2025-03-14  5563.85  5645.27  5563.85  5638.94  5857.7625  5367.048269  5755.587346  6144.126422  39.573869  94.4570
```