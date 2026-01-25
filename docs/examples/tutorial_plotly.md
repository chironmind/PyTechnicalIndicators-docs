# Visualizing Indicators with Plotly example

A full example combining Centaur Technical Indicators with pandas and Plotly visualization
from [tutorial 1](../tutorials/pandas.md) and [tutorial 2](../tutorials/plotly.md).

## Full code

```python
import pandas as pd
import plotly.graph_objects as go
from plotly.subplots import make_subplots
from centaur_technical_indicators import moving_average as ma
from centaur_technical_indicators import candle_indicators as ci
from centaur_technical_indicators import momentum_indicators as mi
from centaur_technical_indicators import other_indicators as oi


df = pd.read_csv("prices.csv", parse_dates=["Date"])
df = df.sort_values("Date").reset_index(drop=True)

close = df["Close"].astype(float).tolist()
high  = df["High"].astype(float).tolist()
low   = df["Low"].astype(float).tolist()
open_ = df["Open"].astype(float).tolist()

# Bulk simple moving average (20-period)
period = 20
sma_series = ma.bulk.moving_average(close, period=period, moving_average_type="simple")
# Align back to DataFrame: last len(sma_series) rows correspond to rolling results
df.loc[df.index[-len(sma_series):], f"SMA_{period}"] = sma_series

# Bulk Bollinger Bands (20-period)
bands = ci.bulk.moving_constant_bands(
    close,
    constant_model_type="exponential_moving_average",
    deviation_model="standard_deviation",
    deviation_multiplier=2.0,
    period=period
)

# Unpack and assign (align to tail)
lower_vals, ema_vals, upper_vals = zip(*bands)
tail_index = df.index[-len(bands):]
df.loc[tail_index, "MCB_Lower"] = lower_vals
df.loc[tail_index, "MCB_EMA"] = ema_vals
df.loc[tail_index, "MCB_Upper"] = upper_vals

# Bulk RSI
rsi_values =  mi.bulk.relative_strength_index(
    close,
    constant_model_type="smoothed_moving_average",
    period=20
)

df.loc[df.index[-len(rsi_values):], "RSI"] = rsi_values

# Bulk ATR
atr_series = oi.bulk.average_true_range(
    close=close,
    high=high,
    low=low,
    constant_model_type="exponential_moving_average",
    period=period
)
df.loc[df.index[-len(atr_series):], f"ATR_{period}"] = atr_series

# Create multi-panel Plotly figure
fig = make_subplots(
    rows=3,
    cols=1,
    shared_xaxes=True,
    vertical_spacing=0.02,
    row_heights=[0.6, 0.2, 0.2],
    subplot_titles=("Price & Overlays", "RSI", "ATR")
)

# --- Row 1: Candlestick ---
fig.add_trace(
    go.Candlestick(
        x=df["Date"],
        open=df["Open"],
        high=df["High"],
        low=df["Low"],
        close=df["Close"],
        name="Price",
        increasing_line_color="#26a69a",
        decreasing_line_color="#ef5350",
        showlegend=True
    ),
    row=1, col=1
)

# --- Row 1: SMA ---
sma_col = "SMA_20"
if sma_col in df.columns:
    fig.add_trace(
        go.Scatter(
            x=df["Date"], y=df[sma_col],
            name=sma_col,
            line=dict(color="orange", width=1.3),
            hovertemplate="SMA %{y:.2f}<extra></extra>"
        ),
        row=1, col=1
    )

# --- Row 1: Moving Constant Bands (shaded) ---
if {"MCB_Lower","MCB_Upper","MCB_EMA"}.issubset(df.columns):
    fig.add_trace(
        go.Scatter(
            x=df["Date"], y=df["MCB_Upper"],
            name="MCB Upper",
            line=dict(color="royalblue", width=1),
            hovertemplate="Upper %{y:.2f}<extra></extra>",
            opacity=0.7
        ),
        row=1, col=1
    )
    fig.add_trace(
        go.Scatter(
            x=df["Date"], y=df["MCB_Lower"],
            name="MCB Lower",
            line=dict(color="royalblue", width=1),
            fill="tonexty",
            fillcolor="rgba(65,105,225,0.15)",
            hovertemplate="Lower %{y:.2f}<extra></extra>",
            opacity=0.7
        ),
        row=1, col=1
    )
    fig.add_trace(
        go.Scatter(
            x=df["Date"], y=df["MCB_EMA"],
            name="MCB Mid",
            line=dict(color="royalblue", width=0.8, dash="dot"),
            hovertemplate="Mid %{y:.2f}<extra></extra>",
            opacity=0.6
        ),
        row=1, col=1
    )

# --- Row 2: RSI ---
if "RSI" in df.columns:
    fig.add_trace(
        go.Scatter(
            x=df["Date"], y=df["RSI"],
            name="RSI (20)",
            line=dict(color="purple"),
            hovertemplate="RSI %{y:.2f}<extra></extra>"
        ),
        row=2, col=1
    )
    # Overbought / Oversold reference lines
    fig.add_hline(y=70, line_dash="dash", line_color="red", row=2, col=1)
    fig.add_hline(y=30, line_dash="dash", line_color="green", row=2, col=1)

# --- Row 3: ATR ---
atr_col = "ATR_20"
if atr_col in df.columns:
    fig.add_trace(
        go.Bar(
            x=df["Date"], y=df[atr_col],
            name=atr_col,
            marker_color="gray",
            hovertemplate="ATR %{y:.2f}<extra></extra>",
            opacity=0.7
        ),
        row=3, col=1
    )

fig.update_layout(
    title=f"Technical Indicators Dashboard",
    legend=dict(orientation="h", yanchor="bottom", y=1.02, xanchor="right", x=1),
    xaxis_rangeslider_visible=False,
    template="plotly_white",
    margin=dict(l=40, r=40, t=60, b=40),
    hovermode="x unified"
)

# Improve y-axis titles
fig.update_yaxes(title_text="Price", row=1, col=1)
fig.update_yaxes(title_text="RSI",   row=2, col=1, range=[0,100])
fig.update_yaxes(title_text="ATR",   row=3, col=1)

fig.show()

# Optional: static export
# fig.write_image("technical_dashboard.png", scale=2, width=1400, height=900)
```

## Run the code

```shell
python3 your_file_name.py
```

## Expected output

An interactive Plotly chart will open in your browser showing:
- Candlestick chart with price data
- SMA overlay on the price chart
- Moving Constant Bands (shaded area) on the price chart
- RSI indicator in a separate panel with 70/30 threshold lines
- ATR indicator in a third panel

The chart will be interactive, allowing you to zoom, pan, and hover over data points.
