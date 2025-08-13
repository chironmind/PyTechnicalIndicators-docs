# 🕯️ Candle Indicators API Reference

**Module:** `candle_indicators`  

Candle indicators are technical indicators designed for use with candlestick price charts.  
They help identify trends, volatility, and price action patterns commonly used in trading and analysis.

## 📚 When to Use

- Analyze support/resistance, volatility bands, and price channels on candle charts.
- Suitable for both traditional and crypto assets.

## 🏗️ Structure

- **single**: Functions that return a single value for a slice of prices.
- **bulk**: Functions that compute values of a slice of prices over a period and return a vector.

---

## 🚀 Bulk Functions

Bulk functions operate over a moving window and return a list of values for the entire data series.

### `moving_constant_envelopes`
```python
moving_constant_envelopes(
    prices: List[float],
    constant_model_type: str,
    difference: float,
    period: int
) -> List[Tuple[float, float, float]]
```

#### Arguments
- **prices**: List of prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **difference**: Percent band width (e.g., `3.0` for ±3%)
- **period**: Period over which to calculate the moving constant envelopes

#### Returns
List of tuples (lower envelope, constant model result, upper envelope)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

prices = [
    100.0, 102.0, 103.0, 102.5, 102.8, 103.1, 103.8, 103.9, 
    104.4, 103.6, 103.1, 102.9, 103.3, 103.7
]

mces = ci.bulk.moving_constant_envelopes(
    prices,
    constant_model_type="simple_moving_average",
    difference=2.0,
    period=5
)

print(f"Bulk Moving Constant Envelopes: {mces}")
```

**Output:**
```
Bulk Moving Constant Envelopes: [
    (100.0188, 102.06, 104.1012), (100.62639999999999, 102.67999999999999, 104.7336), 
    (100.97919999999999, 103.03999999999999, 105.10079999999999), 
    (101.15559999999999, 103.22, 105.2844), (101.52799999999999, 103.6, 105.672), 
    (101.6848, 103.75999999999999, 105.83519999999999), 
    (101.68480000000002, 103.76000000000002, 105.83520000000001), (101.5084, 103.58, 105.6516), 
    (101.3908, 103.46, 105.52919999999999), (101.2536, 103.32000000000001, 105.38640000000001)
]
```

---

### `mcginley_dynamic_envelopes`
```python
mcginley_dynamic_envelopes(
    prices: List[float],
    difference: float,
    previous_mcginley_dynamic: float,
    period: int
) -> List[Tuple[float, float, float]]
```

#### Arguments
- **prices**: List of prices
- **difference**: Percent band width (e.g., `3.0` for ±3%)
- **previous_mcginley_dynamic**: Previous McGinley dynamic (use `0.0` if none)
- **period**: Period over which to calculate the McGinley dynamic envelopes

#### Returns
List of McGinley dynamic envelopes tuple (lower envelope, McGinley dynamic, upper envelope)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

prices = [
    100.0, 102.0, 103.0, 102.5, 102.8, 103.1, 103.8, 103.9,
    104.4, 103.6, 103.1, 102.9, 103.3, 103.7
]

mdes = ci.bulk.mcginley_dynamic_envelopes(
    prices,
    difference=2.0,
    previous_mcginley_dynamic=0.0,
    period=5
)

print(f"Bulk McGinley Dynamic Envelopes: {mdes}")
```

**Output**
```
Bulk McGinley Dynamic Envelopes: [
    (100.744, 102.8, 104.856), (100.80211859724395, 102.85930469106525, 104.91649078488655), 
    (100.9799014916942, 103.04071580785123, 105.10153012400825), 
    (101.14281840437332, 103.20695755548297, 105.27109670659263), 
    (101.36614779143355, 103.43484468513627, 105.50354157883899), 
    (101.3983123111345, 103.46766562360663, 105.53701893607877), 
    (101.32521640748107, 103.39307796681742, 105.46093952615377), 
    (101.22670738127877, 103.29255855232527, 105.35840972337178), 
    (101.22816548479628, 103.29404641305743, 105.35992734131858), 
    (101.3064937650942, 103.37397322968796, 105.44145269428172)
]
```

---

