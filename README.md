# Four Dropdown Technical Indicator Strategy

A comprehensive Pine Script v6 trading strategy that combines four different categories of technical indicators with multi-timeframe analysis and configurable signal combination strategies.

## Strategy Overview

This strategy allows traders to select one indicator from each of four major categories:
- **Trending Indicators** - Identify market direction
- **Volatility Indicators** - Measure market volatility and potential breakouts
- **Momentum Indicators** - Detect overbought/oversold conditions
- **Volume Indicators** - Confirm price movements with volume analysis

### Key Features

- **Multi-Timeframe Analysis**: Displays signals across 5min, 30min, 1h, and 4h timeframes
- **Flexible Signal Combination**: Three strategies for combining indicator signals
- **Pre-configured Presets**: Optimized parameter sets for different trading styles
- **Advanced Risk Management**: Stop-loss, take-profit, and trailing stop options
- **Real-time Performance Tracking**: Live performance metrics and trade monitoring
- **Alert System**: Configurable alerts for buy/sell signals

## Available Indicators

### Trending Indicators
- **SMA (Simple Moving Average)** Leading
- **EMA (Exponential Moving Average)** Leading  
- **MACD (Moving Average Convergence Divergence)** Leading
- **ADX (Average Directional Index)** Lagging
- **Parabolic SAR** Lagging

### Volatility Indicators
- **Bollinger Bands** Leading
- **ATR (Average True Range)** Leading
- **Keltner Channel** Leading
- **Standard Deviation** Lagging
- **Chaikin Volatility** Lagging

### Momentum Indicators
- **RSI (Relative Strength Index)** Leading
- **Stochastic Oscillator** Leading
- **CCI (Commodity Channel Index)** Leading
- **Williams %R** Lagging
- **MACD Histogram** Lagging

### Volume Indicators
- **OBV (On-Balance Volume)** Leading
- **MFI (Money Flow Index)** Leading
- **Chaikin Money Flow** Leading
- **Volume Oscillator** Lagging
- **A/D Line (Accumulation/Distribution)** Lagging

## Strategy Presets

### Default
Standard settings suitable for general market conditions.

### Optimal Performance
Balanced settings designed for consistent performance across various market conditions.
- Fast MACD: 12, Slow: 26, Signal: 9
- Bollinger Bands: 20 periods, 2.0 deviation
- RSI: 14 periods, 70/30 levels
- Volume period: 14

### Breakout
Optimized for catching market breakouts with increased sensitivity.
- Fast MACD: 8, Slow: 21, Signal: 7
- Bollinger Bands: 15 periods, 2.5 deviation
- RSI: 10 periods, 65/35 levels
- Volume period: 10

### Reversal
Tuned for identifying trend reversals with extended parameters.
- Fast MACD: 6, Slow: 19, Signal: 5
- Bollinger Bands: 25 periods, 1.8 deviation
- RSI: 16 periods, 75/25 levels
- Volume period: 16

### Trend Dominator
Maximized for strong trend following with longer-term parameters.
- Fast MACD: 15, Slow: 30, Signal: 10
- Bollinger Bands: 30 periods, 2.2 deviation
- RSI: 12 periods, 68/32 levels
- Volume period: 12

## Signal Combination Strategies

### All Agree
Requires all four selected indicators to agree (all bullish or all bearish) before generating a signal. Most conservative approach with highest accuracy but fewer trades.

### Majority Vote
Generates signals when at least 3 out of 4 indicators agree. Balanced approach between signal frequency and reliability.

### Custom Threshold
Allows setting a custom number (1-4) of indicators required to agree. Provides maximum flexibility for different risk tolerances.

## Risk Management Features

### Stop Loss & Take Profit
- Configurable percentage-based stop loss (default: 2%)
- Configurable percentage-based take profit (default: 5%)
- Automatic position sizing based on account equity

### Trailing Stop
- Optional trailing stop-loss functionality
- Follows price movements to lock in profits
- Configurable trail distance and offset

### Trade Limiting
- Maximum trades per day setting (default: 1)
- Prevents over-trading and excessive risk exposure
- Automatic daily reset of trade counter

### Position Management
- Automatic position reversal on signal change
- Maximum drawdown protection (15% of equity)
- Real-time position monitoring and alerts

## Multi-Timeframe Analysis

The strategy analyzes signals across multiple timeframes:
- **Current Timeframe**: Primary trading signals
- **5 Minutes**: Short-term momentum confirmation
- **30 Minutes**: Medium-term trend validation
- **1 Hour**: Hourly trend alignment
- **4 Hours**: Long-term directional bias

### Timeframe Display Table
Real-time table showing:
- Individual indicator signals for each timeframe
- Combined signal strength per timeframe
- Color-coded visualization (Green: Bullish, Red: Bearish, Gray: Neutral)

## Performance Monitoring

### Live Performance Metrics
- **Net Profit**: Total profit/loss and percentage return
- **Win Rate**: Percentage of winning trades
- **Maximum Drawdown**: Largest peak-to-trough decline
- **Profit Factor**: Ratio of gross profit to gross loss
- **Total Trades**: Number of completed trades
- **Average Trade**: Average profit/loss per trade

### Trade Tracking
- **Last Signal**: Most recent trading action
- **Signal Time**: Timestamp of last signal
- **Active Preset**: Currently selected preset configuration
- **Preset Description**: Brief description of active preset

## Technical Implementation

### Pine Script v6 Features
- Modern Pine Script syntax and best practices
- Efficient memory usage with `var` declarations
- Method-based functions for multi-timeframe analysis
- Request.security() for cross-timeframe data retrieval

