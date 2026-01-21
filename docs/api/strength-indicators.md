# 💪 Strength Indicators API Reference

**Module:** `strength_indicators`  

The `strength_indicators` module provides functions to assess the strength and conviction of price movements and trends using volume and price-based calculations.

## 📚 When to Use

Use these indicators to:
- Analyze volume-price relationships
- Gauge market conviction
- Identify trend strength

## 🏗️ Structure

- **single**: Functions that return a single value for a slice of prices.
- **bulk**: Functions that compute values of a slice of prices over a period and return a vector.

---

## 🚀 Bulk Functions

### `accumulation_distribution`
```python
accumulation_distribution(
    highs: List[float],
    lows: List[float],
    close: List[float],
    volume: List[float],
    previous_accumulation_distribution: float
) -> List[float]
```

#### Arguments
- **highs**: List of high prices
- **lows**: List of low prices
- **close**: List of closing prices
- **volume**: List of volumes
- **previous_accumulation_distribution**: Previous AD value (use 0.0 if none)

#### Returns
List of Accumulation Distribution values

#### Example
```python
from centaur_technical_indicators import strength_indicators as si

highs = [103.0, 102.0, 105.0]
lows = [99.0, 99.0, 100.0]
close = [102.0, 100.0, 103.0]
volume = [1000.0, 1500.0, 1200.0]

ad = si.bulk.accumulation_distribution(
    highs, lows, close, volume, previous_accumulation_distribution=0.0
)
print(f"Bulk Accumulation Distribution: {ad}")
```

**Output:**
```
Bulk Accumulation Distribution: [500.0, 0.0, 240.0]
```

---

### `positive_volume_index`
```python
positive_volume_index(
    close: List[float],
    volume: List[float],
    previous_volume_index: float
) -> List[float]
```

#### Arguments
- **close**: List of closing prices
- **volume**: List of volumes
- **previous_volume_index**: Previous PVI value (use 0.0 if none)

#### Returns
List of Positive Volume Index values

#### Example
```python
from centaur_technical_indicators import strength_indicators as si

close = [100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 112.0, 109.0]
volume = [1000, 1200, 1500, 1100, 1300, 1400, 1250, 1600, 1800, 1350]

pvi = si.bulk.positive_volume_index(
    close, volume, previous_volume_index=0.0
)
print(f"Bulk Positive Volume Index: {pvi}")
```

**Output:**
```
Bulk Positive Volume Index: [
    0.0204, 0.021, 0.021, 0.021611650485436895, 0.022019417475728158, 0.022019417475728158, 
    0.02263678432084203, 0.023048362217584613, 0.023048362217584613
]
```

---

### `negative_volume_index`
```python
negative_volume_index(
    close: List[float],
    volume: List[float],
    previous_volume_index: float
) -> List[float]
```

#### Arguments
- **close**: List of closing prices
- **volume**: List of volumes
- **previous_volume_index**: Previous NVI value (use 0.0 if none)

#### Returns
List of Negative Volume Index values

#### Example
```python
from centaur_technical_indicators import strength_indicators as si

close = [100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 112.0, 109.0]
volume = [1000, 1200, 1500, 1100, 1300, 1400, 1250, 1600, 1800, 1350]

nvi = si.bulk.negative_volume_index(
    close, volume, previous_volume_index=0.0
)
print(f"Bulk Negative Volume Index: {nvi}")
```

**Output:**
```
Bulk Negative Volume Index: [
    0.0, 0.0, -0.01868480725623583, -0.01868480725623583, -0.01868480725623583, -0.018511799781641053, 
    -0.018511799781641053, -0.018511799781641053, -0.018015948001775667
]
```

---

### `relative_vigor_index`
```python
relative_vigor_index(
    open: List[float],
    high: List[float],
    low: List[float],
    close: List[float],
    constant_model_type: str,
    period: int
) -> List[float]
```

