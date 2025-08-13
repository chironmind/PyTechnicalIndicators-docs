# ⚡ Momentum Indicators API Reference

**Module:** `momentum_indicators`  

The `momentum_indicators` module provides functions to measure the speed, strength, and direction of price movements in time series data.

These indicators are commonly used to identify overbought/oversold conditions, trend continuation, or potential reversals.

## 📚 When to Use

Use momentum indicators to:
- Gauge the strength and velocity of price trends
- Identify bullish or bearish momentum
- Spot early signals for possible price reversals or continuations

## 🏗️ Structure

- **single**: Functions that return a single value for a slice of prices.
- **bulk**: Functions that compute values of a slice of prices over a period and return a vector.

---

## 🚀 Bulk Functions

### `relative_strength_index`
```python
relative_strength_index(
    prices: List[float],
    constant_model_type: str,
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **period**: Period over which to calculate the RSI

#### Returns
List of Relative Strength Index values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

rsi = mi.bulk.relative_strength_index(
    prices,
    constant_model_type="simple_moving_average",
    period=5
)

print(f"Bulk RSI: {rsi}")
```

**Output:**
```
Bulk RSI: [
    57.14285714285714, 57.14285714285714, 62.50000000000001, 72.72727272727272, 
    70.0, 55.55555555555556, 43.75, 39.99999999999999, 39.99999999999999, 
    39.99999999999999, 39.99999999999999
]
```

---

### `stochastic_oscillator`
```python
stochastic_oscillator(
    prices: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to calculate the stochastic oscillator

#### Returns
List of Stochastic Oscillator values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

stoch = mi.bulk.stochastic_oscillator(
    prices,
    period=5
)

print(f"Bulk Stochastic Oscillator: {stoch}")
```

**Output:**
```
Bulk Stochastic Oscillator: [100.0, 100.0, 80.0, 100.0, 100.0, 40.0, 80.0, 100.0, 100.0, 50.0, 75.0]
```

---

### `slow_stochastic`
```python
slow_stochastic(
    stochastics: List[float],
    constant_model_type: str,
    period: int
) -> List[float]
```

#### Arguments
- **stochastics**: List of stochastic values
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **period**: Period over which to calculate the slow stochastic

#### Returns
List of Slow Stochastic values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

stochastics = [
    80.0, 75.0, 70.0, 72.0, 68.0, 65.0, 70.0, 73.0,
    75.0, 78.0, 80.0, 77.0, 74.0, 76.0, 79.0
]

slow_stoch = mi.bulk.slow_stochastic(
    stochastics,
    constant_model_type="simple_moving_average",
    period=5
)

print(f"Bulk Slow Stochastic: {slow_stoch}")
```

**Output:**
```
Bulk Slow Stochastic: [73.0, 70.0, 69.0, 69.6, 70.2, 72.2, 75.2, 76.6, 76.8, 77.0, 77.2]
```

---

### `slowest_stochastic`
```python
slowest_stochastic(
    slow_stochastics: List[float],
    constant_model_type: str,
    period: int
) -> List[float]
```

#### Arguments
- **slow_stochastics**: List of slow stochastic values
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **period**: Period over which to calculate the slowest stochastic

#### Returns
List of Slowest Stochastic values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

slow_stochastics = [
    73.0, 70.0, 69.0, 69.6, 70.2, 72.2, 75.2, 76.6, 76.8, 77.0, 77.2
]

slowest_stoch = mi.bulk.slowest_stochastic(
    slow_stochastics,
    constant_model_type="simple_moving_average",
    period=5
)

print(f"Bulk Slowest Stochastic: {slowest_stoch}")
```

**Output:**
```
Bulk Slowest Stochastic: [70.36, 70.2, 71.24, 72.75999999999999, 74.20000000000002, 75.56, 76.56]
```

---

