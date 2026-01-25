# Bulk vs Single functions example

A full example showing when to use bulk vs single functions
from [the how-to guide](../howto/bulk-vs-single.md).

## Full code

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
print(f"Number of RSI values: {len(rsi_series)}")

# 2) Single: compute the next/latest RSI when a new price arrives
new_price = 5769.21
data.append(new_price)

# For period=14, use the latest 14 prices for single RSI
latest_window = data[-period:]
latest_rsi = mi.single.relative_strength_index(latest_window, model)
print(f"\nSingle RSI (after adding new price {new_price}): {latest_rsi}")

# Summary
print("\nSummary:")
print("- Use bulk when: calculating many historical values, initial setup, backtesting")
print("- Use single when: real-time updates, streaming data, memory constraints")
```

## Run the code

```shell
python3 your_file_name.py
```

## Expected output

```text
Bulk RSIs: [47.49434607156126, 50.3221945432267, 53.10086655617841, 54.59653302622837, 54.02834042612616, 53.1831838849601, 50.43337050636098, 50.73062988169513, 53.35617832890983, 54.45176738116544, 58.05765013037176, 56.30853084866037, 57.96857486166298, 57.84082988859398, 58.99843084091084, 56.57854806887618, 53.67089033073862, 52.03030103916983, 51.65074453025388, 51.20085069336844, 48.80949868867423, 50.59064094048877, 46.24896653055863, 45.40743831024948, 42.06832313989923, 41.20267034598993, 40.55468800662056, 39.766991673668226, 35.93370024331353, 33.281206596808124, 36.744062574073495, 33.94916340032853, 33.043555061639476, 39.57386902889889, 40.34609500741716]
Number of RSI values: 35

Single RSI (after adding new price 5769.21): 48.00106962275036

Summary:
- Use bulk when: calculating many historical values, initial setup, backtesting
- Use single when: real-time updates, streaming data, memory constraints
```

## Key takeaways

- **Bulk functions** return a list of values (one for each window in the data)
- **Single functions** return a single value (for the current window)
- Use bulk for historical analysis and backtesting
- Use single for real-time streaming data updates
