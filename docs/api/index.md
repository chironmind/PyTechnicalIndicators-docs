# API Reference

Comprehensive documentation for all modules, classes, and functions available in Centaur Technical Indicators, as well as a full indicator list.

## Quick Navigation

- **[Candle Indicators](candle-indicators.md)** - Moving constant bands, envelopes, Ichimoku Cloud...
- **[Chart Trends](chart-trends.md)** - Peaks, valleys, overall trends, break down trends...
- **[Correlation Indicators](correlation-indicators.md)** - Correlate asset prices
- **[Momentum Indicators](momentum-indicators.md)** - RSI, MACD, stochastic oscillators, rate of change...
- **[Moving Average](moving-average.md)** - Moving average, McGinley Dynamic
- **[Other Indicators](other-indicators.md)** - True Range, internal bar strength, return on investment...
- **[Strength Indicators](strength-indicators.md)** - Relative vigor index, +ve/-ve volumw inswz, accumulation distribution
- **[Trend Indicators](trend-indicators.md)** - Aroon up/down, oscillator, and indicator, volume price trend...
- **[Volatility Indicators](volatility-indicators.md)** - Ulcer index, volatiltiy sytem

---

## Shared Types & Conventions

Many indicator functions use shared model types and enums as string arguments.

### Constant Model Types

Accepted strings:

- `"simple"`, `"ma"`, `"simple_moving_average"`
- `"smoothed"`, `"sma"`, `"smoothed_moving_average"`
- `"exponential"`, `"ema"`, `"exponential_moving_average"`
- `"median"`, `"smm"`, `"simple_moving_median"`
- `"mode"`, `"simple_moving_mode"`

### Deviation Models

Accepted strings:

- `"standard"`, `"std"`, `"standard_deviation"`
- `"mean"`, `"mean_absolute_deviation"`
- `"median"`, `"median_absolute_deviation"`
- `"mode"`, `"mode_absolute_deviation"`
- `"ulcer"`, `"ulcer_index"`

### Moving Average Types

Accepted strings:

- `"simple"`
- `"smoothed"`
- `"exponential"`

### Position Types

Accepted strings:

- `"long"`
- `"short"`

**Error Handling:**  
If you pass an invalid string, the API raises a Python `ValueError` with a list of valid options.

---

## Package Structure

Centaur Technical Indicators is organized into indicator categories and modules:

```python
import centaur_technical_indicators

centaur_technical_indicators.candle_indicators
centaur_technical_indicators.chart_trends
centaur_technical_indicators.correlation_indicators
centaur_technical_indicators.momentum_indicators
centaur_technical_indicators.moving_average
centaur_technical_indicators.other_indicators
centaur_technical_indicators.standard_indicators
centaur_technical_indicators.strength_indicators
centaur_technical_indicators.trend_indicators
centaur_technical_indicators.volatility_indicators
```

Each submodule contains indicator functions, often organized into:
- `single` (single value for latest data slice)
- `bulk` (vectorized/moving window results)

**Example usage:**
```python
from centaur_technical_indicators.candle_indicators.bulk import supertrend
result = supertrend(high, low, close, "simple_moving_average", 3.0, 14)
```

---

## Complete Indicators List

Welcome to the comprehensive catalog of Centaur Technical Indicators! This page lists all 60+ technical indicators available in the library, organized by category for easy navigation.

### Candle Indicators