#### Arguments
- **open**: List of opening prices
- **high**: List of highs
- **low**: List of lows
- **close**: List of closing prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **period**: Period over which to calculate the RVI

#### Returns
List of Relative Vigor Index values

#### Example
```python
from centaur_technical_indicators import strength_indicators as si

open_prices = [98.0, 101.5, 104.5, 102.5, 105.5, 107.5, 106.5, 109.5, 111.5, 108.5]
high_prices = [105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0, 117.0, 114.0]
low_prices = [95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0, 107.0, 104.0]
close_prices = [100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 112.0, 109.0]

rvi = si.bulk.relative_vigor_index(
    open_prices, high_prices, low_prices, close_prices,
    constant_model_type="simple_moving_average",
    period=5
)
print(f"Bulk Relative Vigor Index: {rvi}")
```

**Output:**
```
Bulk Relative Vigor Index: [0.0625, 0.05, 0.05, 0.05, 0.05, 0.05]
```

---

## 🟢 Single Functions

### `accumulation_distribution`
```python
accumulation_distribution(
    high: float,
    low: float,
    close: float,
    volume: float,
    previous_accumulation_distribution: float
) -> float
```

#### Arguments
- **high**: High price
- **low**: Low price
- **close**: Close price
- **volume**: Volume
- **previous_accumulation_distribution**: Previous AD value (use 0.0 if none)

#### Returns
Accumulation Distribution value

#### Example
```python
from centaur_technical_indicators import strength_indicators as si

ad = si.single.accumulation_distribution(
    high=103.0, low=99.0, close=102.0, volume=1600.0, previous_accumulation_distribution=0.0
)
print(f"Single Accumulation Distribution: {ad}")
```

**Output:**
```
Single Accumulation Distribution: 800.0
```

---

### `volume_index`
```python
volume_index(
    current_close: float,
    previous_close: float,
    previous_volume_index: float
) -> float
```

#### Arguments
- **current_close**: Current close price
- **previous_close**: Previous close price
- **previous_volume_index**: Previous PVI/NVI value (use 0.0 if none)

#### Returns
Volume Index value

#### Example
```python
from centaur_technical_indicators import strength_indicators as si

vi = si.single.volume_index(
    current_close=110.0, previous_close=108.0, previous_volume_index=0.0
)
print(f"Single Volume Index: {vi}")
```

**Output:**
```
Single Volume Index: 0.01886145404663923
```

---

### `relative_vigor_index`
```python
relative_vigor_index(
    open: List[float],
    high: List[float],
    low: List[float],
    close: List[float],
    constant_model_type: str
) -> float
```

#### Arguments
- **open**: List of opening prices
- **high**: List of highs
- **low**: List of lows
- **close**: List of closing prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
Relative Vigor Index value

#### Example
```python
from centaur_technical_indicators import strength_indicators as si

open_prices = [98.0, 101.5, 104.5, 102.5, 105.5, 107.5, 106.5, 109.5, 111.5, 108.5]
high_prices = [105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0, 117.0, 114.0]
low_prices = [95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0, 107.0, 104.0]
close_prices = [100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 112.0, 109.0]

rvi = si.single.relative_vigor_index(
    open_prices, high_prices, low_prices, close_prices,
    constant_model_type="simple_moving_average"
)
print(f"Single Relative Vigor Index: {rvi}")
```

**Output:**
```
Single Relative Vigor Index: 0.05357142857142857
```

---

## 🧩 Model Type Choices

**constant_model_type:**  
- `"simple_moving_average"`
- `"smoothed_moving_average"`
- `"exponential_moving_average"`
- `"simple_moving_median"`
- `"simple_moving_mode"`

---

## 📝 Notes

- All input lists must be of type `List[float]` (Python list of floats)
- Bulk functions require a `period` argument for window size where applicable
- Use 0.0 for previous values if no prior value exists (e.g., for AD, PVI, NVI)
- Volume Index is used generically for both Positive and Negative Volume Index calculations

---

## 🔗 See Also

- [API Reference](../api/index.md)

