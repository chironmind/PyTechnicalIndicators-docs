# 🔧 Other Indicators API Reference

**Module:** `other_indicators`  

The `other_indicators` module provides technical analysis tools that do not fit neatly into the main categories like momentum, trend, or volatility.

These calculations often serve as foundational measures or are used as components of broader strategies.

## 📚 When to Use

Use these functions when you need to:
- Calculate foundational metrics for risk, volatility, or bar strength
- Analyze price movement range or return over a period
- Incorporate less common but valuable technical measures into your models

## 🏗️ Structure

- **single**: Functions that return a single value for a slice of prices.
- **bulk**: Functions that compute values of a slice of prices over a period and return a vector.

---

## 🚀 Bulk Functions

### `return_on_investment`
```python
return_on_investment(
    prices: List[float],
    investment: float
) -> List[Tuple[float, float]]
```

#### Arguments
- **prices**: List of prices
- **investment**: Initial investment amount

#### Returns
List of tuples containing (final investment value, percentage return)

#### Example
```python
from pytechnicalindicators import other_indicators as oi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

roi = oi.bulk.return_on_investment(
    prices,
    investment=1000.0
)

print(f"Bulk Return on Investment: {roi}")
```

**Output:**
```
Bulk Return on Investment: [
    (1020.0, 2.0), (1050.0, 2.941176470588235), (1030.0, -1.9047619047619049), 
    (1060.0, 2.912621359223301), (1080.0, 1.8867924528301887), 
    (1070.0, -0.9259259259259258), (1100.0, 2.803738317757009), (1120.0, 1.8181818181818181), 
    (1090.0, -2.6785714285714284), (1110.0, 1.834862385321101), (1130.0, 1.8018018018018018), 
    (1150.0, 1.7699115044247788), (1120.0, -2.608695652173913), (1140.0, 1.7857142857142856)
]
```

---

### `true_range`
```python
true_range(
    close: List[float],
    high: List[float],
    low: List[float]
) -> List[float]
```

#### Arguments
- **close**: List of previous closes
- **high**: List of highs
- **low**: List of lows

#### Returns
List of True Range values

#### Example
```python
from pytechnicalindicators import other_indicators as oi

close_prices = [
    99.0, 101.0, 104.0, 102.0, 105.0, 107.0, 106.0, 109.0,
    111.0, 108.0, 110.0, 112.0, 114.0, 111.0, 113.0
]
high_prices = [
    105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0,
    117.0, 114.0, 116.0, 118.0, 120.0, 117.0, 119.0
]
low_prices = [
    95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0,
    107.0, 104.0, 106.0, 108.0, 110.0, 107.0, 109.0
]

tr = oi.bulk.true_range(
    close_prices,
    high_prices,
    low_prices
)

print(f"Bulk True Range: {tr}")
```

**Output:**
```
Bulki True Range: [10.0, 10.0, 10.0, 10.0, 10.0, 10.0, 10.0, 10.0, 10.0, 10.0, 10.0, 10.0, 10.0, 10.0, 10.0]
```

---

### `average_true_range`
```python
average_true_range(
    close: List[float],
    high: List[float],
    low: List[float],
    constant_model_type: str,
    period: int
) -> List[float]
```

#### Arguments
- **close**: List of previous closes
- **high**: List of highs
- **low**: List of lows
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **period**: Period over which to calculate the ATR

#### Returns
List of Average True Range values

#### Example
```python
from pytechnicalindicators import other_indicators as oi

close_prices = [
    99.0, 101.0, 104.0, 102.0, 105.0, 107.0, 106.0, 109.0,
    111.0, 108.0, 110.0, 112.0, 114.0, 111.0, 113.0
]
high_prices = [
    105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0,
    117.0, 114.0, 116.0, 118.0, 120.0, 117.0, 119.0
]
low_prices = [
    95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0,
    107.0, 104.0, 106.0, 108.0, 110.0, 107.0, 109.0
]

atr = oi.bulk.average_true_range(
    close_prices,
    high_prices,
    low_prices,
    constant_model_type="simple_moving_average",
    period=14
)

print(f"Bulk Average True Range: {atr}")
```

**Output:**
```
Bulk Average True Range: [10.0, 10.0]
```

---

### `internal_bar_strength`
```python
internal_bar_strength(
    high: List[float],
    low: List[float],
    close: List[float]
) -> List[float]
```

#### Arguments
- **high**: List of highs
- **low**: List of lows
- **close**: List of closing prices

#### Returns
List of internal bar strength values

#### Example
```python
from pytechnicalindicators import other_indicators as oi

high_prices = [
    105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0,
    117.0, 114.0, 116.0, 118.0, 120.0, 117.0, 119.0
]
low_prices = [
    95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0,
    107.0, 104.0, 106.0, 108.0, 110.0, 107.0, 109.0
]
close_prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

ibs = oi.bulk.internal_bar_strength(
    high_prices,
    low_prices,
    close_prices
)

print(f"Bulk Internal Bar Strength: {ibs}")
```

