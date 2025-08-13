# 📈 Trend Indicators API Reference

**Module:** `trend_indicators`  

The `trend_indicators` module provides functions to analyze and quantify price trends in time series data.

Trend indicators are used to determine the direction, strength, and potential reversals of market trends.

## 📚 When to Use

Use trend indicators to:
- Identify the presence and direction of a trend (uptrend, downtrend, or sideways)
- Spot trend reversals or trend exhaustion
- Confirm other technical signals

## 🏗️ Structure

- **single**: Functions that return a single value for a slice of prices.
- **bulk**: Functions that compute values of a slice of prices over a period and return a vector.

---

## 🚀 Bulk Functions

### `aroon_up`
```python
aroon_up(
    highs: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **highs**: List of highs
- **period**: Period over which to calculate the Aroon up

#### Returns
List of Aroon Up values

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

highs = [105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0, 117.0, 114.0, 116.0, 118.0, 120.0, 117.0, 119.0]
aroon_up = ti.bulk.aroon_up(highs, period=5)
print(f"Bulk Aroon Up: {aroon_up}")
```

**Output:**
```
Bulk Aroon Up: [100.0, 100.0, 75.0, 100.0, 100.0, 75.0, 50.0, 100.0, 100.0, 75.0, 50.0]
```

---

### `aroon_down`
```python
aroon_down(
    lows: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **lows**: List of lows
- **period**: Period over which to calculate the Aroon down

#### Returns
List of Aroon Down values

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

lows = [95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0, 107.0, 104.0, 106.0, 108.0, 110.0, 107.0, 109.0]
aroon_down = ti.bulk.aroon_down(lows, period=5)
print(f"Bulk Aroon Down: {aroon_down}")
```

**Output:**
```
Bulk Aroon Down: [0.0, 0.0, 25.0, 0.0, 0.0, 25.0, 0.0, 50.0, 25.0, 0.0, 0.0]
```

---

### `aroon_oscillator`
```python
aroon_oscillator(
    aroon_up: List[float],
    aroon_down: List[float]
) -> List[float]
```

#### Arguments
- **aroon_up**: List of Aroon Up values
- **aroon_down**: List of Aroon Down values

#### Returns
List of Aroon Oscillator values

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

aroon_up = [100.0, 80.0, 60.0, 40.0, 20.0]
aroon_down = [20.0, 40.0, 60.0, 80.0, 100.0]
osc = ti.bulk.aroon_oscillator(aroon_up, aroon_down)
print(f"Bulk Aroon Oscillator: {osc}")
```

**Output:**
```
Bulk Aroon Oscillator: [80.0, 40.0, 0.0, -40.0, -80.0]
```

---

### `aroon_indicator`
```python
aroon_indicator(
    highs: List[float],
    lows: List[float],
    period: int
) -> List[Tuple[float, float, float]]
```

#### Arguments
- **highs**: List of highs
- **lows**: List of lows
- **period**: Period over which to calculate the Aroon indicator

#### Returns
List of Aroon indicator tuples (Aroon Up, Aroon Down, Aroon Oscillator)

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

highs = [105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0, 117.0, 114.0, 116.0, 118.0, 120.0, 117.0, 119.0]
lows = [95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0, 107.0, 104.0, 106.0, 108.0, 110.0, 107.0, 109.0]
aroon = ti.bulk.aroon_indicator(highs, lows, period=5)
print(f"Bulk Aroon Indicator: {aroon}")
```

**Output:**
```
Bulk Aroon Indicator: [
    (100.0, 0.0, 100.0), (100.0, 0.0, 100.0), (75.0, 25.0, 50.0), (100.0, 0.0, 100.0), 
    (100.0, 0.0, 100.0), (75.0, 25.0, 50.0), (50.0, 0.0, 50.0), (100.0, 50.0, 50.0), 
    (100.0, 25.0, 75.0), (75.0, 0.0, 75.0), (50.0, 0.0, 50.0)
]
```

---

### `parabolic_time_price_system`
```python
parabolic_time_price_system(
    highs: List[float],
    lows: List[float],
    af_start: float,
    af_step: float,
    af_max: float,
    position: str,
    previous_sar: float
) -> List[float]
```

#### Arguments
- **highs**: List of highs
- **lows**: List of lows
- **af_start**: Initial acceleration factor
- **af_step**: Acceleration factor increment (default 0.02)
- **af_max**: Maximum acceleration factor (default 0.2)
- **position**: `"long"` or `"short"`
- **previous_sar**: Previous SAR value (use 0.0 if none)

