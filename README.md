# TradingView - Selective Adaptive Technical Indicator Strategy

A comprehensive Pine Script v6 trading strategy that combines multiple technical indicators across different timeframes to generate high-confidence trading signals.

## 🚀 Features

### Multi-Category Indicator Selection
- **Trending Indicators**: SMA, EMA, MACD, ADX, Parabolic SAR
- **Volatility Indicators**: Bollinger Bands, ATR, Keltner Channel, Standard Deviation, Chaikin Volatility
- **Momentum Indicators**: RSI, Stochastic, CCI, Williams %R, MACD Histogram
- **Volume Indicators**: OBV, MFI, Chaikin Money Flow, Volume Oscillator, A/D Line

### Multi-Timeframe Analysis
- Current timeframe
- 5-minute signals
- 30-minute signals
- 1-hour signals
- 4-hour signals

### Strategy Presets
- **Default**: Balanced settings for general use
- **Optimal Performance**: Balanced settings for consistent performance
- **Breakout**: Optimized for catching market breakouts
- **Reversal**: Tuned for identifying trend reversals
- **Trend Dominator**: Maximized for strong trend following

### Signal Combination Strategies
- **All Agree**: All 4 indicators must agree (highest confidence)
- **Majority Vote**: 3 out of 4 indicators must agree
- **Custom Threshold**: Set your own threshold (1-4 indicators)

### Risk Management
- Configurable stop-loss percentage
- Configurable take-profit percentage
- Optional trailing stop-loss
- Daily trade limiting
- Maximum drawdown protection (15%)

### Visual Interface
- **Multi-Timeframe Table**: Shows all indicator signals across timeframes
- **Performance Metrics Table**: Real-time strategy performance data
- **Buy/Sell Signal Shapes**: Clear visual entry/exit points
- **Clean Chart Display**: No indicator clutter, focus on signals

## 📊 How It Works

1. **Indicator Calculation**: The strategy calculates all selected indicators for the current timeframe
2. **Multi-Timeframe Analysis**: Fetches signals from 5min, 30min, 1h, and 4h timeframes
3. **Signal Combination**: Combines individual indicator signals based on selected strategy
4. **Trade Execution**: Enters positions when signal criteria are met
5. **Risk Management**: Applies stop-loss, take-profit, and position sizing rules

## 🛠️ Configuration

### Preset Selection
Choose from 5 pre-configured strategy presets, each optimized for different market conditions:

```pinescript
preset = "Optimal Performance"  // Default recommendation
```

### Indicator Selection
Select one indicator from each category:
- Trending: Choose your preferred trend-following indicator
- Volatility: Select volatility measurement method
- Momentum: Pick momentum oscillator
- Volume: Choose volume analysis indicator

### Signal Strategy
Configure how indicators must agree:
- **All Agree**: Maximum confidence, fewer trades
- **Majority Vote**: Balanced approach
- **Custom Threshold**: Fine-tune sensitivity

### Risk Parameters
```pinescript
stopLossPercent = 2.0      // Stop loss percentage
takeProfitPercent = 5.0    // Take profit percentage
useTrailingStop = false    // Enable trailing stops
maxTradesPerDay = 1        // Limit daily trades
```

## 📈 Performance Metrics

The strategy displays real-time performance data:
- **Net Profit**: Total profit/loss with percentage
- **Win Rate**: Percentage of winning trades
- **Max Drawdown**: Maximum equity decline
- **Profit Factor**: Gross profit / Gross loss ratio
- **Total Trades**: Number of completed trades
- **Average Trade**: Average profit per trade
- **Last Signal**: Most recent trade action
- **Signal Time**: Timestamp of last signal

## 🎯 Usage Instructions

1. **Apply to Chart**: Add the script to any TradingView chart
2. **Select Timeframe**: Works on any timeframe (recommended: 1H or 4H)
3. **Choose Preset**: Start with "Optimal Performance" preset
4. **Configure Indicators**: Select preferred indicators from each category
5. **Set Risk Parameters**: Adjust stop-loss and take-profit levels
6. **Monitor Tables**: Watch multi-timeframe signals and performance metrics
7. **Follow Signals**: Enter/exit positions based on triangle signals

## 🔧 Advanced Features

### Leading vs Lagging Indicators
- **Leading (→)**: Predictive indicators that signal before price moves
- **Lagging (←)**: Confirmatory indicators that follow price action

### Multi-Timeframe Confluence
The strategy shows how signals align across different timeframes, helping identify high-probability setups when multiple timeframes agree.

### Adaptive Parameters
Each preset automatically adjusts all indicator parameters for optimal performance in specific market conditions.

## ⚠️ Risk Disclaimer

This strategy is for educational and research purposes. Past performance does not guarantee future results. Always:
- Test thoroughly on historical data
- Use proper position sizing
- Never risk more than you can afford to lose
- Consider market conditions and volatility
- Monitor performance regularly

## 📝 Version History

- **v6.0**: Initial release with Pine Script v6
- Multi-timeframe analysis
- Strategy presets
- Performance tables
- Risk management features

## 🤝 Contributing

Feel free to fork this project and submit improvements. Areas for enhancement:
- Additional indicator options
- More sophisticated signal filtering
- Enhanced risk management features
- Backtesting optimizations

## 📄 License

This project is open source and available under the MIT License.

---

**Created for TradingView Pine Script v6**  
*Selective Adaptive Technical Indicator Strategy* 