### `moving_constant_bands`
```python
moving_constant_bands(
    prices: List[float],
    constant_model_type: str,
    deviation_model: str,
    deviation_multiplier: float,
    period: int
) -> List[Tuple[float, float, float]]
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
- **deviation_multiplier**: Price deviation multiplier
- **period**: Period over which to calculate the moving constant bands

#### Returns
List of Moving constant bands tuple (lower band, constant model result, upper band)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

prices = [
    100.0, 102.0, 103.0, 102.5, 102.8, 103.1, 103.8, 103.9,
    104.4, 103.6, 103.1, 102.9, 103.3, 103.7
]

mcbs = ci.bulk.moving_constant_bands(
    prices,
    constant_model_type="simple_moving_average",
    deviation_model="standard_deviation",
    deviation_multiplier=2.0,
    period=5
)

print(f"Bulk Moving Constant Bands: {mcbs}")
```
 
**Output**
```
Bulk Moving Constant Bands: [
    (99.89260525053695, 102.06, 104.22739474946306), 
    (101.88602267034882, 102.67999999999999, 103.47397732965116), 
    (102.17651867420308, 103.03999999999999, 103.9034813257969), 
    (102.12163758258032, 103.22, 104.31836241741968), 
    (102.44761117672896, 103.6, 104.75238882327103), 
    (102.91525151672226, 103.75999999999999, 104.60474848327772), 
    (102.9152515167223, 103.76000000000002, 104.60474848327775), 
    (102.49630262526847, 103.58, 104.66369737473153), 
    (102.41233593170328, 103.46, 104.5076640682967), 
    (102.72133481811618, 103.32000000000001, 103.91866518188384)
]
```

---

### `mcginley_dynamic_bands`
```python
mcginley_dynamic_bands(
    prices: List[float],
    deviation_model: str,
    deviation_multiplier: float,
    previous_mcginley_dynamic: float,
    period: int
) -> List[Tuple[float, float, float]]
```

#### Arguments
- **prices**: List of prices
- **deviation_model**: Choice of:
    - `"standard_deviation"`
    - `"mean_absolute_deviation"`
    - `"median_absolute_deviation"`
    - `"mode_absolute_deviation"`
    - `"ulcer_index"`
- **deviation_multiplier**: Price deviation multiplier
- **previous_mcginley_dynamic**: Previous McGinley dynamic (`0.0` if none)
- **period**: Period over which to calculate the moving constant bands

#### Returns
List of McGinley dynamic bands tuple (lower band, McGinley dynamic, upper band)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

prices = [
    100.0, 102.0, 103.0, 102.5, 102.8, 103.1, 103.8, 103.9,
    104.4, 103.6, 103.1, 102.9, 103.3, 103.7
]

mdbs = ci.bulk.mcginley_dynamic_bands(
    prices,
    deviation_model="standard_deviation",
    deviation_multiplier=2.0,
    previous_mcginley_dynamic=0.0,
    period=5
)

print(f"Bulk McGinley Dynamic Bands: {mdbs}")
```

**Output**
```
Bulk McGinley Dynamic Bands: [
    (100.63260525053694, 102.8, 104.96739474946305), 
    (102.06532736141408, 102.85930469106525, 103.65328202071642), 
    (102.17723448205432, 103.04071580785123, 103.90419713364814), 
    (102.1085951380633, 103.20695755548297, 104.30531997290265), 
    (102.28245586186523, 103.43484468513627, 104.58723350840731), 
    (102.62291714032891, 103.46766562360663, 104.31241410688436), 
    (102.54832948353969, 103.39307796681742, 104.23782645009514), 
    (102.20886117759375, 103.29255855232527, 104.3762559270568), 
    (102.24638234476072, 103.29404641305743, 104.34171048135414), 
    (102.77530804780413, 103.37397322968796, 103.9726384115718)
]
```

---

### `ichimoku_cloud`
```python
ichimoku_cloud(
    highs: List[float],
    lows: List[float],
    close: List[float],
    conversion_period: int,
    base_period: int,
    span_b_period: int
) -> List[Tuple[float, float, float, float, float]]
```

#### Arguments
- **highs**: List of price highs
- **lows**: List of price lows
- **close**: List of closing prices
- **conversion_period**: Period used to calculate the conversion line
- **base_period**: Period used to calculate the base line
- **span_b_period**: Period used to calculate the Span B line

#### Returns
A list of tuples, each tuple contains (leading span a, leading span b, base line, conversion line, relevant close price)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

high_prices = [
    105.0, 103.0, 107.0, 101.0, 103.0, 100.0, 109.0, 105.0,
    110.0, 112.0, 111.0, 105.0, 106.0, 100.0, 103.0, 102.0, 98.0
]
low_prices = [
    97.0, 99.0, 98.0, 100.0, 95.0, 98.0, 99.0, 100.0, 102.0, 106.0,
    99.0, 101.0, 98.0, 93.0, 98.0, 91.0, 89.0
]
closing_prices = [
    100.0, 102.0, 103.0, 101.0, 99.0, 99.0, 102.0, 103.0, 106.0,
    107.0, 105.0, 104.0, 101.0, 97.0, 100.0, 96.0, 93.0
]

icloud = ci.bulk.ichimoku_cloud(
    high_prices,
    low_prices,
    closing_prices,
    conversion_period=5,
    base_period=10,
    span_b_period=15
)

print(f"Bulk Ichimoku Cloud: {icloud}")
```