**Output:**
```
Bulk Internal Bar Strength: [0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5]
```

---

### `positivity_indicator`
```python
positivity_indicator(
    open: List[float],
    previous_close: List[float],
    signal_period: int,
    constant_model_type: str
) -> List[Tuple[float, float]]
```

#### Arguments
- **open**: List of opening prices
- **previous_close**: List of closing prices
- **signal_period**: Period to calculate the signal
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
List of tuples containing (positivity indicator, signal line)

#### Example
```python
from pytechnicalindicators import other_indicators as oi

open_prices = [
    98.0, 101.5, 104.5, 102.5, 105.5, 107.5, 106.5, 109.5,
    111.5, 108.5, 110.5, 112.5, 114.5, 111.5, 113.5
]
previous_close = [
    99.0, 101.0, 104.0, 102.0, 105.0, 107.0, 106.0, 109.0,
    111.0, 108.0, 110.0, 112.0, 114.0, 111.0, 113.0
]

pos_indicator = oi.bulk.positivity_indicator(
    open_prices,
    previous_close,
    signal_period=5,
    constant_model_type="simple_moving_average"
)

print(f"Bulk Positivity Indicator: {pos_indicator}")
```

**Output:**
```
Bulk Positivity Indicator: [
    (0.4761904761904762, 0.18642085604811287), (0.46728971962616817, 0.48189900199354857), 
    (0.4716981132075472, 0.477228723644959), (0.45871559633027525, 0.47281799675716785), 
    (0.45045045045045046, 0.4648688711609834), (0.4629629629629629, 0.4622233685154808), 
    (0.45454545454545453, 0.45967451549933813), (0.4464285714285714, 0.45462060714354297), 
    (0.43859649122807015, 0.4505967861231019), (0.45045045045045046, 0.4505967861231019), 
    (0.4424778761061947, 0.44649976875174835)
]
```

---

## 🟢 Single Functions

### `return_on_investment`
```python
return_on_investment(
    start_price: float,
    end_price: float,
    investment: float
) -> Tuple[float, float]
```

#### Arguments
- **start_price**: Initial price of the asset
- **end_price**: Final price of the asset
- **investment**: Amount invested at start

#### Returns
Tuple of (final investment value, percentage return)

#### Example
```python
from pytechnicalindicators import other_indicators as oi

roi = oi.single.return_on_investment(
    start_price=100.0,
    end_price=114.0,
    investment=1000.0
)

print(f"Single Return on Investment: {roi}")
```

**Output:**
```
Single Return on Investment: (1140.0, 14.000000000000002)
```

---

### `true_range`
```python
true_range(
    close: float,
    high: float,
    low: float
) -> float
```

#### Arguments
- **close**: Previous period close
- **high**: Current period high
- **low**: Current period low

#### Returns
True Range value

#### Example
```python
from pytechnicalindicators import other_indicators as oi

tr = oi.single.true_range(
    close=113.0,
    high=119.0,
    low=109.0
)

print(f"Single True Range: {tr}")
```

**Output:**
```
Single True Range: 10.0
```

---

### `average_true_range`
```python
average_true_range(
    close: List[float],
    high: List[float],
    low: List[float],
    constant_model_type: str
) -> float
```

#### Arguments
- **close**: List of previous closes
- **high**: List of highs
- **low**: List of lows
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
Average True Range value

#### Example
```python
from pytechnicalindicators import other_indicators as oi

close_prices = [
    99.0, 101.0, 104.0, 102.0, 105.0, 107.0, 106.0, 109.0,
    111.0, 108.0, 110.0, 112.0, 114.0, 111.0, 113.0
]
high_prices = [
    105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0,
    117.0, 114.0, 116.0, 118.0, 120.0, 117.0, 119.0
]
low_prices = [
    95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0,
    107.0, 104.0, 106.0, 108.0, 110.0, 107.0, 109.0
]

atr = oi.single.average_true_range(
    close_prices,
    high_prices,
    low_prices,
    constant_model_type="simple_moving_average"
)

print(f"Single Average True Range: {atr}")
```

**Output:**
```
Single Average True Range: 10.0
```

---

### `internal_bar_strength`
```python
internal_bar_strength(
    high: float,
    low: float,
    close: float
) -> float
```

#### Arguments
- **high**: High price
- **low**: Low price
- **close**: Close price

#### Returns
Internal bar strength value

#### Example
```python
from pytechnicalindicators import other_indicators as oi

ibs = oi.single.internal_bar_strength(
    high=119.0,
    low=109.0,
    close=114.0
)

print(f"Single Internal Bar Strength: {ibs}")
```

**Output:**
```
Single Internal Bar Strength: 0.5
```

---

## 📝 Notes

- All input lists must be of type `List[float]` (Python list of floats)
- True Range calculations require previous close, current high, and current low
- Internal Bar Strength measures where the close falls within the high-low range
- Return on Investment returns both absolute value and percentage return
- Positivity Indicator compares opening gaps to previous closes

---

## 🔗 See Also

- [API Reference Index](../api/index.md)

