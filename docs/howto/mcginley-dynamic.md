# How to use McGinley Dynamic in PyTechnicalIndicators

This guide shows how to use the McGinley Dynamic bands with the Python package PyTechnicalIndicators.  
The same logic can be applied to other McGinley Dynamic functions.

---

## 🎯 Goal

- Use the McGinley Dynamic bands

> This guide assumes you can load price data from a CSV file (see the "Get data from CSV" section below).

---

## 📦 Requirements

Install PyTechnicalIndicators:

```bash
pip install pytechnicalindicators
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

### 2. Calculate the McGinley Dynamic bands

The McGinley Dynamic uses a previously calculated value.  
If no previous McGinley Dynamic is available, use 0.0 for the first computation.

```python
from pytechnicalindicators import candle_indicators as ci

# Example setup
period = 5
deviation_model = "standard"     # or "standard_deviation"
deviation_multiplier = 2.0
previous_mcginley_dynamic = 0.0  # none available on first run

# prices = load_prices_from_csv("data.csv")
bands = ci.bulk.mcginley_dynamic_bands(
    prices,
    deviation_model,
    deviation_multiplier,
    previous_mcginley_dynamic,
    period,
)

print("Loaded", len(prices), "prices")
print("Length of bands", len(bands))
# bands is a list of tuples: (lower_band, mcginley_dynamic, upper_band)
```

### 3. Use the last value to calculate the next McGinley band

From step 2, we now have a previous McGinley Dynamic (the middle value of the last tuple).  
When a new price comes in, compute the next band using the single function.  
Pass the latest period prices to align with the period used in bulk.

```python
# Next price comes in
new_price = 5689.24
prices.append(new_price)

last_mcginley_dynamic = bands[-1][1]  # previous McGinley Dynamic from bulk result

# Use the last 'period' prices for the single calculation
latest_window = prices[-period:]

next_band = ci.single.mcginley_dynamic_bands(
    latest_window,
    deviation_model,
    deviation_multiplier,
    last_mcginley_dynamic,
)

lower, mid, upper = next_band
print(f"Lower band {lower}, McGinley dynamic {mid}, upper band {upper}")
```

---

## 🧪 Output

A runnable sample of the code can be found in [`mcginley_dynamic.py`](https://github.com/chironmind/PyTechnicalIndicators-How-To-guides/blob/main/examples/mcginley_dynamic.py)

```text
Loaded 251 prices
Length of bands 247
Lower band 5551.313227907162, McGinley dynamic 5665.614575300795, upper band 5779.9159226944275
```

---

## ✅ Next Steps

- Programmatically choose a period
- Programmatically choose a `deviation_model`
- Programmatically choose a deviation multiplier
- Combine all selections
- Introduce the notion of punishment to the rating system (e.g., penalize false signals/whipsaws)