Candle indicators analyze price patterns and chart formations from OHLC charts.

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
| **Moving Constant Envelopes** | `moving_constant_envelopes()` | Price channels | Prices | `difference`, `constant_model_type`, `period` _(bulk only)_ |
| **Mcginley Dynamic Envelopes** | `mcginley_dynamic_envelopes()` | Price channels using the McGinley Dynamic | Prices | `difference`, `previous_mcginley_dynamic`, `period` _(bulk only)_ |
| **Moving Constant Bands** | `moving_constant_bands()` | Price channels with deviation | Prices | `constant_model_type`, `deviation_model`, `deviation_multiplier`, `period` _(bulk only)_ |
| **Mcginley Dynamic Bands** | `mcginley_dynamic_bands()` | Price channels using the McGinley Dynamic and deviation | Prices | `deviation_model`, `deviation_multiplier`, `previous_mcginley_dynamic`, `period` _(bulk only)_ |
| **Ichimoku Cloud** | `ichimoku_cloud()` | Complete Ichimoku system | High, Low, Close | `conversion_period`, `base_period`, `span_b_period` |
| **Donchian Channels** | `donchian_channels()` | Highest high, lowest low channels | High, Low prices | `period` _(bulk only)_ |
| **Keltner Channels** | `keltner_channels()` | ATR-based price channels | High, Low, Close | `constant_model_type`, `atr_constant_model_type`, `multiplier`, `period` _(bulk only)_ |
| **Supertrend** | `supertrend()` | Trend-following indicator | High, Low, Close | `constant_model_type`, `multiplier`, `period` _(bulk only)_ |

#### Example Usage
```python
import centaur_technical_indicators as cti

# Simple Moving Average with Standard Deviation Bands
bb = cti.candle_indicators.bulk.moving_constant_bands(close, period=20, constant_model_type="simple_moving_average", deviation_model="standard_deviation", deviation_multiplier=2)

# Ichimoku Cloud
ichimoku = cti.candle_indicators.bulk.ichimoku_cloud(highs, lows, closes, conversion_period=9, conversion_period=26, span_b_period=52)

# Donchian Channels
donchian = cti.candle_indicators.bulk.donchian_channels(highs, lows, period=20)

# Keltner Channels
keltner = cti.candle_indicators.single.keltner_channels(highs, lows, closes, multiplier=2i, constant_model_type="simple_moving_average", atr_constant_model_type="simple_moving_average")

# Supertrend
supertrend = cti.candle_indicators.single.supertrend(highs, lows, closes, constant_model_type="simple_moving_average", multiplier=3)
```

### Chart Trends

Chart trends detect trend in price charts

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
|**Peaks** | `peaks()` | Get all peaks for a period| Prices | `period`, `closest_neighbor` |
|**Valleys** | `valleys()` | Get all valleys for a period| Prices | `period`, `closest_neighbor` |
|**Peak Trends** | `peak_trend()` | Calculate the trend of the peaks| Prices | `period` |
|**Valley Trends** | `valley_trend()` | Calculate the trend of the valleys| Prices | `period` |
|**Overall Trend** | `overall_trend()` | Calculates the overall trend of prices | Prices | |
|**Breakdown Trends** | `break_down_trends()` | Splits the price trends into differrent parts| Prices | `max_outliers`, `soft_r_squared_minimum`, `soft_r_squared_maximum`, `hard_r_squared_minimum`, `hard_r_squared_maximum`, `soft_standard_error_multiplier`, `hard_standard_error_multiplier`, `soft_reduced_chi_squared_multiplier`, `hard_reduced_chi_squared_multiplier` |

#### Example Usage

```python
# Get all peaks within 5 periods of each other within a 10 period window
peaks = cti.chart_trends.peaks(prices, period=10, closest_neighbor=5)

# Get the trend of valleys over a 5 period window
valley_trend = valley_trend(prices, period=5)
```

### Correlation Indicators

Correlation indicators determine the correlation between two assets

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
| **Correlate Asset Prices** | `correlate_asset_prices()` | Calculate the correlation between two assests | Prices of both assets | `constant_model_type`, `deviation_model`, `period` _(bulk only)_ |

#### Example Usage

```python
# Correlation between a and b over the past 5 periods
corr = cti.correlation_indicators.bulk(prices_asset_a, prices_asset_b, constant_model_type="simple_moving_average", deviation_model="standard_deviation", period=5)
```

### Momentum Indicators

