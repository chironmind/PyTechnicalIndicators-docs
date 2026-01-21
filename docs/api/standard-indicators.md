# 📊 Standard Indicators API Reference

**Module:** `standard_indicators`  

The `standard_indicators` module provides implementations of widely-recognized technical indicators, following their established formulas and default parameters as commonly found in financial literature and platforms.

## 📚 When to Use

Use these functions when you need classic, industry-standard indicators for:
- Quick benchmarking
- Reproducing signals used by major charting tools or trading strategies
- Comparing with custom or alternative indicator settings

## 🏗️ Structure

- **single**: Functions that return a single value for a slice of prices.
- **bulk**: Functions that compute values of a slice of prices over a period and return a vector.

---

## 🚀 Bulk Functions

### `simple_moving_average`
```python
simple_moving_average(
    prices: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to calculate the moving average

#### Returns
List of simple moving averages

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

ma = si.bulk.simple_moving_average(
    prices,
    period=5
)

print(f"Bulk Simple Moving Average: {ma}")
```

**Output:**
```
Bulk Simple Moving Average: [103.2, 104.8, 105.8, 106.8, 108.6, 109.2, 109.8, 111.0, 112.0, 112.0, 113.0]
```

---

### `smoothed_moving_average`
```python
smoothed_moving_average(
    prices: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to calculate the moving average

#### Returns
List of smoothed moving averages

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

sma = si.bulk.smoothed_moving_average(
    prices,
    period=5
)

print(f"Bulk Smoothed Moving Average: {sma}")
```

**Output:**
```
Bulk Smoothed Moving Average: [
    103.75535459305092, 105.38410280818657, 106.1946692051404, 107.44312232270346, 109.23179438362685, 
    109.47786768205616, 110.0747263207996, 111.24464540694908, 112.4831032841504, 112.3864826273203,
    113.19657306044739
]
```

---

### `exponential_moving_average`
```python
exponential_moving_average(
    prices: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to calculate the moving average

#### Returns
List of exponential moving averages

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

ema = si.bulk.exponential_moving_average(
    prices,
    period=5
)

print(f"Bulk Exponential Moving Average: {ema}")
```

**Output:**
```
Bulk Exponential Moving Average: [
    104.15165876777255, 105.83886255924172, 106.478672985782, 107.90521327014218, 
    109.72511848341233, 109.6350710900474, 110.24170616113744, 111.46445497630334, 
    112.8957345971564, 112.59715639810429, 113.3175355450237
]
```

---

### `bollinger_bands`
```python
bollinger_bands(
    prices: List[float]
) -> List[Tuple[float, float, float]]
```

#### Arguments
- **prices**: List of prices

#### Returns
List of Bollinger band tuples (lower band, MA, upper band)

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    5224.0, 5223.28, 5238.34, 5294.39, 5306.26, 5297.43, 5311.95, 5314.53,
    5305.4, 5288.88, 5298.25, 5300.94, 5270.64, 5239.26, 5249.84, 5273.27,
    5282.59, 5335.27, 5350.22, 5351.13,5352.7, 5359.50
]

bands = si.bulk.bollinger_bands(prices)

print(f"Bulk Bollinger Bands: {bands}")
```

**Output:**
```
Bulk Bollinger Bands: [
    (5213.581409805746, 5287.7935, 5362.005590194253), 
    (5220.94517276249, 5294.2285, 5367.51182723751), 
    (5230.115435120723, 5301.039500000001, 5371.963564879278)
]
```

---

### `macd`
```python
macd(
    prices: List[float]
) -> List[Tuple[float, float, float]]
```

#### Arguments
- **prices**: List of prices

#### Returns
List of MACD tuples (MACD, Signal Line, Histogram)

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    5006.28, 5058.21, 5069.38, 5032.25, 5095.91, 5109.44, 5060.61, 5042.65, 5049.49,
    5122.71, 5168.05, 5188.96, 5181.83, 5203.26, 5224.01, 5223.28, 5238.34, 5294.39,
    5306.26, 5297.44, 5311.95, 5314.53, 5305.4, 5288.88, 5298.25, 5300.95, 5270.64,
    5239.26, 5249.84, 5273.28, 5282.59, 5335.28, 5350.22, 5351.13, 5352.7, 5359.51,
    5425.8
]

macd = si.bulk.macd(prices)

print(f"Bulk MACD: {macd}")
```

