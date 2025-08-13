# 🔗 Correlation Indicators API Reference

**Module:** `correlation_indicators`  

The `correlation_indicators` module provides functions to measure the co-movement and statistical relationship between two different price series or assets.

## 📚 When to Use

Use correlation indicators when you want to:
- Quantify how closely two assets move together
- Assess diversification or hedging effectiveness
- Explore relationships between assets

## 🏗️ Structure

- **single**: Functions that return a single value for a slice of prices.
- **bulk**: Functions that compute values of a slice of prices over a period and return a vector.

---

## 🚀 Bulk Functions

Bulk functions operate over a moving window and return a list of correlation values for the entire data series.

### `correlate_asset_prices`
```python
correlate_asset_prices(
    prices_asset_a: List[float],
    prices_asset_b: List[float],
    constant_model_type: str,
    deviation_model: str,
    period: int
) -> List[float]
```

#### Arguments
- **prices_asset_a**: List of prices for asset A
- **prices_asset_b**: List of prices for asset B
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **deviation_model**: Choice of:
    - `"standard_deviation"`
    - `"mean_absolute_deviation"`
    - `"median_absolute_deviation"`
    - `"mode_absolute_deviation"`
    - `"ulcer_index"`
- **period**: Period over which to calculate the correlation

#### Returns
List of correlations for each period

#### Example
```python
from pytechnicalindicators import correlation_indicators as ci

prices_asset_a = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

prices_asset_b = [
    50.0, 51.0, 52.5, 51.5, 53.0, 54.0, 53.5, 55.0,
    56.0, 55.5, 56.5, 57.0, 58.0, 57.5, 58.5
]

correlations = ci.bulk.correlate_asset_prices(
    prices_asset_a,
    prices_asset_b,
    constant_model_type="simple_moving_average",
    deviation_model="standard_deviation",
    period=5
)

print(f"Bulk Correlations: {correlations}")
```

**Output:**
```
Bulk Correlations: [
    1.0, 1.0, 1.0, 1.0, 1.0, 0.9025419790150205, 0.8806955667454326, 0.7999999999999998, 
    0.9299811099505544, 0.9299811099505544, 0.7999999999999998
]
```

---

## 🟢 Single Functions

Single functions compute a single correlation value for the entire dataset.

### `correlate_asset_prices`
```python
correlate_asset_prices(
    prices_asset_a: List[float],
    prices_asset_b: List[float],
    constant_model_type: str,
    deviation_model: str
) -> float
```

#### Arguments
- **prices_asset_a**: List of prices for asset A
- **prices_asset_b**: List of prices for asset B
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **deviation_model**: Choice of:
    - `"standard_deviation"`
    - `"mean_absolute_deviation"`
    - `"median_absolute_deviation"`
    - `"mode_absolute_deviation"`
    - `"ulcer_index"`

#### Returns
Correlation between the two asset price series

#### Example
```python
from pytechnicalindicators import correlation_indicators as ci

prices_asset_a = [ 
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

prices_asset_b = [ 
    50.0, 51.0, 52.5, 51.5, 53.0, 54.0, 53.5, 55.0,
    56.0, 55.5, 56.5, 57.0, 58.0, 57.5, 58.5
]

correlation = ci.single.correlate_asset_prices(
    prices_asset_a,
    prices_asset_b,
    constant_model_type="simple_moving_average",
    deviation_model="standard_deviation"
)

print(f"Single Correlation: {correlation}")
```

**Output:**
```
Single Correlation: 0.985299794588134
```

---

## 💡 Understanding Correlation Values

### Correlation Range
- **+1.0**: Perfect positive correlation (assets move exactly together)
- **+0.7 to +1.0**: Strong positive correlation
- **+0.3 to +0.7**: Moderate positive correlation
- **-0.3 to +0.3**: Weak or no correlation
- **-0.7 to -0.3**: Moderate negative correlation
- **-1.0 to -0.7**: Strong negative correlation
- **-1.0**: Perfect negative correlation (assets move in exact opposite directions)

_Using different deviation models can impact the range to go beyond +/-1_

---

## 🔧 Model Selection Guidelines

### Constant Model Types
- **simple_moving_average**: Best for stable, trending markets
- **exponential_moving_average**: More responsive to recent price changes
- **simple_moving_median**: Robust against outliers
- **smoothed_moving_average**: Reduces noise in volatile markets
- **simple_moving_mode**: Useful for identifying recurring price levels

### Deviation Models
- **standard_deviation**: Most common choice for normal market conditions
- **mean_absolute_deviation**: Less sensitive to extreme outliers
- **median_absolute_deviation**: Very robust against outliers
- **ulcer_index**: Focuses on downside risk correlation

---

## 📝 Notes

- Both price series must have the same length
- All input lists must be of type `List[float]` (Python list of floats)
- Bulk functions require sufficient data points relative to the period
- Consider market conditions when interpreting correlation values
- Correlation does not imply causation

---

## ⚠️ Important Considerations

1. **Data Length**: Ensure both asset price series have the same number of data points
2. **Market Regimes**: Correlations can change dramatically during market stress
3. **Time Alignment**: Make sure price data is properly time-aligned between assets
4. **Statistical Significance**: Longer time series provide more reliable correlation estimates
5. **Non-Linear Relationships**: Correlation only measures linear relationships

---

## 🔗 See Also

- [API Reference Index](../api/index.md)