**Output**
```
Bulk Ichimoku Cloud: [
    (102.25, 102.5, 102.5, 102.0, 99.0), (100.0, 101.5, 101.5, 98.5, 102.0), (99.0, 100.5, 100.5, 97.5, 103.0)
]
```

---

### `donchian_channels`
```python
donchian_channels(
    high: List[float],
    low: List[float],
    period: int
) -> List[Tuple[float, float, float]]
```

#### Arguments
- **high**: List of price highs
- **low**: List of price lows
- **period**: Period over which to calculate the Donchian channels

#### Returns
List of Donchian channel tuples (lower, average, upper)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

high_prices = [
    105.0, 103.0, 107.0, 101.0, 103.0, 100.0, 109.0, 105.0,
    110.0, 112.0, 111.0, 105.0, 106.0, 100.0, 103.0, 102.0, 98.0
]
low_prices = [
    97.0, 99.0, 98.0, 100.0, 95.0, 98.0, 99.0, 100.0, 102.0, 106.0,
    99.0, 101.0, 98.0, 93.0, 98.0, 91.0, 89.0
]

dc = ci.bulk.donchian_channels(
    high_prices,
    low_prices,
    period=5
)

print(f"Bulk Donchian Channels: {dc}")
```

**Output**
```
Bulk Donchian Channels: [
    (95.0, 101.0, 107.0), (95.0, 101.0, 107.0), (95.0, 102.0, 109.0), (95.0, 102.0, 109.0), 
    (95.0, 102.5, 110.0), (98.0, 105.0, 112.0), (99.0, 105.5, 112.0), (99.0, 105.5, 112.0), 
    (98.0, 105.0, 112.0), (93.0, 102.5, 112.0), (93.0, 102.0, 111.0), (91.0, 98.5, 106.0), 
    (89.0, 97.5, 106.0)
]
```

---

### `keltner_channel`
```python
keltner_channel(
    high: List[float],
    low: List[float],
    close: List[float],
    constant_model_type: str,
    atr_constant_model_type: str,
    multiplier: float,
    period: int
) -> List[Tuple[float, float, float]]
```

#### Arguments
- **highs**: List of price highs
- **lows**: List of price lows
- **close**: List of closing prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **atr_constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **multiplier**: Multiplier for the ATR
- **period**: Period over which to calculate the Keltner Channel

#### Returns
List of Keltner channel tuples

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

high_prices = [
    105.0, 103.0, 107.0, 101.0, 103.0, 100.0, 109.0, 105.0,
    110.0, 112.0, 111.0, 105.0, 106.0, 100.0, 103.0, 102.0, 98.0
]
low_prices = [
    97.0, 99.0, 98.0, 100.0, 95.0, 98.0, 99.0, 100.0, 102.0, 106.0,
    99.0, 101.0, 98.0, 93.0, 98.0, 91.0, 89.0
]
closing_prices = [
    100.0, 102.0, 103.0, 101.0, 99.0, 99.0, 102.0, 103.0, 106.0,
    107.0, 105.0, 104.0, 101.0, 97.0, 100.0, 96.0, 93.0
]

kc = ci.bulk.keltner_channel(
    high_prices,
    low_prices,
    closing_prices,
    constant_model_type="simple_moving_average",
    atr_constant_model_type="simple_moving_average",
    multiplier=2.0,
    period=10
)

print(f"Bulk Keltner Channel: {kc}")
```