### `williams_percent_r`
```python
williams_percent_r(
    high: List[float],
    low: List[float],
    close: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **high**: List of high prices
- **low**: List of low prices
- **close**: List of closing prices
- **period**: Period over which to calculate Williams %R

#### Returns
List of Williams %R values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

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

williams_r = mi.bulk.williams_percent_r(
    high_prices,
    low_prices,
    close_prices,
    period=5
)

print(f"Bulk Williams %R: {williams_r}")
```

**Output:**
```
Bulk Williams %R: [
    -31.25, -31.25, -40.0, -29.411764705882355, -31.25, -53.333333333333336, -40.0, 
    -35.714285714285715, -31.25, -50.0, -42.857142857142854
]
```

---

### `money_flow_index`
```python
money_flow_index(
    prices: List[float],
    volume: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **volume**: List of volumes
- **period**: Period over which to calculate the MFI

#### Returns
List of Money Flow Index values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]
volume = [
    1000, 1200, 1500, 1100, 1300, 1400, 1250, 1600,
    1800, 1350, 1450, 1550, 1700, 1400, 1500
]

mfi = mi.bulk.money_flow_index(
    prices,
    volume,
    period=5
)

print(f"Bulk Money Flow Index: {mfi}")
```

**Output:**
```
Bulk Money Flow Index: [
    78.66290018832392, 79.76062879599857, 53.912881261076386, 77.66179540709813, 
    79.8128443136367, 57.34244495064541, 78.54017792037334, 78.51354311163028, 
    78.32044198895028, 77.22254503195816, 77.55028992769704
]
```

---

### `rate_of_change`
```python
rate_of_change(
    prices: List[float]
) -> List[float]
```

#### Arguments
- **prices**: List of prices

#### Returns
List of Rate of Change values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

roc = mi.bulk.rate_of_change(prices)

print(f"Bulk Rate of Change: {roc}")
```

**Output:**
```
Bulk Rate of Change: [
    2.0, 2.941176470588235, -1.9047619047619049, 2.912621359223301, 1.8867924528301887, 
    -0.9259259259259258, 2.803738317757009, 1.8181818181818181, -2.6785714285714284, 
    1.834862385321101, 1.8018018018018018, 1.7699115044247788, -2.608695652173913,
    1.7857142857142856
]
```

---

### `on_balance_volume`
```python
on_balance_volume(
    prices: List[float],
    volume: List[float],
    previous_on_balance_volume: float
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **volume**: List of volumes
- **previous_on_balance_volume**: Previous OBV value (use 0.0 if none)

#### Returns
List of On Balance Volume values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]
volume = [
    1000, 1200, 1500, 1100, 1300, 1400, 1250, 1600,
    1800, 1350, 1450, 1550, 1700, 1400, 1500
]

obv = mi.bulk.on_balance_volume(
    prices,
    volume,
    previous_on_balance_volume=0.0
)