### Code Architecture

```pinescript
// Core Components:
1. Input Configuration System
2. Preset Parameter Management  
3. Indicator Calculation Engine
4. Signal Generation Logic
5. Multi-Timeframe Analysis
6. Risk Management System
7. Visualization Framework
8. Performance Tracking
```

### Key Functions

#### Multi-Timeframe Methods
- `trendingSignalMTF(tf)` - Calculate trending signals for any timeframe
- `volatilitySignalMTF(tf)` - Calculate volatility signals for any timeframe  
- `momentumSignalMTF(tf)` - Calculate momentum signals for any timeframe
- `volumeSignalMTF(tf)` - Calculate volume signals for any timeframe
- `combinedSignalMTF()` - Combine individual signals using selected strategy

#### Signal Processing
```pinescript
// Signal values: 1 (Bullish), -1 (Bearish), 0 (Neutral)
final_signal = signalStrategy == "All Agree" ? 
    (positive_signals >= 4 ? 1 : negative_signals >= 4 ? -1 : 0) :
    signalStrategy == "Majority Vote" ? 
    (positive_signals > negative_signals and positive_signals >= 3 ? 1 : 
     negative_signals > positive_signals and negative_signals >= 3 ? -1 : 0) :
    (positive_signals >= customThreshold ? 1 : 
     negative_signals >= customThreshold ? -1 : 0)
```

### Performance Optimizations
- Conditional plotting to reduce computational load
- Efficient table updates only on last bar
- Minimal security calls with batched requests
- Smart variable initialization with `var` keyword

## Usage Instructions

### For Traders

1. **Select Your Preset**: Choose from 5 pre-configured presets based on your trading style
2. **Choose Indicators**: Select one indicator from each category (or use preset defaults)
3. **Configure Signal Strategy**: Choose how indicators should be combined
4. **Set Risk Parameters**: Configure stop-loss, take-profit, and position sizing
5. **Enable Alerts**: Set up notifications for buy/sell signals
6. **Monitor Performance**: Track results using the built-in performance table

### For Developers

1. **Code Structure**: The script is organized into clear sections with comprehensive comments
2. **Customization**: Easy to add new indicators or modify existing ones
3. **Extension**: Multi-timeframe framework supports additional timeframes
4. **Integration**: Alert system compatible with external trading bots
5. **Testing**: Built-in backtesting with detailed performance metrics

## Configuration Options

### Input Groups
- **Strategy Presets**: Pre-configured parameter sets
- **Trending Indicator Settings**: Trend-following indicator selection
- **Volatility Indicator Settings**: Volatility measurement tools
- **Momentum Indicator Settings**: Momentum oscillator configuration
- **Volume Indicator Settings**: Volume analysis tools
- **Signal and Risk Management**: Signal combination and risk controls
- **Order Limiting**: Trade frequency and position management

### Alert Conditions
- **Buy Signal Alert**: Triggered when bullish signal is generated
- **Sell Signal Alert**: Triggered when bearish signal is generated
- **Custom Messages**: Configurable alert messages for external systems

## Visual Features

### Chart Overlays
- Buy/Sell signal markers (triangles)
- Selected indicator plots with appropriate styling
- Support/resistance levels for relevant indicators
- Color-coded signal strength visualization

### Information Tables
- **Multi-Timeframe Signals**: Real-time indicator status across timeframes
- **Performance Metrics**: Live trading statistics and results
- **Preset Information**: Active configuration details

## Advanced Features

### Custom ADX Implementation
Full custom implementation of Average Directional Index with:
- True Range calculation
- Directional Movement (+DM, -DM) computation
- Wilder's smoothing technique
- ADX threshold-based signal generation

### Dynamic Parameter Updates
Preset-based parameter updating system that:
- Modifies indicator parameters based on selected preset
- Maintains parameter consistency across timeframes
- Provides preset descriptions for user guidance

### Intelligent Position Management
- Automatic position reversal on signal changes
- Trade limiting to prevent over-trading
- Daily trade counter with automatic reset
- Maximum drawdown protection

## Best Practices

### For Optimal Performance
1. **Backtest Thoroughly**: Test different presets on historical data
2. **Start Conservative**: Begin with "All Agree" strategy for higher accuracy
3. **Monitor Drawdown**: Keep maximum drawdown below 15%
4. **Use Appropriate Timeframes**: Match strategy timeframe to trading style
5. **Regular Review**: Periodically assess and adjust parameters

### Risk Management
1. **Position Sizing**: Never risk more than 2% per trade
2. **Diversification**: Don't rely on a single timeframe or indicator
3. **Market Conditions**: Adjust presets based on market volatility
4. **Stop Losses**: Always use stop-loss orders
5. **Take Profits**: Set realistic profit targets

## Troubleshooting

### Common Issues
- **No Signals**: Check if "All Agree" is too conservative for current market
- **Too Many Signals**: Consider using "All Agree" or increasing custom threshold
- **Poor Performance**: Try different presets or adjust risk parameters
- **Missing Plots**: Ensure correct indicator is selected for visualization

### Performance Tips
- Use higher timeframes for less noise
- Combine with fundamental analysis
- Consider market session timing
- Monitor volume confirmation
- Watch for divergences between timeframes

## License & Disclaimer

This strategy is provided for educational and research purposes. Past performance does not guarantee future results. Always conduct thorough backtesting and consider your risk tolerance before live trading.

---

**Version**: Pine Script v6  
**Compatibility**: TradingView Platform  
**Last Updated**: 2024  
**Author**: Trading Strategy Developer
