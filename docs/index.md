![Centaur Technical Indicators Banner](assets/pytechnicalindicators_banner.png)

# Centaur Technical Indicators

**Fast, configurable technical indicators for Python — powered by Rust.**

Centaur Technical Indicators is a Python library delivering technical analysis with the speed and safety of Rust.  
Built for quants, traders, and developers who demand robust, high-performance financial analytics.

---

## 🚀 Features

- **Fast**: Rust-powered core for microsecond-level performance.
- **Comprehensive**: 60+ indicators, from classics (RSI, MACD, SMA) to advanced (McGinley, Ulcer).
- **Configurable**: Most indicators offer multiple calculation models, rolling/bulk or single/scalar outputs.
- **Research-Ready**: Designed for backtesting, data science, and live analytics.
- **Easy to use**: Consistent API, pandas-friendly, works in Jupyter.

---

## ⚡ Quickstart

Install:

```bash
pip install centaur_technical_indicators
```

First indicator:

```python
import centaur_technical_indicators as cti
prices = [100.2, 100.46, 100.53, 100.38, 100.19]
ma = cti.moving_average(prices, "simple")
print(f"Simple Moving Average: {ma}")
```

---

## 📚 Indicator Coverage

Centaur Technical Indicators covers:

- **Standard**: SMA, EMA, Bollinger Bands, MACD, RSI
- **Candle**: Ichimoku Cloud, Moving Constant Bands, Donchian, Keltner, Supertrend
- **Momentum**: Chaikin Oscillator, CCI, MFI, OBV, ROC, Williams %R
- **Moving Average**: McGinley Dynamic, classic MA types
- **Trend**: Aroon, Parabolic, TSI, Volume-Price Trend
- **Correlation**: Asset price correlations
- **Strength**: Accumulation/Distribution, PVI, NVI, RVI
- **Volatility**: Ulcer Index, Volatility System
- **Other**: ROI, ATR, True Range, IBS

[See full API Reference](api/index.md)

---

## 📖 Documentation

- [Tutorials](tutorials/index.md) — Start-to-finish workflows for real-world analysis
- [How-To Guides](howto/index.md) — Task-focused, practical recipes
- [API Reference](api/index.md) — Full indicator and function docs
- [Benchmarks](benchmarks/index.md) — Performance tables by indicator and dataset size
- [Source code](https://github.com/chironmind/PyTechnicalIndicators) — Where the magic happens

---

## 🗂️ Source Code

Find Centaur Technical Indicators source code on [GitHub](https://github.com/chironmind/CentaurTechnicalIndicators-Python).

---

## 🤝 Community & Contributing

- [GitHub Discussions](https://github.com/chironmind/CentaurTechnicalIndicators-Python/discussions)
- [Open an Issue](https://github.com/chironmind/CentaurTechnicalIndicators-Python/issues)
- [Contributing Guide](https://github.com/chironmind/CentaurTechnicalIndicators-Python/blob/main/CONTRIBUTING.md)

---

## 📄 License

MIT License