Momentum indicators measure the speed and strength of price movements.

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
| **RSI** | `relative_strength_index()` | Relative Strength Index | Prices | `constant_model_type`, `period` _(bulk only)_ |
| **Stochastic Oscillator** | `stochastic_oscillator()` | Stochastic oscillator | Prices | `period` _(bulk only)_ |
| **Slow Stochastic Oscillator** | `slow_stochastic()` | Slow stochastic oscillator | Stochastics | `constant_model_type`, `period` _(bulk only)_ |
| **Slowest Stochastic Oscillator** | `slowest_stochastic()` | Slowest stochastic oscillator | Slow stochastics | `constant_model_type`, `period` _(bulk only)_ |
| **MACD** | `macd_line()` | Moving Average Convergence Divergence | Prices | `short_period`, `short_period_model`, `long_period_model`, `long_period` _(bulk only)_ |
| **Signal Line** | `signal_line()` | Signal line for the MACD | MACDs | `constant_model_type`, `period` _(bulk only)_ |
| **McGinley Dynamic MACD** | `macd_line()` | MACD using McGinley Dynamic | Prices | `short_period`, `previous_short_mcginley`, `previous_long_mcginley`, `long_period` _(bulk only)_ |
| **Williams %R** | `williams_percent_r()` | Williams Percent Range | High, Low, Close | `period` _(bulk only)_ |
| **RoC** | `rate_of_change()` | Rate of change | Prices |  |
| **MFI** | `money_flow_index()` | Money Flow Index | Prices, Volume | `period` _(bulk only)_ |
| **Commodity Channel Index** | `commodity_channel_index()` | CCI oscillator | Prices | `constant_model_type`, `deviation_model`, `constant_multiplier`, `period` _(bulk only)_ |
| **McGinley Dynamic CCI** | `mcginley_dynamic_commodity_channel_index()` | CCI oscillator using the McGinley Dynamic | Prices | `previous_mcginley_dynamic`, `deviation_model`, `constant_multiplier`, `period` _(bulk only)_ |
| **OBV** | `on_balance_volume()` | On Balance Volume | Prices, Volume | `previous_on_balance_volume` |
| **Chaikin Oscillator** | `chaikin_oscillator()` | Chaikin Oscillator (CO) | High, Low, Close, Volume | `short_period`, `previous_accumulation_distribution`, `short_period_model`, `long_period_model`, `long_period` _(bulk only)_ |
| **PPO** | `percentage_price_oscillator()` | Percentage Price Oscillator | Prices | `short_period`, `constant_model_type`, `long_period` _(bulk only)_ |
| **CMO** | `chande_momentum_oscillator()` | Chande Momentum Oscillator | Prices | `period` _(bulk only)_ |

#### Example Usage
```python
# RSI
rsi = cti.momentum_indicators.bulk.relative_strength_index(close, constant_model_type="simple_moving_average", period=14)

# MACD
macd = cti.momentum_indicators.bulk.macd_line(close, short_period=12, long_period=26, short_period_model="simple_moving_average", long_period_model="simple_moving_average")
signal = cti.momentum_indicators.bulk.signal_line(macd, period=9)

# Stochastic
stoch = cti.momentum_indicators.bulk.stochastic_oscillator(close, period=14)

# Williams %R
williams = cti.momentum_indicators.single.williams_r(highs, lows, close)

# Money Flow Index
mfi = cti.momentum_indicators.single.money_flow_index(close, volumes)
```

### Moving Average

Calculate various moving averages and the McGinley dynamic.

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
| **Moving Average** | `moving_average()` | Generic Moving Average | Prices | `moving_average_type`, `period` _(bulk only)_ |
| **McGinley Dynamic** | `mcginley_dynamic()` | McGinley Dynamic | Prices | `previous_mcginley_dynamic`, `period` |