**Output**
```
Bulk Keltner Channel: [
    (90.16666666666667, 102.36666666666667, 114.56666666666668),
    (89.8, 102.8, 115.8), (90.0, 103.0, 116.0), 
    (90.10000000000002, 102.90000000000002, 115.70000000000002), 
    (88.5, 102.5, 116.5), 
    (89.23333333333332, 102.63333333333333, 116.03333333333333), 
    (87.16666666666666, 102.36666666666666, 117.56666666666666),
    (86.36666666666667, 101.36666666666667, 116.36666666666667)
]
```
---

### `supertrend`
```python
supertrend(
    high: List[float],
    low: List[float],
    close: List[float],
    constant_model_type: str,
    multiplier: float,
    period: int
) -> List[float]
```

#### Arguments
- **highs**: List of price highs
- **lows**: List of price lows
- **close**: List of closing prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **multiplier**: Multiplier for the ATR
- **period**: Period over which to calculate the Keltner Channel

#### Returns
List of Super Trend indicators

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

high_prices = [
    105.0, 103.0, 107.0, 101.0, 103.0, 100.0, 109.0, 105.0,
    110.0, 112.0, 111.0, 105.0, 106.0, 100.0, 103.0, 102.0, 98.0
]
low_prices = [
    97.0, 99.0, 98.0, 100.0, 95.0, 98.0, 99.0, 100.0, 102.0, 106.0,
    99.0, 101.0, 98.0, 93.0, 98.0, 91.0, 89.0
]
closing_prices = [
    100.0, 102.0, 103.0, 101.0, 99.0, 99.0, 102.0, 103.0, 106.0,
    107.0, 105.0, 104.0, 101.0, 97.0, 100.0, 96.0, 93.0
]

sptrnd = ci.bulk.supertrend(
    high_prices,
    low_prices,
    closing_prices,
    constant_model_type="simple_moving_average",
    multiplier=2.0,
    period=10
)

print(f"Bulk Supertend : {sptrnd}")
```

**Output**
```
Bulk Supertend : [115.7, 116.5, 116.5, 116.3, 116.5, 115.9, 116.7, 115.5]
```

---

## 🟢 Single Functions

Single functions compute a single value for the latest available data slice.

### `moving_constant_envelopes`
```python
moving_constant_envelopes(
    prices: List[float],
    constant_model_type: str,
    difference: float
) -> Tuple[float, float, float]
```

#### Arguments
- **prices**: List of prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **difference**: Percent band width (e.g., `3.0` for ±3%)

#### Returns
Moving Constant Envelopes tuples (lower envelope, constant model result, upper envelope)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

prices = [
    100.0, 102.0, 103.0, 102.5, 102.8, 103.1, 103.8, 103.9,
    104.4, 103.6, 103.1, 102.9, 103.3, 103.7
]

mce = ci.single.moving_constant_envelopes(
    prices,
    constant_model_type="simple_moving_average",
    difference=2.0,
)

print(f"Single Moving Constant Envelopes: {mce}")
```

**Output:**
```
Single Moving Constant Envelopes: (100.94699999999999, 103.00714285714285, 105.06728571428572)
```

---

### `mcginley_dynamic_envelopes`
```python
mcginley_dynamic_envelopes(
    prices: List[float],
    difference: float,
    previous_mcginley_dynamic: float
) -> Tuple[float, float, float]
```

#### Arguments
- **prices**: List of prices
- **difference**: Percent band width (e.g., `3.0` for ±3%)
- **previous_mcginley_dynamic**: Previous McGinley dynamic (use `0.0` if none)

#### Returns
McGinley dynamic envelopes tuple (lower envelope, McGinley dynamic, upper envelope)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

prices = [
    100.0, 102.0, 103.0, 102.5, 102.8, 103.1, 103.8, 103.9,
    104.4, 103.6, 103.1, 102.9, 103.3, 103.7
]

mde = ci.single.mcginley_dynamic_envelopes(
    prices,
    difference=2.0,
    previous_mcginley_dynamic=0.0,
)

