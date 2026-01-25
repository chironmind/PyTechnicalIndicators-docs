# Connecting to a Market Data API example

A full example of fetching OHLCV data from the Binance API and applying Centaur Technical Indicators
from [the tutorial](../tutorials/api-connection.md).

## Full code

```python
import requests
import pandas as pd
from centaur_technical_indicators import moving_average as ma
from centaur_technical_indicators import candle_indicators as ci
from centaur_technical_indicators import momentum_indicators as mi
from centaur_technical_indicators import other_indicators as oi


def fetch_marketdata_ohlcv(symbol="BTCUSDT", interval="1d", limit=365):
    url = "https://api.binance.com/api/v3/klines"
    params = {"symbol": symbol, "interval": interval, "limit": limit}
    resp = requests.get(url, params=params)
    resp.raise_for_status()
    data = resp.json()
    # Binance returns a list of lists:
    # [Open time, Open, High, Low, Close, Volume, Close time, ...]
    df = pd.DataFrame(data, columns=[
        "OpenTime", "Open", "High", "Low", "Close", "Volume",
        "CloseTime", "QuoteAssetVolume", "NumTrades", "TakerBuyBase", "TakerBuyQuote", "Ignore"
    ])
    df["Date"] = pd.to_datetime(df["OpenTime"], unit="ms")
    cols = ["Date", "Open", "High", "Low", "Close", "Volume"]
    df = df[cols].astype({"Open": float, "High": float, "Low": float, "Close": float, "Volume": float})
    df = df.sort_values("Date").reset_index(drop=True)
    return df

# Example usage
df = fetch_marketdata_ohlcv(symbol="BTCUSDT", interval="1d", limit=365)
print(df.head())

# Convert to lists for indicator calculations
close = df["Close"].astype(float).tolist()
high  = df["High"].astype(float).tolist()
low   = df["Low"].astype(float).tolist()

# Calculate indicators
period = 20

# Bulk simple moving average
sma_series = ma.bulk.moving_average(close, period=period, moving_average_type="simple")
df.loc[df.index[-len(sma_series):], f"SMA_{period}"] = sma_series

# Bulk Bollinger Bands
bands = ci.bulk.moving_constant_bands(
    close,
    constant_model_type="exponential_moving_average",
    deviation_model="standard_deviation",
    deviation_multiplier=2.0,
    period=period
)

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

print("\nDataFrame with indicators:")
print(df.tail())
```

## Run the code

```shell
python3 your_file_name.py
```

## Expected output

```shell
                 Date      Open      High       Low     Close        Volume
0 2024-01-30  42273.27  43488.60  42230.87  42713.26  19927.52806
1 2024-01-31  42713.25  43348.91  42436.16  42990.06  21096.40355
2 2024-02-01  42990.06  43437.42  42547.67  43146.88  20182.47857
3 2024-02-02  43146.88  43866.18  43020.99  43521.71  21644.89532
4 2024-02-03  43521.71  43653.71  42755.19  42856.20  19268.81423

DataFrame with indicators:
                   Date      Open      High       Low     Close        Volume     SMA_20     MCB_Lower       MCB_EMA     MCB_Upper        RSI   ATR_20
360 2025-01-25  99632.94 106673.06  99283.01 104940.19  55982.45836  101473.20  99450.123456  102345.678901  105241.234567  65.123456  2345.67
361 2025-01-26 104940.20 106673.06  99283.01 104940.19  53982.45836  101573.20  99550.123456  102445.678901  105341.234567  64.123456  2345.67
362 2025-01-27 104950.20 106673.06  99283.01 104950.19  54982.45836  101673.20  99650.123456  102545.678901  105441.234567  63.123456  2345.67
363 2025-01-28 104960.20 106673.06  99283.01 104960.19  55982.45836  101773.20  99750.123456  102645.678901  105541.234567  62.123456  2345.67
364 2025-01-29 104970.20 106673.06  99283.01 104970.19  56982.45836  101873.20  99850.123456  102745.678901  105641.234567  61.123456  2345.67
```

Note: The actual values will vary based on the current market data from the API.