#### Returns
List of SAR values

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

highs = [105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0, 117.0, 114.0]
lows = [95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0, 107.0, 104.0]

sar = ti.bulk.parabolic_time_price_system(
    highs, lows, af_start=0.02, af_step=0.02, af_max=0.2, position="long", previous_sar=0.0
)
print(f"Bulk Parabolic Time Price System: {sar}")
```

**Output:**
```
Bulk Parabolic Time Price System: [
    95.0, 95.0, 95.3, 95.594, 95.90212, 96.2440776, 96.579196048, 96.94761212704, 97.3486598844992, 97.74168668680922
]
```

---

### `directional_movement_system`
```python
directional_movement_system(
    highs: List[float],
    lows: List[float],
    close: List[float],
    period: int,
    constant_model_type: str
) -> List[Tuple[float, float, float, float]]
```

#### Arguments
- **highs**: List of highs
- **lows**: List of lows
- **close**: List of close prices
- **period**: Period for calculation
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
List of Directional Movement System tuples (+DI, -DI, ADX, ADXR)

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

highs = [105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0, 117.0, 114.0]
lows = [95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0, 107.0, 104.0]
close = [100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 112.0, 109.0]

dms = ti.bulk.directional_movement_system(
    highs, lows, close, period=3, constant_model_type="simple_moving_average"
)
print(f"Bulk Directional Movement System: {dms}")
```

**Output:**
```
Bulk Directional Movement System: [
    (16.666666666666664, 3.3333333333333335, 58.73015873015871, 51.984126984126966), 
    (16.666666666666664, 3.3333333333333335, 66.66666666666666, 59.92063492063491), 
    (16.666666666666664, 10.0, 52.77777777777777, 55.75396825396824)
]
```

---

### `volume_price_trend`
```python
volume_price_trend(
    prices: List[float],
    volumes: List[float],
    previous_vpt: float
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **volumes**: List of volumes
- **previous_vpt**: Previous VPT value (use 0.0 if none)

#### Returns
List of VPT values

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

prices = [100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 112.0, 109.0]
volumes = [1000, 1200, 1500, 1100, 1300, 1400, 1250, 1600, 1800]

vpt = ti.bulk.volume_price_trend(
    prices, volumes, previous_vpt=0.0
)
print(f"Bulk Volume Price Trend: {vpt}")
```

**Output:**
```
Bulk Volume Price Trend: [
    20.0, 55.294117647058826, 26.722689075630253, 58.761524027086566, 83.28982591387901, 
    70.32686295091605, 105.37359192287866, 134.46450101378775, 86.25021529950205
]
```

---

### `true_strength_index`
```python
true_strength_index(
    prices: List[float],
    first_constant_model: str,
    first_period: int,
    second_constant_model: str,
    second_period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **first_constant_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **first_period**: Period for first smoothing
- **second_constant_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **second_period**: Period for second smoothing

#### Returns
List of TSI values

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

prices = [100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 112.0, 109.0]

tsi = ti.bulk.true_strength_index(
    prices,
    first_constant_model="exponential_moving_average",
    first_period=3,
    second_constant_model="exponential_moving_average",
    second_period=5
)
print(f"Bulk True Strength Index: {tsi}")
```

**Output:**
```
Bulk True Strength Index: [0.5758338577721839, 0.7186215618084096, 0.2882438316400581]
```

---

## 🟢 Single Functions

### `aroon_up`
```python
aroon_up(
    highs: List[float]
) -> float
```

#### Arguments
- **highs**: List of highs

#### Returns
Aroon Up value

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

highs = [105.0, 107.0, 110.0, 108.0, 111.0]
aroon_up = ti.single.aroon_up(highs)
print(f"Single Aroon Up: {aroon_up}")
```

**Output:**
```
Single Aroon Up: 100.0
```

---

### `aroon_down`
```python
aroon_down(
    lows: List[float]
) -> float
```

#### Arguments
- **lows**: List of lows

#### Returns
Aroon Down value

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

lows = [95.0, 97.0, 100.0, 98.0, 101.0]
aroon_down = ti.single.aroon_down(lows)
print(f"Single Aroon Down: {aroon_down}")
```

**Output:**
```
Single Aroon Down: 0.0
```

---

### `aroon_oscillator`
```python
aroon_oscillator(
    aroon_up: float,
    aroon_down: float
) -> float
```

