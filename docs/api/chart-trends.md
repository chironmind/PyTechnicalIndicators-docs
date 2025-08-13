# 📈 Chart Trends API Reference

**Module:** `chart_trends`  

The `chart_trends` module provides utilities for detecting, analyzing, and breaking down trends in price charts.  
These functions help identify overall direction, peaks, valleys, and trend segments in a time series.

## 📚 When to Use

Use chart trend indicators to:
- Decompose a price series into upward/downward trends
- Find peaks and valleys for support/resistance analysis
- Quantify the overall or local trend direction of an asset

## 🏗️ Structure

Unlike other modules, `chart_trends` contains standalone functions that operate directly on price data without bulk/single subdivisions.

---

### `peaks`
```python
peaks(
    prices: List[float],
    period: int,
    closest_neighbor: int
) -> List[Tuple[float, int]]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to find peaks
- **closest_neighbor**: Minimum distance between peaks

#### Returns
List of tuples containing (peak value, peak index)

#### Example
```python
from pytechnicalindicators import chart_trends as ct

prices = [
    100.0, 102.0, 108.0, 106.0, 104.0, 107.0, 112.0, 110.0,
    109.0, 111.0, 115.0, 113.0, 108.0, 105.0, 102.0
]

peaks_result = ct.peaks(
    prices,
    period=3,
    closest_neighbor=2
)

print(f"Peaks: {peaks_result}")
```

**Output:**
```
Peaks: [(108.0, 2), (112.0, 6), (115.0, 10)]
```

---

### `valleys`
```python
valleys(
    prices: List[float],
    period: int,
    closest_neighbor: int
) -> List[Tuple[float, int]]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to find valleys
- **closest_neighbor**: Minimum distance between valleys

#### Returns
List of tuples containing (valley value, valley index)

#### Example
```python
from pytechnicalindicators import chart_trends as ct

prices = [
    100.0, 102.0, 108.0, 106.0, 104.0, 107.0, 112.0, 110.0,
    109.0, 111.0, 115.0, 113.0, 108.0, 105.0, 102.0
]

valleys_result = ct.valleys(
    prices,
    period=3,
    closest_neighbor=2
)

print(f"Valleys: {valleys_result}")
```

**Output:**
```
Valleys: [(100.0, 0), (102.0, 1), (104.0, 4), (109.0, 8), (102.0, 14)]
```

---

### `peak_trend`
```python
peak_trend(
    prices: List[float],
    period: int
) -> Tuple[float, float]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to calculate the peaks

#### Returns
Tuple containing (slope, intercept) of the peak trend line

#### Example
```python
from pytechnicalindicators import chart_trends as ct

prices = [
    100.0, 102.0, 108.0, 106.0, 104.0, 107.0, 112.0, 110.0,
    109.0, 111.0, 115.0, 113.0, 108.0, 105.0, 102.0
]

peak_trend_result = ct.peak_trend(
    prices,
    period=3
)

print(f"Peak Trend (slope, intercept): {peak_trend_result}")
```

**Output:**
```
Peak Trend (slope, intercept): (0.875, 106.41666666666667)
```

---

### `valley_trend`
```python
valley_trend(
    prices: List[float],
    period: int
) -> Tuple[float, float]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to calculate the valleys

#### Returns
Tuple containing (slope, intercept) of the valley trend line

#### Example
```python
from pytechnicalindicators import chart_trends as ct

prices = [
    100.0, 102.0, 108.0, 106.0, 104.0, 107.0, 112.0, 110.0,
    109.0, 111.0, 115.0, 113.0, 108.0, 105.0, 102.0
]

valley_trend_result = ct.valley_trend(
    prices,
    period=3
)

print(f"Valley Trend (slope, intercept): {valley_trend_result}")
```

**Output:**
```
Valley Trend (slope, intercept): (0.1996951219512195, 102.32164634146342)
```

---

### `overall_trend`
```python
overall_trend(
    prices: List[float]
) -> Tuple[float, float]
```

#### Arguments
- **prices**: List of prices

#### Returns
Tuple containing (slope, intercept) of the overall trend line

#### Example
```python
from pytechnicalindicators import chart_trends as ct

prices = [
    100.0, 102.0, 108.0, 106.0, 104.0, 107.0, 112.0, 110.0,
    109.0, 111.0, 115.0, 113.0, 108.0, 105.0, 102.0
]

overall_trend_result = ct.overall_trend(prices)

print(f"Overall Trend (slope, intercept): {overall_trend_result}")
```

**Output:**
```
Overall Trend (slope, intercept): (0.35, 105.01666666666667)
```

