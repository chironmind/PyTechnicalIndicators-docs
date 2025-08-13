![PyTechnicalIndicators Banner](assets/pytechnicalindicators_banner.png)

# PyTechnicalIndicators

**Fast, configurable technical indicators for Python — powered by Rust.**

PyTechnicalIndicators is a Python library delivering technical analysis with the speed and safety of Rust.  
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
pip install pytechnicalindicators
```

First indicator:

```python
import pytechnicalindicators as pti
prices = [100.2, 100.46, 100.53, 100.38, 100.19]
ma = pti.moving_average(prices, "simple")
print(f"Simple Moving Average: {ma}")
```

---

## 📚 Indicator Coverage

PyTechnicalIndicators covers:

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

Find PyTechnicalIndicators source code on [GitHub](https://github.com/chironmind/PyTechnicalIndicators).

---

## 🤝 Community & Contributing

- [GitHub Discussions](https://github.com/chironmind/PyTechnicalIndicators/discussions)
- [Open an Issue](https://github.com/chironmind/PyTechnicalIndicators/issues)
- [Contributing Guide](CONTRIBUTING.md)

---

## 📄 License

MIT License

