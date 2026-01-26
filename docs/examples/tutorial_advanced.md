# Advanced Usage - Systematically Evaluating RSI Variants example

A full example of systematically comparing different RSI configurations
from [the tutorial](../tutorials/advanced.md).

## Full code

```python
import pandas as pd
from centaur_technical_indicators import momentum_indicators as mi


# Load and prepare data
df = pd.read_csv("prices.csv", parse_dates=["Date"]).sort_values("Date").reset_index(drop=True)
df = df.dropna(subset=["Open","High","Low","Close"])
close = df["Close"].astype(float).tolist()

# Define variant dimensions
CONSTANT_MODELS = [
    "simple_moving_average",
    "smoothed_moving_average",
    "exponential_moving_average",
    "simple_moving_median",
    "simple_moving_mode"
]
RSI_PERIOD = 5
OVERSOLD = 30.0

# Compute RSI variants and rate signals
results = []

for ctype in CONSTANT_MODELS:
    # Compute RSI for this model type
    rsi_vals = mi.bulk.relative_strength_index(
        close,
        constant_model_type=ctype,
        period=RSI_PERIOD
    )
    # The output is aligned to the tail, so index mapping:
    start_idx = len(close) - len(rsi_vals)
    rating = 0
    total_signals = 0
    for offset, rsi in enumerate(rsi_vals[:-1]):  # skip last (no future bar)
        idx = start_idx + offset
        price = close[idx]
        if rsi < OVERSOLD:
            total_signals += 1
            next_price = close[idx + 1]
            if next_price > price:
                rating += 1
    success_rate = rating / total_signals if total_signals > 0 else 0.0
    results.append({
        "model": ctype,
        "period": RSI_PERIOD,
        "signals": total_signals,
        "correct_signals": rating,
        "success_rate": success_rate
    })

# Present the results
score_df = pd.DataFrame(results)
score_df = score_df.sort_values("success_rate", ascending=False)

print("RSI Model Ratings (RSI < 30, period=5):")
print(score_df[["model", "period", "signals", "correct_signals", "success_rate"]])

best = score_df.iloc[0]
print(f"\nBest model: {best['model']} (Success Rate: {best['success_rate']:.2%})")
```

## Run the code

```shell
python3 your_file_name.py
```

## Expected output

```shell
RSI Model Ratings (RSI < 30, period=5):
                       model  period  signals  correct_signals  success_rate
3        simple_moving_median       5       43               24      0.558140
0   simple_moving_average       5       38               20      0.526316
1     smoothed_moving_average       5       41               21      0.512195
4         simple_moving_mode       5       41               21      0.512195
2  exponential_moving_average       5       46               22      0.478261

Best model: simple_moving_median (Success Rate: 55.81%)
```