print(f"Single McGinley Dynamic Envelopes: {mde}")
```

**Output**
```
Single McGinley Dynamic Envelopes: (101.626, 103.7, 105.774)
```

---

### `moving_constant_bands`
```python
moving_constant_bands(
    prices: List[float],
    constant_model_type: str,
    deviation_model: str,
    deviation_multiplier: float
) -> Tuple[float, float, float]
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
- **deviation_multiplier**: Price deviation multiplier

#### Returns
Moving constant bands tuple (lower band, constant model result, upper band)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

prices = [
    100.0, 102.0, 103.0, 102.5, 102.8, 103.1, 103.8, 103.9,
    104.4, 103.6, 103.1, 102.9, 103.3, 103.7
]

mcb = ci.single.moving_constant_bands(
    prices,
    constant_model_type="simple_moving_average",
    deviation_model="standard_deviation",
    deviation_multiplier=2.0,
)

print(f"Single Moving Constant Bands: {mcb}")
```

**Output**
```
Single Moving Constant Bands: (100.95989482874084, 103.00714285714285, 105.05439088554486)
```

---

### `mcginley_dynamic_bands`
```python
mcginley_dynamic_bands(
    prices: List[float],
    deviation_model: str,
    deviation_multiplier: float,
    previous_mcginley_dynamic: float
) -> Tuple[float, float, float]
```

#### Arguments
- **prices**: List of prices
- **deviation_model**: Choice of:
    - `"standard_deviation"`
    - `"mean_absolute_deviation"`
    - `"median_absolute_deviation"`
    - `"mode_absolute_deviation"`
    - `"ulcer_index"`
- **deviation_multiplier**: Price deviation multiplier
- **previous_mcginley_dynamic**: Previous McGinley dynamic (`0.0` if none)

#### Returns
McGinley dynamic bands tuple (lower band, McGinley dynamic, upper band)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

prices = [
    100.0, 102.0, 103.0, 102.5, 102.8, 103.1, 103.8, 103.9,
    104.4, 103.6, 103.1, 102.9, 103.3, 103.7
]

mdb = ci.single.mcginley_dynamic_bands(
    prices,
    deviation_model="standard_deviation",
    deviation_multiplier=2.0,
    previous_mcginley_dynamic=0.0,
)

print(f"Single McGinley Dynamic Bands: {mdb}")
```

**Output**
```
Single McGinley Dynamic Bands: (101.652751971598, 103.7, 105.74724802840201)
```

---

### `ichimoku_cloud`
```python
ichimoku_cloud(
    highs: List[float],
    lows: List[float],
    close: List[float],
    conversion_period: int,
    base_period: int,
    span_b_period: int
) -> Tuple[float, float, float, float, float]
```

#### Arguments
- **highs**: List of price highs
- **lows**: List of price lows
- **close**: List of closing prices
- **conversion_period**: Period used to calculate the conversion line
- **base_period**: Period used to calculate the base line
- **span_b_period**: Period used to calculate the Span B line

#### Returns
(leading span a, leading span b, base line, conversion line, relevant close price)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

high_prices = [
    105.0, 103.0, 107.0, 101.0, 103.0, 100.0, 109.0, 105.0,
    110.0, 112.0, 111.0, 105.0, 106.0, 100.0, 103.0, 102.0, 98.0
]
low_prices = [
    97.0, 99.0, 98.0, 100.0, 95.0, 98.0, 99.0, 100.0, 102.0, 106.0,
    99.0, 101.0, 98.0, 93.0, 98.0, 91.0, 89.0
]
closing_prices = [
    100.0, 102.0, 103.0, 101.0, 99.0, 99.0, 102.0, 103.0, 106.0,
    107.0, 105.0, 104.0, 101.0, 97.0, 100.0, 96.0, 93.0
]

icloud = ci.single.ichimoku_cloud(
    high_prices,
    low_prices,
    closing_prices,
    conversion_period=5,
    base_period=10,
    span_b_period=15
)

