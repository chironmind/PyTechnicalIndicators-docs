# How to choose the best period example

A full example of programmatically determining the best period for RSI
from [the how-to guide](../howto/choose-period.md).

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


def choose_best_rsi_period(prices: list[float], min_p: int = 2, max_p: int = 15) -> tuple[int, float]:
    best_rating = -1.0
    best_period = min_p
    model = "smoothed_moving_average"

    for p in range(min_p, max_p + 1):
        rsi = mi.bulk.relative_strength_index(prices, model, p)

        current_rating = 0.0
        attempts = 0.0

        # rsi[0] aligns with prices index i = p
        for i in range(p, len(prices) - 1):
            rsi_val = rsi[i - p]

            # Overbought: expect price to fall next step
            if rsi_val > 70.0:
                attempts += 1.0
                if prices[i + 1] < prices[i]:
                    current_rating += 1.0

            # Oversold: expect price to rise next step
            if rsi_val < 30.0:
                attempts += 1.0
                if prices[i + 1] > prices[i]:
                    current_rating += 1.0

        average_rating = (current_rating / attempts) if attempts > 0 else 0.0
        if average_rating > best_rating:
            best_rating = average_rating
            best_period = p

    return best_period, best_rating


def main():
    if len(sys.argv) < 2:
        print("Usage: python choose_period.py <path_to_csv>")
        sys.exit(1)

    csv_path = sys.argv[1]
    prices = load_prices_from_csv(csv_path)

    print(f"Loaded {len(prices)} prices")

    if len(prices) < 16:
        print("Not enough data to evaluate RSI periods up to 15.")
        sys.exit(1)

    best_period, best_rating = choose_best_rsi_period(prices, min_p=2, max_p=15)
    print(f"Best period for RSI is {best_period} with a rating of {best_rating}")


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
Best period for RSI is 7 with a rating of 0.5555555555555556
```

Note: The best period and rating will vary based on your prices.csv data.