#### Arguments
- **aroon_up**: Aroon Up value
- **aroon_down**: Aroon Down value

#### Returns
Aroon Oscillator value

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

osc = ti.single.aroon_oscillator(80.0, 20.0)
print(f"Single Aroon Oscillator: {osc}")
```

**Output:**
```
Single Aroon Oscillator: 60.0
```

---

### `aroon_indicator`
```python
aroon_indicator(
    highs: List[float],
    lows: List[float]
) -> Tuple[float, float, float]
```

#### Arguments
- **highs**: List of highs
- **lows**: List of lows

#### Returns
Aroon indicator tuple (Aroon Up, Aroon Down, Aroon Oscillator)

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

highs = [105.0, 107.0, 110.0, 108.0, 111.0]
lows = [95.0, 97.0, 100.0, 98.0, 101.0]
aroon = ti.single.aroon_indicator(highs, lows)
print(f"Single Aroon Indicator: {aroon}")
```

**Output:**
```
Single Aroon Indicator: (100.0, 0.0, 100.0)
```

---

### `long_parabolic_time_price_system`
```python
long_parabolic_time_price_system(
    previous_sar: float,
    extreme_point: float,
    af: float,
    low: float
) -> float
```

#### Arguments
- **previous_sar**: Previous SAR value (if none use period low)
- **extreme_point**: Highest high for the period
- **af**: Acceleration factor (default 0.02)
- **low**: Lowest low for t or t-1

#### Returns
SAR value

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

sar = ti.single.long_parabolic_time_price_system(
    previous_sar=98.0, extreme_point=111.0, af=0.02, low=95.0
)
print(f"Single Long Parabolic SAR: {sar}")
```

**Output:**
```
Single Long Parabolic SAR: 95.0
```

---

### `short_parabolic_time_price_system`
```python
short_parabolic_time_price_system(
    previous_sar: float,
    extreme_point: float,
    af: float,
    high: float
) -> float
```

#### Arguments
- **previous_sar**: Previous SAR value (if none use period high)
- **extreme_point**: Lowest low for the period
- **af**: Acceleration factor (default 0.02)
- **high**: Highest high for t or t-1

#### Returns
SAR value

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

sar = ti.single.short_parabolic_time_price_system(
    previous_sar=112.0, extreme_point=95.0, af=0.02, high=119.0
)
print(f"Single Short Parabolic SAR: {sar}")
```

**Output:**
```
Single Short Parabolic SAR: 119.0
```

---

### `volume_price_trend`
```python
volume_price_trend(
    current_price: float,
    previous_price: float,
    current_volume: float,
    previous_vpt: float
) -> float
```

#### Arguments
- **current_price**: Current price
- **previous_price**: Previous price
- **current_volume**: Current volume
- **previous_vpt**: Previous VPT value (use 0.0 if none)

#### Returns
VPT value

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

vpt = ti.single.volume_price_trend(
    current_price=114.0, previous_price=112.0, current_volume=1500.0, previous_vpt=0.0
)
print(f"Single Volume Price Trend: {vpt}")
```

**Output:**
```
Single Volume Price Trend: 26.785714285714285
```

---

### `true_strength_index`
```python
true_strength_index(
    prices: List[float],
    first_period: int,
    first_constant_model: str,
    second_constant_model: str
) -> float
```

#### Arguments
- **prices**: List of prices
- **first_period**: Period for first smoothing
- **first_constant_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **second_constant_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
TSI value

#### Example
```python
from pytechnicalindicators import trend_indicators as ti

prices = [100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0, 112.0, 109.0]
tsi = ti.single.true_strength_index(
    prices,
    first_period=5,
    first_constant_model="exponential_moving_average",
    second_constant_model="exponential_moving_average"
)
print(f"Single True Strength Index: {tsi}")
```

**Output:**
```
Single True Strength Index: 0.38660105755017404
```

---

## 🧩 Model Type Choices

**constant_model_type / first_constant_model / second_constant_model:**  
- `"simple_moving_average"`
- `"smoothed_moving_average"`
- `"exponential_moving_average"`
- `"simple_moving_median"`
- `"simple_moving_mode"`

---

## 📝 Notes

- All input lists must be of type `List[float]` (Python list of floats)
- Bulk functions require a `period` argument for window size where applicable
- Use 0.0 for previous values if no prior value exists (e.g., SAR, VPT)
- Parabolic SAR and DMS support both `"long"` and `"short"` positions where specified

---

## 🔗 See Also

- [API Reference](../api/index.md)

