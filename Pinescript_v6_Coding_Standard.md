# Pine Script v6 Coding Standard - Institutional Trading System

## Overview
This document establishes mandatory coding standards for Pine Script v6 institutional trading systems, combining professional-grade requirements with technical compatibility solutions to ensure maintainable, reliable, and error-free code.

## CRITICAL RULES - ZERO TOLERANCE

### Rule 1: NO EMOJIS ALLOWED
**STRICTLY FORBIDDEN: Use of emojis anywhere in the codebase**

#### Prohibited Locations:
- Pine Script code files (.pinescript)
- Comments within code
- Documentation files (.md)
- Commit messages
- Variable names, function names, string literals
- Error messages and user interfaces

#### Examples:
```pinescript
// FORBIDDEN - Never use emojis
// 🚀 Buy signal activated
// 📊 Performance table
// ✅ Validation passed
// ❌ Error detected

// CORRECT - Use descriptive text
// Buy signal activated
// Performance table  
// Validation passed
// Error detected
```

#### Rationale:
1. **Character Encoding**: Emojis cause UTF-8/ASCII encoding conflicts
2. **Platform Compatibility**: Inconsistent rendering across systems
3. **Professional Standards**: Institutional code must be emoji-free
4. **Parsing Issues**: May interfere with Pine Script compilation
5. **Code Clarity**: Text descriptions are more precise and searchable
6. **Version Control**: Emojis cause git diff/merge issues

### Rule 2: MANDATORY SINGLE-LINE FORMAT
**CRITICAL: All executable Pine Script code must be on single lines**

Pine Script v6 has strict line continuation rules that cause "end of line without line continuation" errors. The ONLY reliable solution is single-line format for all executable code.

#### Root Causes of Line Continuation Errors:
- Improper continuation syntax
- Invisible characters or encoding issues
- Missing continuation markers
- Indentation problems
- Multi-line ternary operators
- Multi-line boolean expressions
- Multi-line arithmetic expressions

## TECHNICAL IMPLEMENTATION RULES

### Rule 3: Single-Line Function Calls
**All function calls with multiple parameters must be on single lines**

```pinescript
// ✅ CORRECT - Single line format
kernelType = input.string("Rational Quadratic", "ML Kernel", options=["Rational Quadratic", "Gaussian", "Periodic", "Locally Periodic"], group="Core Settings")
var table mtfTable = table.new(position.bottom_right, 6, 6, bgcolor=color.new(color.black, 80), border_width=1, border_color=color.gray)
plotshape(buySignal, title="Buy Signal", location=location.belowbar, style=shape.triangleup, size=size.small, color=color.green)
strategy.exit("Long Exit", "Long", stop=close * (1 - stopLossPercent / 100), limit=close * (1 + takeProfitPercent / 100))

// ❌ INCORRECT - Multi-line format (CAUSES ERRORS)
kernelType = input.string(
    defval   = "Rational Quadratic",
    title    = "ML Kernel",
    options  = ["Rational Quadratic", "Gaussian", "Periodic", "Locally Periodic"],
    group    = "Core Settings"
)
```

### Rule 4: Single-Line Expressions
**All ternary operators, boolean expressions, and arithmetic must be on single lines**

```pinescript
// ✅ CORRECT - Single line expressions
smoothedSignal = smoothedPrediction > 0.2 ? 1 : smoothedPrediction < -0.2 ? -1 : 0
shouldEnterLong = buySignal and strategy.position_size == 0 and (optimizationMode != "Conservative" or lcConfidence > 0.6)
weightedSignal = (lcSignal * lcWeight) + (signalCurrent * mtfWeight) + (smoothedSignal * kernelWeight) + (adaptiveSignal * adaptiveWeight)
signal = condition1 ? value1 : condition2 ? value2 : condition3 ? value3 : defaultValue

// ❌ INCORRECT - Multi-line expressions (ERROR PRONE)
smoothedSignal = smoothedPrediction > 0.2 ? 1 : 
                smoothedPrediction < -0.2 ? -1 : 0
shouldEnterLong = buySignal and strategy.position_size == 0 and 
                 (optimizationMode != "Conservative" or lcConfidence > 0.6)
```

### Rule 5: Single-Line Object Constructors
**All object constructors and complex assignments must be on single lines**

```pinescript
// ✅ CORRECT - Single line constructors
newZone = SupplyDemandZone.new(topPrice, bottomPrice, time[pivotIndex], time, "Supply", strength, 0, true, zoneVolume, momentum, bar_index - pivotIndex, strength)
config = RiskConfig.new(maxDrawdown = 15.0, maxPosition = 10.0, maxDailyLoss = 3.0, maxLosses = 5, volatilityMult = 1.5, correlation = 0.8, trailingStop = true, breakevenStop = true, timeExit = true)

// ❌ INCORRECT - Multi-line constructors (ERROR PRONE)
newZone = SupplyDemandZone.new(
    topPrice, bottomPrice, time[pivotIndex], time, "Supply", 
    strength, 0, true, zoneVolume, momentum, bar_index - pivotIndex, strength)
```

