# When to choose bulk vs single functions in Centaur Technical Indicators

This guide shows when to choose the bulk version of a function or the single version of a function using the Python package Centaur Technical Indicators.

---

## 🎯 Goal

- Understand when to use bulk or single functions

---

## 📦 Requirements

Install Centaur Technical Indicators:

```bash
pip install centaur_technical_indicators
```

---

## 💻 Step-by-Step

### 1. What period are you using?

Determine how much time (period) you want to include when computing the indicator (e.g., RSI period = 14).

### 2. Observe the data

Do you have just enough data to cover the period?

- If yes, you’ll only be able to calculate a single value for the indicator.

Do you have extra data?

- If yes, you’ll be able to calculate multiple values over time (historical series).

### 3. What is your goal?

Will previous values for the indicator help you make a decision?

- If yes, make sure you collect enough data to compute previous values (use bulk).
- If no (e.g., streaming updates), compute just the latest value (use single).

### 4. Example (RSI)

The default RSI often uses 14 previous prices. If you have 53 days of data you can:

- Use the bulk function to calculate the historical RSIs.
- When a new price comes in, use the single function on the latest window.

```python
from centaur_technical_indicators import momentum_indicators as mi

# 53 example closing prices
data = [
    6037.59, 5970.84, 5906.94, 5881.63, 5868.55, 5942.47, 5975.38, 5909.03,
    5918.25, 5827.04, 5836.22, 5842.91, 5949.91, 5937.34, 5996.66, 6049.24,
    6086.37, 6118.71, 6101.24, 6012.28, 6067.70, 6039.31, 6071.17, 6040.53,
    5994.57, 6037.88, 6061.48, 6083.57, 6025.99, 6066.44, 6068.50, 6051.97,
    6115.07, 6114.63, 6129.58, 6144.15, 6117.52, 6013.13, 5983.25, 5955.25,
    5956.06, 5861.57, 5954.50, 5849.72, 5778.15, 5842.63, 5738.52, 5770.20,
    5614.56, 5572.07, 5599.30, 5521.52, 5638.94
]

period = 14
model = "smoothed_moving_average"  # options: "simple_moving_average", "smoothed_moving_average",
                                   #          "exponential_moving_average", "simple_moving_median",
                                   #          "simple_moving_mode"

# 1) Bulk: compute historical RSI values
rsi_series = mi.bulk.relative_strength_index(data, model, period)
print("Bulk RSIs:", rsi_series)

# 2) Single: compute the next/latest RSI when a new price arrives
new_price = 5769.21
data.append(new_price)

# For period=14, use the latest 14 prices for single RSI
latest_window = data[-period:]
latest_rsi = mi.single.relative_strength_index(latest_window, model)
print("Single RSI:", latest_rsi)
```

Notes:

- Valid `constant_model_type` values:
  - `simple_moving_average`
  - `smoothed_moving_average`
  - `exponential_moving_m_average`
  - `simple_moving_median`
  - `simple_moving_mode`

---

## 🧪 Example Output

A runnable sample of the code can be found in [`bulk_vs_single.py`](https://github.com/chironmind/CentaurTechnicalIndicators-How-To-guides/blob/main/examples/bulk_vs_single.py)

```text
Bulk RSIs: [47.49434607156126, 50.3221945432267, ..., 40.34609500741716]
Single RSI: 48.00106962275036
```

- Use bulk when: calculating many historical values, initial setup, backtesting
- Use single when: real-time updates, streaming data, memory constraints