**Output:**
```
Bulk MACD: [
    (23.732307403474806, 27.84886885937626, -4.116561455901454), 
    (23.231108818822577, 25.86755810362568, -2.6364492848031027), 
    (23.98848088685554, 24.707439348177488, -0.7189584613219466), 
    (30.420645672222236, 25.56096652679902, 4.859679145423215)]

```

---

### `rsi`
```python
rsi(
    prices: List[float]
) -> List[float]
```

#### Arguments
- **prices**: List of prices

#### Returns
List of Relative Strength Index values

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

rsi = si.bulk.rsi(prices)

print(f"Bulk RSI: {rsi}")
```

**Output:**
```
Bulk RSI: [48.19372711085414, 47.86761658453627]
```

---

## 🟢 Single Functions

### `simple_moving_average`
```python
simple_moving_average(
    prices: List[float]
) -> float
```

#### Arguments
- **prices**: List of prices

#### Returns
Simple moving average

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

ma = si.single.simple_moving_average(prices)

print(f"Single Simple Moving Average: {ma}")
```

**Output:**
```
Single Simple Moving Average: 108.46666666666667
```

---

### `smoothed_moving_average`
```python
smoothed_moving_average(
    prices: List[float]
) -> float
```

#### Arguments
- **prices**: List of prices

#### Returns
Smoothed moving average

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

sma = si.single.smoothed_moving_average(prices)

print(f"Single Smoothed Moving Average: {sma}")
```

**Output:**
```
Single Smoothed Moving Average: 109.65816790024674
```

---

### `exponential_moving_average`
```python
exponential_moving_average(
    prices: List[float]
) -> float
```

#### Arguments
- **prices**: List of prices

#### Returns
Exponential moving average

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

ema = si.single.exponential_moving_average(prices)

print(f"Single Exponential Moving Average: {ema}")
```

**Output:**
```
Single Exponential Moving Average: 110.62189805422618
```

---

### `bollinger_bands`
```python
bollinger_bands(
    prices: List[float]
) -> Tuple[float, float, float]
```

#### Arguments
- **prices**: List of prices

#### Returns
Bollinger band tuple (lower band, MA, upper band)

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    5238.34, 5294.39, 5306.26, 5297.43, 5311.95, 5314.53, 5305.4, 5288.88, 5298.25,
    5300.94, 5270.64, 5239.26, 5249.84, 5273.27, 5282.59, 5335.27, 5350.22, 5351.13,
    5352.7, 5359.50
]

bands = si.single.bollinger_bands(prices)

print(f"Single Bollinger Bands: {bands}")
```

**Output:**
```
Single Bollinger Bands: (5230.115435120723, 5301.039500000001, 5371.963564879278)
```

---

### `macd`
```python
macd(
    prices: List[float]
) -> Tuple[float, float, float]
```

#### Arguments
- **prices**: List of prices

#### Returns
MACD tuple (MACD, Signal Line, Histogram)

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    5069.38, 5032.25, 5095.91, 5109.44, 5060.61, 5042.65, 5049.49, 5122.71,
    5168.05, 5188.96, 5181.83, 5203.26, 5224.01, 5223.28, 5238.34, 5294.39,
    5306.26, 5297.44, 5311.95, 5314.53, 5305.40, 5288.88, 5298.25, 5300.95,
    5270.64, 5239.26, 5249.84, 5273.28, 5282.59, 5335.28, 5350.22, 5351.13,
    5352.70, 5359.51,
]

macd = si.single.macd(prices)

print(f"Single MACD: {macd}")
```

**Output:**
```
Single MACD: (23.98848088685554, 24.707439348177488, -0.7189584613219466)
```

---

### `rsi`
```python
rsi(
    prices: List[float]
) -> float
```

#### Arguments
- **prices**: List of prices

#### Returns
Relative Strength Index value

#### Example
```python
from centaur_technical_indicators import standard_indicators as si

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0
]

rsi = si.single.rsi(prices)

print(f"Single RSI: {rsi}")
```

**Output:**
```
Single RSI: 48.19372711085414
```

---

## 📝 Notes

- All input lists must be of type `List[float]` (Python list of floats)
- Bulk functions require a `period` argument for window size where applicable
- The output format for tuple-returning functions is (lower band, moving average, upper band) for Bollinger Bands and (MACD, Signal Line, Histogram) for MACD

---

## 🔗 See Also

- [API Reference](../api/index.md)