print(f"Bulk On Balance Volume: {obv}")
```

**Output:**
```
Bulk On Balance Volume: [
    1200.0, 2700.0, 1600.0, 2900.0, 4300.0, 3050.0, 4650.0, 6450.0, 
    5100.0, 6550.0, 8100.0, 9800.0, 8400.0, 9900.0
]
```

---

### `commodity_channel_index`
```python
commodity_channel_index(
    prices: List[float],
    constant_model_type: str,
    deviation_model: str,
    constant_multiplier: float,
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
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
- **constant_multiplier**: Scale factor (normally 0.015)
- **period**: Period over which to calculate the CCI

#### Returns
List of Commodity Channel Index values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

cci = mi.bulk.commodity_channel_index(
    prices,
    constant_model_type="simple_moving_average",
    deviation_model="standard_deviation",
    constant_multiplier=0.015,
    period=5
)

print(f"Bulk Commodity Channel Index: {cci}")
```

**Output:**
```
Bulk Commodity Channel Index: [
    87.4146757476247, 99.90248656871412, 46.49905549752783, 92.14596539534516, 
    105.22735830032957, -7.749842582921396, 46.49905549752783, 94.28090415820633, 
    100.0, 0.0, 47.14045207910316
]
```

---

### `mcginley_dynamic_commodity_channel_index`
```python
mcginley_dynamic_commodity_channel_index(
    prices: List[float],
    previous_mcginley_dynamic: float,
    deviation_model: str,
    constant_multiplier: float,
    period: int
) -> List[Tuple[float, float]]
```

#### Arguments
- **prices**: List of prices
- **previous_mcginley_dynamic**: Previous McGinley dynamic (0.0 if none)
- **deviation_model**: Choice of:
    - `"standard_deviation"`
    - `"mean_absolute_deviation"`
    - `"median_absolute_deviation"`
    - `"mode_absolute_deviation"`
    - `"ulcer_index"`
- **constant_multiplier**: Scale factor (normally 0.015)
- **period**: Period over which to calculate the CCI

#### Returns
List of tuples with (Commodity Channel Index, McGinley Dynamic)

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

md_cci = mi.bulk.mcginley_dynamic_commodity_channel_index(
    prices,
    previous_mcginley_dynamic=0.0,
    deviation_model="standard_deviation",
    constant_multiplier=0.015,
    period=5
)

print(f"Bulk McGinley Dynamic CCI: {md_cci}")
```

**Output:**
```
Bulk McGinley Dynamic CCI: [
    (0.0, 106.0), (50.850886978780416, 106.37118330162708), (19.606471661011987, 106.49401626029876), 
    (83.21929522843516, 107.11000103381146), (126.02325345778948, 107.92806406359112), 
    (33.551336476526465, 108.13414147145478), (91.04621305102224, 108.65037258300794), 
    (169.99349322038103, 109.39389344558838), (156.2681314408503, 110.31195605677449),
    (45.67777102116869, 110.62966686936494), (130.69756142487142, 111.22748604095784)
]
```

---

### `macd_line`
```python
macd_line(
    prices: List[float],
    short_period: int,
    short_period_model: str,
    long_period: int,
    long_period_model: str
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **short_period**: Length of the short period
- **short_period_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **long_period**: Length of the long period
- **long_period_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
List of MACD line values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

macd = mi.bulk.macd_line(
    prices,
    short_period=5,
    short_period_model="exponential_moving_average",
    long_period=10,
    long_period_model="exponential_moving_average"
)

print(f"Bulk MACD Line: {macd}")
```

**Output:**
```
Bulk MACD Line: [
    1.724347544444484, 1.4586781725636513, 1.6040869889352223, 
    1.8185082303110107, 1.0980111584955239, 1.1377855745084275
]
```

---

### `signal_line`
```python
signal_line(
    macds: List[float],
    constant_model_type: str,
    period: int
) -> List[float]
```

#### Arguments
- **macds**: List of MACD values
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **period**: Period over which to calculate the signal line

#### Returns
List of Signal line values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

macds = [
    -0.5, -0.3, 0.1, 0.4, 0.2, 0.6, 0.8, 0.5,
    0.3, 0.7, 0.9, 1.1, 0.8, 0.6, 1.0
]

signal = mi.bulk.signal_line(
    macds,
    constant_model_type="exponential_moving_average",
    period=9
)

print(f"Bulk Signal Line: {signal}")
```

**Output:**
```
Bulk Signal Line: [
    0.37228892577740375, 0.4750370938526215, 0.597235628312796, 0.728793463675819, 
    0.755436755350888, 0.7367513886909434, 0.8018030953629876
]
```

---

### `mcginley_dynamic_macd_line`
```python
mcginley_dynamic_macd_line(
    prices: List[float],
    short_period: int,
    previous_short_mcginley: float,
    long_period: int,
    previous_long_mcginley: float
) -> List[Tuple[float, float, float]]
```

#### Arguments
- **prices**: List of prices
- **short_period**: Length of the short period
- **previous_short_mcginley**: Previous short McGinley dynamic (0.0 if none)
- **long_period**: Length of the long period
- **previous_long_mcginley**: Previous long McGinley dynamic (0.0 if none)

#### Returns
List of tuples with (MACD, short McGinley dynamic, long McGinley dynamic)

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

md_macd = mi.bulk.mcginley_dynamic_macd_line(
    prices,
    short_period=5,
    previous_short_mcginley=0.0,
    long_period=10,
    previous_long_mcginley=0.0
)

print(f"Bulk McGinley Dynamic MACD: {md_macd}")
```

**Output:**
```
Bulk McGinley Dynamic MACD: [
    (0.0, 109.0, 109.0), (0.18597050580808627, 109.37194101161619, 109.1859705058081), 
    (0.4903282630853596, 110.00875741201432, 109.51842914892896), 
    (0.8753505778212656, 110.84466130783767, 109.9693107300164), 
    (0.9082933415511008, 111.06634122265551, 110.15804788110441), 
    (1.1019577190823497, 111.5949690859446, 110.49301136686225)
]
```

---

### `chaikin_oscillator`
```python
chaikin_oscillator(
    highs: List[float],
    lows: List[float],
    close: List[float],
    volume: List[float],
    short_period: int,
    long_period: int,
    previous_accumulation_distribution: float,
    short_period_model: str,
    long_period_model: str
) -> List[Tuple[float, float]]
```

#### Arguments
- **highs**: List of high prices
- **lows**: List of low prices
- **close**: List of closing prices
- **volume**: List of volumes
- **short_period**: Short period for AD calculation
- **long_period**: Long period for AD calculation
- **previous_accumulation_distribution**: Previous AD value (0.0 if none)
- **short_period_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **long_period_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
List of tuples with (Chaikin Oscillator, Accumulation Distribution)

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

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
volume = [
    1000, 1200, 1500, 1100, 1300, 1400, 1250, 1600,
    1800, 1350, 1450, 1550, 1700, 1400, 1500
]

chaikin = mi.bulk.chaikin_oscillator(
    high_prices,
    low_prices,
    close_prices,
    volume,
    short_period=5,
    long_period=10,
    previous_accumulation_distribution=0.0,
    short_period_model="simple_moving_average",
    long_period_model="simple_moving_average"
)

print(f"Bulk Chaikin Oscillator: {chaikin}")
```

**Output:**
```
Bulk Chaikin Oscillator: []
```

---

### `percentage_price_oscillator`
```python
percentage_price_oscillator(
    prices: List[float],
    short_period: int,
    long_period: int,
    constant_model_type: str
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **short_period**: Length of short period
- **long_period**: Length of long period
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
List of Percentage Price Oscillator values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

ppo = mi.bulk.percentage_price_oscillator(
    prices,
    short_period=5,
    long_period=10,
    constant_model_type="exponential_moving_average"
)

print(f"Bulk Percentage Price Oscillator: {ppo}")
```

**Output:**
```
Bulk Percentage Price Oscillator: [
    1.5979390071606525, 1.340906021403326, 1.4601143417976372, 
    1.6371566789984267, 0.9847709201141644, 1.0142521932946695
]
```

---

### `chande_momentum_oscillator`
```python
chande_momentum_oscillator(
    prices: List[float],
    period: int
) -> List[float]
```

#### Arguments
- **prices**: List of prices
- **period**: Period over which to calculate the CMO

#### Returns
List of Chande Momentum Oscillator values

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

cmo = mi.bulk.chande_momentum_oscillator(
    prices,
    period=10
)

print(f"Bulk Chande Momentum Oscillator: {cmo}")
```

**Output:**
```
Bulk Chande Momentum Oscillator: [42.857142857142854, 42.857142857142854, 40.0, 60.0, 30.0, 30.0]
```

---

## 🟢 Single Functions

### `relative_strength_index`
```python
relative_strength_index(
    prices: List[float],
    constant_model_type: str
) -> float
```

#### Arguments
- **prices**: List of prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
Relative Strength Index value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

rsi = mi.single.relative_strength_index(
    prices,
    constant_model_type="simple_moving_average"
)

print(f"Single RSI: {rsi}")
```

**Output:**
```
Single RSI: 50.54945054945055
```

---

### `stochastic_oscillator`
```python
stochastic_oscillator(
    prices: List[float]
) -> float
```

#### Arguments
- **prices**: List of prices

#### Returns
Stochastic Oscillator value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

stoch = mi.single.stochastic_oscillator(prices)

print(f"Single Stochastic Oscillator: {stoch}")
```

**Output:**
```
Single Stochastic Oscillator: 93.33333333333333
```

---

### `slow_stochastic`
```python
slow_stochastic(
    stochastics: List[float],
    constant_model_type: str
) -> float
```

#### Arguments
- **stochastics**: List of stochastic values
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
Slow Stochastic value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

stochastics = [
    80.0, 75.0, 70.0, 72.0, 68.0, 65.0, 70.0, 73.0,
    75.0, 78.0, 80.0, 77.0, 74.0, 76.0, 79.0
]

slow_stoch = mi.single.slow_stochastic(
    stochastics,
    constant_model_type="simple_moving_average"
)

print(f"Single Slow Stochastic: {slow_stoch}")
```

**Output:**
```
Single Slow Stochastic: 74.13333333333334
```

---

### `slowest_stochastic`
```python
slowest_stochastic(
    slow_stochastics: List[float],
    constant_model_type: str
) -> float
```

#### Arguments
- **slow_stochastics**: List of slow stochastic values
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
Slowest Stochastic value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

slow_stochastics = [
    75.0, 72.3, 71.7, 68.3, 67.7, 69.0, 72.7, 75.3,
    77.7, 78.3, 76.3, 75.0, 76.3, 78.0
]

slowest_stoch = mi.single.slowest_stochastic(
    slow_stochastics,
    constant_model_type="simple_moving_average"
)

print(f"Single Slowest Stochastic: {slowest_stoch}")
```

**Output:**
```
Single Slowest Stochastic: 73.82857142857142
```

---

### `williams_percent_r`
```python
williams_percent_r(
    high: List[float],
    low: List[float],
    close: float
) -> float
```

#### Arguments
- **high**: List of high prices
- **low**: List of low prices
- **close**: Closing price for the observed period

#### Returns
Williams %R value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

high_prices = [
    105.0, 107.0, 110.0, 108.0, 111.0, 113.0, 112.0, 115.0,
    117.0, 114.0, 116.0, 118.0, 120.0, 117.0, 119.0
]
low_prices = [
    95.0, 97.0, 100.0, 98.0, 101.0, 103.0, 102.0, 105.0,
    107.0, 104.0, 106.0, 108.0, 110.0, 107.0, 109.0
]

williams_r = mi.single.williams_percent_r(
    high_prices,
    low_prices,
    close=114.0
)

print(f"Single Williams %R: {williams_r}")
```

**Output:**
```
Single Williams %R: -24.0
```

---

### `money_flow_index`
```python
money_flow_index(
    prices: List[float],
    volume: List[float]
) -> float
```

#### Arguments
- **prices**: List of prices
- **volume**: List of volumes

#### Returns
Money Flow Index value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]
volume = [
    1000, 1200, 1500, 1100, 1300, 1400, 1250, 1600,
    1800, 1350, 1450, 1550, 1700, 1400, 1500
]

mfi = mi.single.money_flow_index(prices, volume)

print(f"Single Money Flow Index: {mfi}")
```

**Output:**
```
Single Money Flow Index: 74.95568383255306
```

---

### `rate_of_change`
```python
rate_of_change(
    current_price: float,
    previous_price: float
) -> float
```

#### Arguments
- **current_price**: Current price
- **previous_price**: Previous price

#### Returns
Rate of Change value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

roc = mi.single.rate_of_change(
    current_price=114.0,
    previous_price=100.0
)

print(f"Single Rate of Change: {roc}")
```

**Output:**
```
Single Rate of Change: 14.000000000000002
```

---

### `on_balance_volume`
```python
on_balance_volume(
    current_price: float,
    previous_price: float,
    current_volume: float,
    previous_on_balance_volume: float
) -> float
```

#### Arguments
- **current_price**: Current price
- **previous_price**: Previous price
- **current_volume**: Current volume
- **previous_on_balance_volume**: Previous OBV value (0.0 if none)

#### Returns
On Balance Volume value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

obv = mi.single.on_balance_volume(
    current_price=114.0,
    previous_price=112.0,
    current_volume=1500.0,
    previous_on_balance_volume=5000.0
)

print(f"Single On Balance Volume: {obv}")
```

**Output:**
```
Single On Balance Volume: 6500.0
```

---

### `commodity_channel_index`
```python
commodity_channel_index(
    prices: List[float],
    constant_model_type: str,
    deviation_model: str,
    constant_multiplier: float
) -> float
```

#### Arguments
- **prices**: List of prices
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
- **constant_multiplier**: Scale factor (normally 0.015)

#### Returns
Commodity Channel Index value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

cci = mi.single.commodity_channel_index(
    prices,
    constant_model_type="simple_moving_average",
    deviation_model="standard_deviation",
    constant_multiplier=0.015
)

print(f"Single Commodity Channel Index: {cci}")
```

**Output:**
```
Single Commodity Channel Index: 83.64657763003788
```

---

### `mcginley_dynamic_commodity_channel_index`
```python
mcginley_dynamic_commodity_channel_index(
    prices: List[float],
    previous_mcginley_dynamic: float,
    deviation_model: str,
    constant_multiplier: float
) -> Tuple[float, float]
```

#### Arguments
- **prices**: List of prices
- **previous_mcginley_dynamic**: Previous McGinley dynamic (0.0 if none)
- **deviation_model**: Choice of:
    - `"standard_deviation"`
    - `"mean_absolute_deviation"`
    - `"median_absolute_deviation"`
    - `"mode_absolute_deviation"`
    - `"ulcer_index"`
- **constant_multiplier**: Scale factor (normally 0.015)

#### Returns
Tuple with (Commodity Channel Index, McGinley Dynamic)

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

md_cci = mi.single.mcginley_dynamic_commodity_channel_index(
    prices,
    previous_mcginley_dynamic=0.0,
    deviation_model="standard_deviation",
    constant_multiplier=0.015
)

print(f"Single McGinley Dynamic CCI: {md_cci}")
```

**Output:**
```
Single McGinley Dynamic CCI: (0.0, 114.0)
```

---

### `macd_line`
```python
macd_line(
    prices: List[float],
    short_period: int,
    short_period_model: str,
    long_period_model: str
) -> float
```

#### Arguments
- **prices**: List of prices
- **short_period**: Length of the short period
- **short_period_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **long_period_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
MACD line value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

macd = mi.single.macd_line(
    prices,
    short_period=5,
    short_period_model="exponential_moving_average",
    long_period_model="exponential_moving_average"
)

print(f"Single MACD Line: {macd}")
```

**Output:**
```
Single MACD Line: 2.695637490797523
```

---

### `signal_line`
```python
signal_line(
    macds: List[float],
    constant_model_type: str
) -> float
```

#### Arguments
- **macds**: List of MACD values
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
Signal line value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

macds = [
    -0.5, -0.3, 0.1, 0.4, 0.2, 0.6, 0.8, 0.5,
    0.3, 0.7, 0.9, 1.1, 0.8, 0.6, 1.0
]

signal = mi.single.signal_line(
    macds,
    constant_model_type="exponential_moving_average"
)

print(f"Single Signal Line: {signal}")
```

**Output:**
```
Single Signal Line: 0.662108064232895
```

---

### `mcginley_dynamic_macd_line`
```python
mcginley_dynamic_macd_line(
    prices: List[float],
    short_period: int,
    previous_short_mcginley: float,
    previous_long_mcginley: float
) -> Tuple[float, float, float]
```

#### Arguments
- **prices**: List of prices
- **short_period**: Length of the short period
- **previous_short_mcginley**: Previous short McGinley dynamic (0.0 if none)
- **previous_long_mcginley**: Previous long McGinley dynamic (0.0 if none)

#### Returns
Tuple with (MACD, short McGinley dynamic, long McGinley dynamic)

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

md_macd = mi.single.mcginley_dynamic_macd_line(
    prices,
    short_period=5,
    previous_short_mcginley=0.0,
    previous_long_mcginley=0.0
)

print(f"Single McGinley Dynamic MACD: {md_macd}")
```

**Output:**
```
Single McGinley Dynamic MACD: (0.0, 114.0, 114.0)
```

---

### `chaikin_oscillator`
```python
chaikin_oscillator(
    highs: List[float],
    lows: List[float],
    close: List[float],
    volume: List[float],
    short_period: int,
    previous_accumulation_distribution: float,
    short_period_model: str,
    long_period_model: str
) -> Tuple[float, float]
```

#### Arguments
- **highs**: List of high prices
- **lows**: List of low prices
- **close**: List of closing prices
- **volume**: List of volumes
- **short_period**: Short period for AD calculation
- **previous_accumulation_distribution**: Previous AD value (0.0 if none)
- **short_period_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **long_period_model**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
Tuple with (Chaikin Oscillator, Accumulation Distribution)

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

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
volume = [
    1000, 1200, 1500, 1100, 1300, 1400, 1250, 1600,
    1800, 1350, 1450, 1550, 1700, 1400, 1500
]

chaikin = mi.single.chaikin_oscillator(
    high_prices,
    low_prices,
    close_prices,
    volume,
    short_period=5,
    previous_accumulation_distribution=0.0,
    short_period_model="exponential_moving_average",
    long_period_model="exponential_moving_average"
)

print(f"Single Chaikin Oscillator: {chaikin}")
```

**Output:**
```
Single Chaikin Oscillator: []
```

---

### `percentage_price_oscillator`
```python
percentage_price_oscillator(
    prices: List[float],
    short_period: int,
    constant_model_type: str
) -> float
```

#### Arguments
- **prices**: List of prices
- **short_period**: Length of short period
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`

#### Returns
Percentage Price Oscillator value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

ppo = mi.single.percentage_price_oscillator(
    prices,
    short_period=5,
    constant_model_type="exponential_moving_average"
)

print(f"Single Percentage Price Oscillator: {ppo}")
```

**Output:**
```
Single Percentage Price Oscillator: 2.436802783365856
```

---

### `chande_momentum_oscillator`
```python
chande_momentum_oscillator(
    prices: List[float]
) -> float
```

#### Arguments
- **prices**: List of prices

#### Returns
Chande Momentum Oscillator value

#### Example
```python
from pytechnicalindicators import momentum_indicators as mi

prices = [
    100.0, 102.0, 105.0, 103.0, 106.0, 108.0, 107.0, 110.0,
    112.0, 109.0, 111.0, 113.0, 115.0, 112.0, 114.0
]

cmo = mi.single.chande_momentum_oscillator(prices)

print(f"Single Chande Momentum Oscillator: {cmo}")
```

**Output:**
```
Single Chande Momentum Oscillator: 43.75
```

---

## 🧩 Model Type Choices

**constant_model_type:**  
- `"simple_moving_average"`
- `"smoothed_moving_average"`
- `"exponential_moving_average"`
- `"simple_moving_median"`
- `"simple_moving_mode"`

**deviation_model:**  
- `"standard_deviation"`
- `"mean_absolute_deviation"`
- `"median_absolute_deviation"`
- `"mode_absolute_deviation"`
- `"ulcer_index"`

---

## 📝 Notes

- All input lists must be of type `List[float]` (Python list of floats)
- Bulk functions require a `period` argument for window size
- Some functions require previous values for continuity
- Volume-based indicators require both price and volume data

---

## 🔗 See Also

- [API Reference Index](../api/index.md)

