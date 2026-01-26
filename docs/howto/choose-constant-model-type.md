# How to determine the best `constant_model_type` for a Centaur Technical Indicators function

This guide shows how to programmatically determine the best `constant_model_type` for your indicator using the Python package Centaur Technical Indicators.

The rating model is overly simplified and should be refined to suit your needs before usage.

---

## 🎯 Goal

- Determine the best `constant_model_type` for the RSI from a year of data

> This guide assumes you can load price data from a CSV file (see the "Get data from CSV" section below).

---

## 📦 Requirements

Install Centaur Technical Indicators:

```bash
pip install centaur_technical_indicators
```

---

## 💻 Step-by-Step

### 1. Get data from CSV

This example expects a CSV file with either:

- a header containing a "close" column (case-insensitive), or
- a single column of prices (no header)

Example loader:

```python
import csv

def load_prices_from_csv(path: str) -> list[float]:
    prices: list[float] = []
    with open(path, "r", newline="") as f:
        # Try to read as DictReader first (with a header)
        sample = f.read(4096)
        f.seek(0)
        has_header = "," in sample and any(ch.isalpha() for ch in sample.splitlines()[0])
        if has_header:
            reader = csv.DictReader(f)
            # Find a 'close' column in a case-insensitive way
            close_key = None
            if reader.fieldnames:
                for k in reader.fieldnames:
                    if k and k.lower() == "close":
                        close_key = k
                        break
            if close_key is None and reader.fieldnames:
                # Fallback to the first column if 'close' not found
                close_key = reader.fieldnames[0]
            for row in reader:
                try:
                    prices.append(float(row[close_key]))
                except (ValueError, TypeError, KeyError):
                    continue
        else:
            # No header: read first column only
            f.seek(0)
            reader = csv.reader(f)
            for row in reader:
                if not row:
                    continue
                try:
                    prices.append(float(row[0]))
                except ValueError:
                    continue
    return prices
```

### 2. Calculate the RSI for multiple constant models

The default model for the RSI is a smoothed moving average.

We’ll test several models and compute the RSI series for each using the bulk function.

```python
from centaur_technical_indicators import momentum_indicators as mi

period = 14
models = [
    "simple_moving_average",
    "smoothed_moving_average",
    "exponential_moving_average",
    "simple_moving_median",
    "simple_moving_mode",
]

# Example: prices = load_prices_from_csv("data.csv")
# rsi_series = mi.bulk.relative_strength_index(prices, model, period)
```

### 3. Rate each RSI to find the best

> The logic is overly simple for the purpose of the guide.

- If RSI > 70 (overbought) and the next price < current price, the model gets +1
- If RSI < 30 (oversold) and the next price > current price, the model gets +1

We normalize the score by the number of "attempts" (how many times we evaluated a signal).

```python
def choose_best_rsi_model(prices: list[float], period: int = 14) -> tuple[str, float]:
    from centaur_technical_indicators import momentum_indicators as mi

    models = [
        "simple_moving_average",
        "smoothed_moving_average",
        "exponential_moving_average",
        "simple_moving_median",
        "simple_moving_mode",
    ]

    best_rating = -1.0
    best_model = models[0]

    for m in models:
        rsi = mi.bulk.relative_strength_index(prices, m, period)

        current_rating = 0.0
        attempts = 0.0

        # rsi[0] corresponds to prices[period], so align indices accordingly
        for i in range(period, len(prices) - 1):
            rsi_val = rsi[i - period]

            # Overbought condition: expect price drop next step
            if rsi_val > 70.0:
                attempts += 1.0
                if prices[i + 1] < prices[i]:
                    current_rating += 1.0

            # Oversold condition: expect price rise next step
            if rsi_val < 30.0:
                attempts += 1.0
                if prices[i + 1] > prices[i]:
                    current_rating += 1.0

        average_rating = (current_rating / attempts) if attempts > 0 else 0.0
        if average_rating > best_rating:
            best_rating = average_rating
            best_model = m

    return best_model, best_rating
```

### 4. Full example

```python
import sys
from centaur_technical_indicators import momentum_indicators as mi

# Reuse load_prices_from_csv and choose_best_rsi_model from above

def main():
    if len(sys.argv) < 2:
        print("Usage: python choose_constant_model_type.py <path_to_csv>")
        sys.exit(1)

    csv_path = sys.argv[1]
    prices = load_prices_from_csv(csv_path)

    print(f"Loaded {len(prices)} prices")

    if len(prices) < 15:
        print("Not enough data to compute RSI with period 14.")
        sys.exit(1)

    best_model, best_rating = choose_best_rsi_model(prices, period=14)
    print(f"Best model for RSI is {best_model} with a rating of {best_rating}")

if __name__ == "__main__":
    main()
```

---

## 🧩 Putting It All Together

A full runnable example of the code can be found in the [choose constant model example](../examples/howto_choose_constant_model_type.md).

---

## ✅ Next Steps

- Programmatically choose a period (e.g., grid search over common RSI periods like 7, 14, 21)
- Combine period selection and constant model selection
- Introduce the notion of punishment to the rating system (e.g., penalize false signals or whipsaws)


