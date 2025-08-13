# Volatility Indicators API Reference

**Module:** `volatility_indicators`  

The `volatility_indicators` module provides functions for measuring the volatility of an asset—how much and how quickly prices move over time.

## 🏗️ Structure

- **single**: Functions that return a single value for a slice of prices.
- **bulk**: Functions that compute values of a slice of prices over a period and return a vector.

---

## 🚀 Bulk Functions

### `ulcer_index`
```python
ulcer_index(
    prices: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to calculate the Ulcer Index

#### Returns
List of Ulcer Index values (one per window)

#### Example
```python
from pytechnicalindicators import volatility_indicators as vi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

ulcer = vi.bulk.ulcer_index(
    prices,
    period=10
)

print(f"Bulk Ulcer Index: {ulcer}")
```

**Output:**
```
Bulk Ulcer Index: [
    1.079824135315245, 1.116127031732687, 1.116127031732687, 0.9396423572728076, 1.2503827654555648, 1.2802622492591058
]
```

---

### `volatility_system`
```python
volatility_system(
    high: List[float],
    low: List[float],
    close: List[float],
    period: int,
    constant_multiplier: float,
    constant_model_type: str
) -> List[float]
```

#### Arguments
- **high**: List of highs
- **low**: List of lows
- **close**: List of closing prices
- **period**: Period over which to calculate the volatility system
- **constant_multiplier**: Multiplier for ATR
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
List of volatility system SaR points

#### Example
```python
from pytechnicalindicators import volatility_indicators as vi

high = [105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0, 117.0, 114.0, 116.0, 118.0, 120.0, 117.0, 119.0]
low = [95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0, 107.0, 104.0, 106.0, 108.0, 110.0, 107.0, 109.0]
close = [100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0]

volsys = vi.bulk.volatility_system(
    high,
    low,
    close,
    period=14,
    constant_multiplier=2.0,
    constant_model_type="exponential_moving_average"
)

print(f"Bulk Volatility System: {volsys}")
```

**Output:**
```
Bulk Volatility System: [95.0, 95.0]
```

---

## 🟢 Single Functions

### `ulcer_index`
```python
ulcer_index(
    prices: List[float]
) -> float
```

#### Arguments
- **prices**: List of prices

#### Returns
Ulcer Index value

#### Example
```python
from pytechnicalindicators import volatility_indicators as vi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

ulcer = vi.single.ulcer_index(prices)

print(f"Single Ulcer Index: {ulcer}")
```

**Output:**
```
Single Ulcer Index: 1.1552440487507982
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
- Bulk functions require a `period` argument for window size
- Ulcer Index measures downside volatility (drawdown risk)
- Volatility System is a stop-and-reverse (SaR) trend system

---

## 🔗 See Also

- [API Reference](../api/index.md)

