# 📈 Moving Average API Reference

**Module:** `moving_average`  

The `moving_average` module provides functions for calculating moving averages, a core component of many technical indicators and trading strategies.

## 🏗️ Structure

- **single**: Functions that return a single value for a slice of prices.
- **bulk**: Functions that compute values of a slice of prices over a period and return a vector.

---

## 🚀 Bulk Functions

### `moving_average`
```python
moving_average(
    prices: List[float],
    moving_average_type: str,
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **moving_average_type**: Choice of:
    - `"simple"`
    - `"smoothed"`
    - `"exponential"`
- **period**: Period over which to calculate the moving average

#### Returns
List of moving averages

#### Example
```python
from pytechnicalindicators import moving_average as ma

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

sma = ma.bulk.moving_average(
    prices,
    moving_average_type="simple",
    period=5
)

print(f"Bulk Moving Average: {sma}")
```

**Output:**
```
Bulk Moving Average: [103.2, 104.8, 105.8, 106.8, 108.6, 109.2, 109.8, 111.0, 112.0, 112.0, 113.0]
```

---

### `mcginley_dynamic`
```python
mcginley_dynamic(
    prices: List[float],
    previous_mcginley_dynamic: float,
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **previous_mcginley_dynamic**: Previous McGinley dynamic (if none use 0.0)
- **period**: Period over which to calculate the McGinley dynamic

#### Returns
List of McGinley dynamics

#### Example
```python
from pytechnicalindicators import moving_average as ma

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

mcginley = ma.bulk.mcginley_dynamic(
    prices,
    previous_mcginley_dynamic=0.0,
    period=10
)

print(f"Bulk McGinley Dynamic: {mcginley}")
```

**Output:**
```
Bulk McGinley Dynamic: [
    109.0, 109.1859705058081, 109.51842914892896, 109.9693107300164, 110.15804788110441, 110.49301136686225
]
```

---

## 🟢 Single Functions

### `moving_average`
```python
moving_average(
    prices: List[float],
    moving_average_type: str
) -> float
```

#### Arguments
- **prices**: List of prices
- **moving_average_type**: Choice of:
    - `"simple"`
    - `"smoothed"`
    - `"exponential"`

#### Returns
Moving average value

#### Example
```python
from pytechnicalindicators import moving_average as ma

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

sma = ma.single.moving_average(
    prices,
    moving_average_type="simple"
)

print(f"Single Moving Average: {sma}")
```

**Output:**
```
Single Moving Average: 108.46666666666667
```

---

### `mcginley_dynamic`
```python
mcginley_dynamic(
    latest_price: float,
    previous_mcginley_dynamic: float,
    period: int
) -> float
```

#### Arguments
- **latest_price**: Most recent price
- **previous_mcginley_dynamic**: Previous McGinley dynamic (if none use 0.0)
- **period**: Length of the observed period

#### Returns
McGinley dynamic value

#### Example
```python
from pytechnicalindicators import moving_average as ma

mcginley = ma.single.mcginley_dynamic(
    latest_price=114.0,
    previous_mcginley_dynamic=108.5,
    period=10
)

print(f"Single McGinley Dynamic: {mcginley}")
```

**Output:**
```
Single McGinley Dynamic: 108.95129678212406
```

---

## 📝 Notes

- All input lists must be of type `List[float]` (Python list of floats)
- Bulk functions require a `period` argument for window size
- McGinley Dynamic requires previous values for continuity
- Use 0.0 for `previous_mcginley_dynamic` if no previous value exists

---

## 🔗 See Also

- [API Reference Index](../api/index.md)

