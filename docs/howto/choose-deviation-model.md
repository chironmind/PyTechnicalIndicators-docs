# How to determine the best `deviation_model` for a Centaur Technical Indicators function

This guide shows how to programmatically determine the best `deviation_model` for your indicator using the Python package Centaur Technical Indicators.

The rating model is overly simplified and should be refined to suit your needs before usage.

---

## 🎯 Goal

- Determine the best `deviation_model` for the Moving Constant Bands from a year of data

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

This example expects a CSV file with either

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

### 2. Calculate moving constant bands for multiple deviation models

The default deviation model is the standard deviation; however other models may provide more insight.

We’ll test several deviation models while keeping the constant model fixed (e.g., exponential moving average), a deviation multiplier (e.g., 2.0), and a short period (e.g., 5) for illustration.

```python
from centaur_technical_indicators import candle_indicators as ci

period = 5
deviation_multiplier = 2.0
constant_model = "exponential"  # or "exponential_moving_average"

deviation_models = [
    "standard",  # or "standard_deviation"
    "mean",      # or "mean_absolute_deviation"
    "median",    # or "median_absolute_deviation"
    "mode",      # or "mode_absolute_deviation"
    "ulcer",     # or "ulcer_index"
]

# Example:
# bands = ci.bulk.moving_constant_bands(prices, constant_model, dev_model, deviation_multiplier, period)
```

### 3. Rate each model to find the best

> The logic is overly simple for the purpose of the guide.

- If price > upper band (overbought) and next price < current price, model gets +1
- If price < lower band (oversold) and next price > current price, model gets +1

We normalize the score by the number of "attempts" (how many times we evaluated a signal).

Important: `moving_constant_bands` returns tuples ordered as (`lower_band`, `moving_constant`, `upper_band`). Align the band window to the price index i using `bands_idx = i - (period - 1)`.

```python
def choose_best_deviation_model(
    prices: list[float],
    constant_model_type: str = "exponential",
    deviation_multiplier: float = 2.0,
    period: int = 5,
) -> tuple[str, float]:
    from centaur_technical_indicators import candle_indicators as ci

    deviation_models = [
        "standard",  # or "standard_deviation"
        "mean",      # or "mean_absolute_deviation"
        "median",    # or "median_absolute_deviation"
        "mode",      # or "mode_absolute_deviation"
        "ulcer",     # or "ulcer_index"
    ]

    best_rating = -1.0
    best_model = deviation_models[0]

    for dm in deviation_models:
        bands = ci.bulk.moving_constant_bands(
            prices,
            constant_model_type,
            dm,
            deviation_multiplier,
            period,
        )
        # bands[k] corresponds to price index i = k + (period - 1)

        current_rating = 0.0
        attempts = 0.0

        for i in range(period - 1, len(prices) - 1):
            bands_idx = i - (period - 1)
            lower_band, _, upper_band = bands[bands_idx]

            # Overbought: expect next price to fall
            if prices[i] > upper_band:
                attempts += 1.0
                if prices[i + 1] < prices[i]:
                    current_rating += 1.0

            # Oversold: expect next price to rise
            if prices[i] < lower_band:
                attempts += 1.0
                if prices[i + 1] > prices[i]:
                    current_rating += 1.0

        average_rating = (current_rating / attempts) if attempts > 0 else 0.0
        if average_rating > best_rating:
            best_rating = average_rating
            best_model = dm

    return best_model, best_rating
```

### 4. Calling from `main`

```python
import sys

# Reuse load_prices_from_csv and choose_best_deviation_model from above

def main():
    if len(sys.argv) < 2:
        print("Usage: python choose_deviation_model.py <path_to_csv>")
        sys.exit(1)

    csv_path = sys.argv[1]
    prices = load_prices_from_csv(csv_path)

    print(f"Loaded {len(prices)} prices")

    if len(prices) < 5:
        print("Not enough data to compute moving constant bands with period 5.")
        sys.exit(1)

    best_model, best_rating = choose_best_deviation_model(
        prices,
        constant_model_type="exponential",  # keep constant model fixed for this search
        deviation_multiplier=2.0,
        period=5,
    )
    print(f"Best model for moving constant bands is {best_model} with a rating of {best_rating}")

if __name__ == "__main__":
    main()
```

---

## 🧩 Putting It All Together

A full runnable example of the code can be found in the  [choose deviation model example](../examples/howto_choose_deviation_model.md).


---

## ✅ Next Steps

- Programmatically choose a period
- Programmatically choose a `constant_model_type`
- Programmatically choose a deviation multiplier
- Combine all selections
- Introduce the notion of punishment to the rating system (e.g., penalize false signals/whipsaws)

---