#### Example Usage
```python
# Different MAs for the past 5 periods
simple_ma = cti.moving_average.bulk.moving_average(prices, moving_average_type="simple", period=5)
smoothed_ma = cti.moving_average.bulk.moving_average(prices, moving_average_type="smoothed", period=5)
exponential_ma = cti.moving_average.bulk.moving_average(prices, moving_average_type="exponential", period=5)

# McGinley Dynamic
bulk_mcginley = cti.moving_average.bulk.mcginley_dynamic(prices, previous_mcginley_dynamic=0.0, period=5)
next_mcginley = cti.moving_average.single.mcginley_dynamic(next_price, previous_mcginley_dynamic=bulk_mcginley[-1], period=5
```

### Other Indicators

Indicators that don't fall into a specific category.

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
| **Return on Investment** | `return_on_investment()` | ROI | Prices | `investment` |
| **True Range** | `true_range()` | True Range (TR) | High, Low, Close |  |
| **Average True Range** | `average_true_range()` | Average True Range (ATR) | High, Low, Close | `constant_model_type`, `period` _(bulk only)_ |
| **Internal Bar Strength** | `internal_bar_strength()` | Internal Bar Strength (IBS) | High, Low, Close |  |
| **Positivity Indicator** | `positivity_indicator()` | Positivity Indicator | Open, Close | `signal_period`, `constant_model_type` |

#### Example Usage
```python
# Calculate the Return on an 1000 investment
roi = cti.other_indicators.bulk.return_on_investment(prices, investment=1000)

# Calculate the internal bar strengrh for the last bar
ibs = cti.other_indicators.single.internal_bar_strength(high[-1], low[-1], close[-1]
```

### Standard Indicators

Indicators with the defaults pre set to match they traditionally are.

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
| **Simple Moving Average** | `simple_moving_average()` | Simple Moving Average | Prices | `period` _(bulk only)_ |
| **Smoothed Moving Average** | `smoothed_moving_average()` | Smoothed Moving Average | Prices | `period` _(bulk only)_ |
| **Exponential Moving Average** | `exponential_moving_average()` | Exponential Moving Average | Prices | `period` _(bulk only)_ |
| **Bollinger Bands** | `bollinger_bands()` | Bollinger Bands | Prices | |
| **MACD** | `macd()` | MACD, signal line, and histogram | Prices | |
| **RSI** | `rsi()` | Relative Strength Index| Prices | |

#### Example Usage
```python
rsi = cti.standard_indicators.bulk.rsi(prices)

macd, signal, hist = cti.standard_indicators.bulk.macd(prices)
```

### Strength Indicators

Strength indicators analyze market participation and buying/selling pressure.

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
| **Accumulation/Distribution** | `accumulation_distribution()` | A/D Line | High, Low, Close, Volume | `previous_accumulation_distribution` |
| **Volume Index** _single only_ | `volume_index()` | `current_price`, `previous_price` | `previous_volume_index`
| **Positive Volume Index** _bulk only_ | `positive_volume_index()` | PVI volume indicator | Close, Volume | `previous_volume_index` |
| **Negative Volume Index** _bulk only_ | `negative_volume_index()` | NVI volume indicator | Close, Volume | `previous_volume_index` |
| **Relative Vigor Index** | `relative_vigor_index()` | RVI strength indicator | Open, High, Low, Close | `constant_model_type`, `period` _(bulk only)_ |

#### Example Usage
```python
# Volume Index Indicators
pvi = cti.strength_indicators.bulk.positive_volume_index(close, volume, previous_volume_index=0.0)
nvi = cti.strength_indicators.bulk.negative_volume_index(close, volume, previous_volume_index=0.0)

# Relative Vigor Index
rvi = cti.strength_indicators.single.relative_vigor_index(open, high, low, close, constant_model_type="simple_moving_average")
```

### Trend Indicators

