# How to choose the best constant model type example

A full example of programmatically determining the best constant model type for RSI
from [the how-to guide](../howto/choose-constant-model-type.md).

## Full code

```python
import csv
import sys
from centaur_technical_indicators import momentum_indicators as mi


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


def choose_best_rsi_model(prices: list[float], period: int = 14) -> tuple[str, float]:
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

## Run the code

```shell
python3 your_file_name.py prices.csv
```

## Expected output

```text
Loaded 251 prices
Best model for RSI is simple_moving_average with a rating of 0.5625
```

Note: The best model and rating will vary based on your prices.csv data.