---

### `break_down_trends`
```python
break_down_trends(
    prices: List[float],
    max_outliers: int,
    soft_r_squared_minimum: float,
    soft_r_squared_maximum: float,
    hard_r_squared_minimum: float,
    hard_r_squared_maximum: float,
    soft_standard_error_multiplier: float,
    hard_standard_error_multiplier: float,
    soft_reduced_chi_squared_multiplier: float,
    hard_reduced_chi_squared_multiplier: float
) -> List[Tuple[int, int, float, float]]
```

#### Arguments
- **prices**: List of prices
- **max_outliers**: Allowed consecutive trend-breaks before splitting
- **soft_r_squared_minimum**: Soft minimum value for R²
- **soft_r_squared_maximum**: Soft maximum value for R²
- **hard_r_squared_minimum**: Hard minimum value for R²
- **hard_r_squared_maximum**: Hard maximum value for R²
- **soft_standard_error_multiplier**: Soft standard error multiplier
- **hard_standard_error_multiplier**: Hard standard error multiplier
- **soft_reduced_chi_squared_multiplier**: Soft chi squared multiplier
- **hard_reduced_chi_squared_multiplier**: Hard chi squared multiplier

#### Returns
List of tuples containing (start index, end index, slope, intercept) for each trend segment

#### Example
```python
from pytechnicalindicators import chart_trends as ct

prices = [
    100.0, 102.0, 108.0, 106.0, 104.0, 107.0, 112.0, 110.0,
    109.0, 111.0, 115.0, 113.0, 108.0, 105.0, 102.0, 98.0,
    95.0, 93.0, 91.0, 89.0
]

trends = ct.break_down_trends(
    prices,
    max_outliers=2,
    soft_r_squared_minimum=0.7,
    soft_r_squared_maximum=0.95,
    hard_r_squared_minimum=0.5,
    hard_r_squared_maximum=0.99,
    soft_standard_error_multiplier=1.5,
    hard_standard_error_multiplier=2.0,
    soft_reduced_chi_squared_multiplier=1.2,
    hard_reduced_chi_squared_multiplier=2.5
)

print(f"Trend Segments: {trends}")
```

**Output:**
```
Trend Segments: [
    (0, 2, 4.0, 99.33333333333333), (2, 5, -0.5, 108.0), 
    (5, 11, 0.9316770186335402, 103.70186335403727), (11, 14, -3.6, 152.0)
]
```

---

## 💡 Usage Tips

### Peak and Valley Detection
- **period**: Controls the local window for peak/valley detection. Larger values find more significant peaks/valleys.
- **closest_neighbor**: Prevents detecting peaks/valleys too close together. Use to filter noise.

### Trend Line Analysis
- Use `peak_trend()` and `valley_trend()` to identify resistance and support trend lines.
- `overall_trend()` gives the general direction of the entire price series.

### Advanced Trend Decomposition
The `break_down_trends()` function uses sophisticated statistical criteria:
- **R² values**: Measure how well the trend line fits the data
- **Standard error multipliers**: Control sensitivity to deviations from the trend
- **Chi-squared multipliers**: Statistical tests for goodness of fit
- **Soft vs Hard limits**: Soft limits are preferred, hard limits are absolute boundaries

#### Parameter Guidelines
```python
# Conservative settings (fewer, more reliable trends)
max_outliers=3
soft_r_squared_minimum=0.8
hard_r_squared_minimum=0.6

# Aggressive settings (more trend segments, higher sensitivity)
max_outliers=1
soft_r_squared_minimum=0.6
hard_r_squared_minimum=0.4
```

---

## 📊 Interpreting Results

### Slope Interpretation
- **Positive slope**: Upward trend
- **Negative slope**: Downward trend
- **Slope magnitude**: Rate of change (steeper = faster price movement)

### Index Values
- All index values refer to positions in the original price array
- Use these to map trends back to time periods in your data

---

## 🔗 Use Cases

1. **Support/Resistance Analysis**: Use `peaks()` and `valleys()` to identify key price levels
2. **Trend Following**: Use `overall_trend()` to determine market direction
3. **Trend Change Detection**: Use `break_down_trends()` to identify trend reversals
4. **Technical Analysis**: Combine with candle indicators for comprehensive analysis

---

## 📝 Notes

- All input lists must be of type `List[float]` (Python list of floats)
- Functions return exact indices from the input array
- Peak/valley detection requires sufficient data points relative to the period
- Trend decomposition works best with longer price series (20+ data points)

---

## 🔗 See Also

- [API Reference](../api/index.md)