print(f"Single Ichimoku Cloud: {icloud}")
```

**Output**
```
Single Ichimoku Cloud: (99.0, 100.5, 100.5, 97.5, 103.0)
```

---

### `donchian_channels`
```python
donchian_channels(
    high: List[float],
    low: List[float]
) -> Tuple[float, float, float]
```

#### Arguments
- **high**: List of price highs
- **low**: List of price lows

#### Returns
Donchian channel tuple (lower, average, upper)

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

high_prices = [
    105.0, 103.0, 107.0, 101.0, 103.0, 100.0, 109.0, 105.0,
    110.0, 112.0, 111.0, 105.0, 106.0, 100.0, 103.0, 102.0, 98.0
]
low_prices = [
    97.0, 99.0, 98.0, 100.0, 95.0, 98.0, 99.0, 100.0, 102.0, 106.0,
    99.0, 101.0, 98.0, 93.0, 98.0, 91.0, 89.0
]

dc = ci.single.donchian_channels(
    high_prices,
    low_prices,
)

print(f"Single Donchian Channels: {dc}")
```

**Output**
```
Single Donchian Channels: (89.0, 100.5, 112.0)
```

---

### `keltner_channel`
```python
keltner_channel(
    high: List[float],
    low: List[float],
    close: List[float],
    constant_model_type: str,
    atr_constant_model_type: str,
    multiplier: float
) -> Tuple[float, float, float]
```

#### Arguments
- **highs**: List of price highs
- **lows**: List of price lows
- **close**: List of closing prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **atr_constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **multiplier**: Multiplier for the ATR

#### Returns
Keltner channel tuple

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

high_prices = [
    105.0, 103.0, 107.0, 101.0, 103.0, 100.0, 109.0, 105.0,
    110.0, 112.0, 111.0, 105.0, 106.0, 100.0, 103.0, 102.0, 98.0
]
low_prices = [
    97.0, 99.0, 98.0, 100.0, 95.0, 98.0, 99.0, 100.0, 102.0, 106.0,
    99.0, 101.0, 98.0, 93.0, 98.0, 91.0, 89.0
]
closing_prices = [
    100.0, 102.0, 103.0, 101.0, 99.0, 99.0, 102.0, 103.0, 106.0,
    107.0, 105.0, 104.0, 101.0, 97.0, 100.0, 96.0, 93.0
]

kc = ci.single.keltner_channel(
    high_prices,
    low_prices,
    closing_prices,
    constant_model_type="simple_moving_average",
    atr_constant_model_type="simple_moving_average",
    multiplier=2.0,
)

print(f"Single Keltner Channel: {kc}")
```

**Output**
```
Single Keltner Channel: (87.4313725490196, 101.19607843137254, 114.96078431372548)
```

---

### `supertrend`
```python
supertrend(
    high: List[float],
    low: List[float],
    close: List[float],
    constant_model_type: str,
    multiplier: float
) -> float
```

#### Arguments
- **highs**: List of price highs
- **lows**: List of price lows
- **close**: List of closing prices
- **constant_model_type**: Choice of:
    - `"simple_moving_average"`
    - `"smoothed_moving_average"`
    - `"exponential_moving_average"`
    - `"simple_moving_median"`
    - `"simple_moving_mode"`
- **multiplier**: Multiplier for the ATR

#### Returns
List of Super Trend indicators

#### Example
```python
from pytechnicalindicators import candle_indicators as ci

high_prices = [
    105.0, 103.0, 107.0, 101.0, 103.0, 100.0, 109.0, 105.0,
    110.0, 112.0, 111.0, 105.0, 106.0, 100.0, 103.0, 102.0, 98.0
]
low_prices = [
    97.0, 99.0, 98.0, 100.0, 95.0, 98.0, 99.0, 100.0, 102.0, 106.0,
    99.0, 101.0, 98.0, 93.0, 98.0, 91.0, 89.0
]
closing_prices = [
    100.0, 102.0, 103.0, 101.0, 99.0, 99.0, 102.0, 103.0, 106.0,
    107.0, 105.0, 104.0, 101.0, 97.0, 100.0, 96.0, 93.0
]

sptrnd = ci.single.supertrend(
    high_prices,
    low_prices,
    closing_prices,
    constant_model_type="simple_moving_average",
    multiplier=2.0,
)

print(f"Single Supertend : {sptrnd}")
```

**Output**
```
Single Supertend : 114.26470588235294
```

---

## 🧩 Model Type Choices

Many functions accept model types as string arguments.  
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

- All input lists must be of type `List[float]` (Python list of floats).
- Bulk functions require a `period` argument for window size.
- Some functions require previous values (`previous_mcginley_dynamic`).

---

## 🔗 See Also

- [API Reference](../api/index.md)

---