## PROFESSIONAL CODING STANDARDS

### Rule 6: Professional Naming Conventions
```pinescript
// Variables: camelCase
var float currentPrice = 0.0
var int signalCount = 0
var bool isSignalActive = false

// Functions: camelCase with descriptive names
calculateMovingAverage(source, length) =>
    ta.sma(source, length)

calculateRiskMetrics(equity, riskPercent) =>
    if equity <= 0 or riskPercent <= 0
        runtime.error("Invalid parameters for risk calculation")
    equity * (riskPercent / 100)

// Constants: UPPER_CASE
MAX_LOOKBACK_BARS = 5000
DEFAULT_RSI_LENGTH = 14
MIN_POSITION_SIZE = 0.01

// Types: PascalCase
type TradingSignal
    float confidence
    int direction
    string reason
    int timestamp

type RiskConfig
    float maxDrawdown
    float positionSize
    bool trailingStop
    bool breakevenStop
```

### Rule 7: Comment Standards
```pinescript
// Single-line comments: Start with capital, end without period
// This is a proper comment format

// Multi-line documentation blocks for functions
// Function: calculateRiskMetrics
// Purpose: Determines position sizing based on volatility and equity
// Parameters: equity (float) - Account equity, riskPercent (float) - Risk percentage
// Returns: Risk amount in base currency units
// Example: calculateRiskMetrics(10000.0, 2.0) returns 200.0
```

### Rule 8: Error Handling and Validation
**Always validate inputs and handle edge cases**

```pinescript
// Input validation with proper bounds
rsiLength = input.int(14, "RSI Length", minval=2, maxval=200)
if rsiLength < 2 or rsiLength > 200
    runtime.error("RSI Length must be between 2 and 200")

// Defensive programming for calculations
calculatePositionSize(equity, riskPercent, stopDistance) =>
    if equity <= 0 or riskPercent <= 0 or stopDistance <= 0
        runtime.error("Invalid parameters for position size calculation")
    
    riskAmount = equity * (riskPercent / 100)
    positionSize = riskAmount / stopDistance
    math.max(positionSize, MIN_POSITION_SIZE)  // Ensure minimum position size
```

### Rule 9: Code Organization
- Group related functionality together
- Use consistent spacing and indentation (4 spaces)
- Separate major sections with comment dividers
- Follow logical code flow: inputs → types → calculations → outputs

```pinescript
// ═══════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
// INPUTS AND CONFIGURATION
// ═══════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════

// Core settings
rsiLength = input.int(14, "RSI Length", minval=2, maxval=200, group="Technical Settings")
maLength = input.int(20, "Moving Average Length", minval=1, maxval=500, group="Technical Settings")

// ═══════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
// TYPE DEFINITIONS
// ═══════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════

type TradingSignal
    float confidence
    int direction
    string reason

// ═══════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
// CALCULATIONS
// ═══════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════

// Technical indicators
rsiValue = ta.rsi(close, rsiLength)
maValue = ta.sma(close, maLength)
```

### Rule 10: Performance Standards
- Minimize redundant calculations
- Use efficient algorithms and built-in functions
- Cache expensive computations when possible
- Limit historical data lookbacks to necessary periods

```pinescript
// Cache expensive calculations
var float cachedRsiValue = na
if barstate.isconfirmed
    cachedRsiValue := ta.rsi(close, rsiLength)

// Use efficient lookback periods
MAX_LOOKBACK_BARS = 5000  // Reasonable limit for historical analysis
```

## ADVANCED PATTERN RULES

### Rule 11: Complex Expression Patterns
**Handle advanced Pine Script patterns correctly**

```pinescript
// ✅ CORRECT - Complex ternary chains on single lines
signalColor = rsiValue > 70 ? color.new(color.red, 0) : rsiValue < 30 ? color.new(color.green, 0) : rsiValue > 50 ? color.new(color.yellow, 50) : color.new(color.gray, 80)

// ✅ CORRECT - Complex boolean conditions on single lines
entryCondition = rsiValue < 30 and close > maValue and strategy.position_size == 0 and barstate.isconfirmed and volume > ta.sma(volume, 20)

// ✅ CORRECT - Complex arithmetic on single lines
weightedScore = (rsiScore * 0.3) + (maScore * 0.2) + (volumeScore * 0.2) + (momentumScore * 0.3)
```

### Rule 12: Plotting and Visualization
**Use single-line format for all plotting functions**

```pinescript
// ✅ CORRECT - Single line plotting
plotshape(buySignal, title="Buy Signal", location=location.belowbar, style=shape.triangleup, size=size.small, color=color.green)
plotshape(sellSignal, title="Sell Signal", location=location.abovebar, style=shape.triangledown, size=size.small, color=color.red)
table.cell(perfTable, 0, 1, "Win Rate", bgcolor=color.new(color.gray, 80), text_color=color.white, text_size=size.small)

// ❌ INCORRECT - Multi-line plotting (ERROR PRONE)
plotshape(buySignal, title="Buy Signal", 
         location=location.belowbar, style=shape.triangleup, 
         size=size.small, color=color.green)
```

