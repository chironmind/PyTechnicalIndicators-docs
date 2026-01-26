# How to choose the best deviation model example

A full example of programmatically determining the best deviation model for Moving Constant Bands
from [the how-to guide](../howto/choose-deviation-model.md).

## Full code

```python
import csv
import sys
from centaur_technical_indicators import candle_indicators as ci


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


def choose_best_deviation_model(
    prices: list[float],
    constant_model_type: str = "exponential_moving_average",
    deviation_multiplier: float = 2.0,
    period: int = 5,
) -> tuple[str, float]:
    deviation_models = [
        "standard_deviation",
        "mean_absolute_deviation",
        "median_absolute_deviation",
        "mode_absolute_deviation",
        "ulcer_index",
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
        constant_model_type="exponential_moving_average",
        deviation_multiplier=2.0,
        period=5,
    )
    print(f"Best model for moving constant bands is {best_model} with a rating of {best_rating}")


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
Best model for moving constant bands is median_absolute_deviation with a rating of 0.6666666666666666
```

Note: The best model and rating will vary based on your prices.csv data.