Trend indicators help identify the direction and strength of market trends.

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
| **Aroon Oscillator** | `aroon_oscillator()` | Measures trend strength and direction | Aroon Ups, Aroon Downs | |
| **Aroon Up** | `aroon_up()` | Uptrend strength indicator | High prices | `period` _(bulk only)_ |
| **Aroon Down** | `aroon_down()` | Downtrend strength indicator | Low prices | `period` _(bulk only)_ |
| **Aroon Indicator** | `aroon_indicator()` | Measures trend strength and direction | High, Low prices | `period` _(bulk only)_ |
| **Long Parabolic Time Price System** _single only_ | `long_parabolic_time_price_system()` | Long Stop and Reverse trend indicator | | `previous_sar`, `extreme_point`, `af`, `low` |
| **Short Parabolic Time Price System** _single only_ | `short_parabolic_time_price_system()` | Short  Stop and Reverse trend indicator | | `previous_sar`, `extreme_point`, `af`, `high` |
| **Parabolic SaR** _bulk only_ | `parabolic_time_price_system()` | Stop and Reverse trend indicator | High, Low prices | `af_start`, `af_step`, `af_max`, `position`, `previous_sar` |
| **True Strength Index** | `true_strength_index()` | Double-smoothed momentum oscillator | Prices | `first_constant_model`, `second_constant_model`, `first_period`, `second_period` _(bulk only)_ |
| **Directional Movement System** _bulk only_ | `directional_movement_system ()` | Directional Movement System |  High, Low, Close | `period`, `constant_model_type` |
| **Volume Price Trend** | `volume_price_trend()` | Volume Price Trend (VPT) | Prices, Volumes | `previous_vpt` |

#### Example Usage
```python
# Aroon Indicators
aroon_up = cti.trend_indicators.bulk.aroon_up(highs, period=14)
aroon_down = cti.trend_indicators.bulk.aroon_down(lows, period=14)
aroon_oscillator = cti.trend_indicators.bulk.aroon_oscillator(aroon_up, aroon_down)

# Parabolic SAR
psar = cti.trend_indicators.bulk.parabolic_time_price_system(highs, lows, af_start=0.02, af_step=0.02, af_max=0.2, position="long", previous_sar=0)
```

### Volatility Indicators

Volatility indicators measure market volatility and price range movements.

| Indicator | Function Name | Description | Input Data | Parameters |
|-----------|---------------|-------------|------------|------------|
| **Ulcer Index** | `ulcer_index()` | Downside volatility measure | Prices | `period` _(bulk only)_ |
| **Volatility System** _bulk only_ | `volatility_system()` | Volatility-based signals | High, Low, Close | `period`, `constant_multiplier`, `constant_model_type` |

#### Example Usage
```python
# Ulcer Index
ui = cti.volatility_indicators.bulk.ulcer_index(close, period=14)

# Volatility System
vs = cti.volatility_indicators.bulk.volatility_system(high, low, close, period=14, constant_multiplier=2.0, constant_model_type="simple_moving_average")
```

### Parameter Conventions

| Parameter Type | Description | Example |
|----------------|-------------|---------|
| **period** | Number of periods for calculation | `period=14` |
| **constant_model_type** | Type of constant model to use | `constant_model_type="exponential_moving_average"` |
| **constant_multiplier** | Multiplier for the constant model used | `constant_multiplier=2.0` |
| **deviation_model** | Type of deviation model to use | `deviation_model="median_aboslute_deviation"` |
| **deviation_multiplier** | Multiplier for the deviation model | `deviation_multiplier=2.0` |

---

## Data Requirements

### Data Format
```python
# All data should be provided as Python lists
closes = [100.0, 101.5, 99.8, 102.3, 103.1]
highs = [101.0, 102.0, 100.5, 103.0, 104.0]
lows = [99.5, 100.8, 99.0, 101.5, 102.0]
volumes = [10000, 12000, 9500, 15000, 11000]
```

---

**🎉 That's all 60+ indicators available in Centaur Technical Indicators!**