## AUTOMATED DETECTION PATTERNS

### Search Patterns for Code Review
Use these regex patterns to find problematic code:

1. **Multi-line ternary operators**: `\?\s*$` or `:\s*$`
2. **Multi-line boolean expressions**: `and\s*$` or `or\s*$`
3. **Multi-line arithmetic**: `\+\s*$` or `\*\s*$` or `-\s*$` or `/\s*$`
4. **Multi-line function calls**: `=.*\(\s*$`
5. **Incomplete lines**: `,\s*$`
6. **Emoji detection**: `[\u{1F000}-\u{1F9FF}]`

### Automated Cleanup Process
1. **Search** for problematic patterns using regex
2. **Identify** the complete multi-line statement
3. **Combine** all lines into a single line
4. **Remove** unnecessary whitespace and indentation
5. **Test** for syntax errors
6. **Repeat** until no patterns remain

## ENFORCEMENT AND COMPLIANCE

### Code Review Checklist
- [ ] No emojis present anywhere in code
- [ ] All executable code uses single-line format
- [ ] Pine Script v6 compatibility verified
- [ ] Professional naming conventions followed
- [ ] Proper error handling implemented
- [ ] Documentation complete and clear
- [ ] Performance optimizations applied
- [ ] No multi-line function calls or expressions

### Automated Checks
1. **Emoji Detection**: Scan all files for Unicode emoji characters
2. **Line Continuation Check**: Search for problematic multi-line patterns
3. **Syntax Validation**: Pine Script v6 compilation test
4. **Naming Convention**: Verify camelCase/PascalCase usage
5. **Comment Quality**: Check for proper documentation

### Violation Consequences
- **Critical**: Code with emojis or line continuation errors - IMMEDIATE REJECTION
- **Major**: Pine Script compatibility issues - MUST BE FIXED
- **Minor**: Style guide violations - REQUIRE CORRECTION

## DOCUMENTATION REQUIREMENTS

### Rule 13: Comprehensive Documentation
- Every function must have purpose documentation
- Complex algorithms require detailed explanation comments
- All input parameters must be documented with types and ranges
- Trading logic must be clearly explained with examples
- Performance characteristics must be documented

```pinescript
// Function: calculateAdaptiveRSI
// Purpose: Calculates RSI with adaptive length based on market volatility
// Parameters: 
//   source (series float) - Price source for RSI calculation
//   baseLength (int) - Base RSI length (default: 14)
//   volatilityMultiplier (float) - Volatility adjustment factor (default: 1.5)
// Returns: Adaptive RSI value (0-100)
// Performance: O(n) where n is the lookback period
// Example: adaptiveRsi = calculateAdaptiveRSI(close, 14, 1.5)
calculateAdaptiveRSI(source, baseLength, volatilityMultiplier) =>
    volatility = ta.atr(20) / close
    adaptiveLength = math.round(baseLength * (1 + volatility * volatilityMultiplier))
    ta.rsi(source, math.max(adaptiveLength, 2))
```

## EXCEPTION RULES

### The ONLY Acceptable Multi-Line Patterns
1. **Comment blocks** using `//`
2. **Documentation sections** for functions and algorithms
3. **Visual separators** using `// ═══`
4. **Type definitions** (but constructor calls must be single-line)

```pinescript
// ✅ ACCEPTABLE - Multi-line comments and documentation
// This is a multi-line comment block explaining
// the complex algorithm implementation
// Used for detailed explanations only

// ✅ ACCEPTABLE - Type definitions
type ComplexTradingSignal
    float confidence
    int direction
    string reason
    int timestamp
    float stopLoss
    float takeProfit

// ✅ ACCEPTABLE - Visual separators
// ═══════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════
```

## FINAL MANDATE

**GOLDEN RULE: When in doubt, put it on one line!**

This approach guarantees:
- ✅ Pine Script v6 compatibility
- ✅ Elimination of line continuation errors
- ✅ Professional code standards
- ✅ Institutional-grade reliability
- ✅ Consistent cross-platform behavior
- ✅ Simplified maintenance and debugging

## COMPLIANCE VERIFICATION

All code must pass these checks before commit:

1. **Emoji Scan**: `grep -r "[\u{1F000}-\u{1F9FF}]" .` returns no results
2. **Line Continuation Scan**: All regex patterns return no matches
3. **Pine Script Test**: Code compiles without errors on TradingView
4. **Style Check**: Follows all naming and formatting conventions
5. **Documentation**: All functions and complex logic documented
6. **Performance**: No unnecessary computational overhead

## DOCUMENT UPDATES

This document may only be updated through:
1. Team consensus on new standards
2. Pine Script version compatibility requirements
3. Institutional compliance mandates
4. Critical bug fixes or security requirements

**Last Updated**: December 2024
**Version**: 2.0 (Unified Standard)
**Applies To**: All Pine Script v6 projects and contributors
**Supersedes**: All previous coding standards and style guides 