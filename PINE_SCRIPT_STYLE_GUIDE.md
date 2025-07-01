# Pine Script v6 Style Guide - Preventing Line Continuation Errors

## The Problem
Pine Script v6 has strict rules about line continuation that frequently cause "end of line without line continuation" errors. This guide provides a permanent solution to prevent these issues.

## Root Cause Analysis
The error "Mismatched input 'end of line without line continuation' expecting ')'" occurs when:

1. **Multi-line function calls** don't follow proper continuation syntax
2. **Invisible characters** or encoding issues in multi-line statements
3. **Improper indentation** in continued lines
4. **Missing continuation markers** (commas, operators, brackets)
5. **Multi-line ternary operators** split across lines
6. **Multi-line boolean expressions** with logical operators
7. **Multi-line arithmetic expressions** with mathematical operators

## Permanent Solution: Single-Line Function Calls

### ✅ CORRECT - Single Line Format
```pinescript
// Input statements
kernelType = input.string("Rational Quadratic", "ML Kernel", options=["Rational Quadratic", "Gaussian", "Periodic", "Locally Periodic"], group="Core Settings")

// Table declarations
var table mtfTable = table.new(position.bottom_right, 6, 6, bgcolor=color.new(color.black, 80), border_width=1, border_color=color.gray)

// Plot functions
plotshape(buySignal, title="Buy Signal", location=location.belowbar, style=shape.triangleup, size=size.small, color=color.green)

// Strategy operations
strategy.exit("Long Exit", "Long", stop=close * (1 - stopLossPercent / 100), limit=close * (1 + takeProfitPercent / 100))

// Ternary operators
smoothedSignal = smoothedPrediction > 0.2 ? 1 : smoothedPrediction < -0.2 ? -1 : 0

// Boolean expressions
shouldEnterLong = buySignal and strategy.position_size == 0 and (optimizationMode != "Conservative" or lcConfidence > 0.6)

// Arithmetic expressions
weightedSignal = (lcSignal * lcWeight) + (signalCurrent * mtfWeight) + (smoothedSignal * kernelWeight) + (adaptiveSignal * adaptiveWeight)

// Object constructors
newZone = SupplyDemandZone.new(topPrice, bottomPrice, time[pivotIndex], time, "Supply", strength, 0, true, zoneVolume, momentum, bar_index - pivotIndex, strength)
```

### ❌ INCORRECT - Multi-Line Format (CAUSES ERRORS)
```pinescript
// Multi-line input (ERROR PRONE)
kernelType = input.string(
    defval   = "Rational Quadratic",
    title    = "ML Kernel",
    options  = ["Rational Quadratic", "Gaussian", "Periodic", "Locally Periodic"],
    group    = "Core Settings"
)

// Multi-line table (ERROR PRONE)
var table mtfTable = table.new(position.bottom_right, 6, 6, 
    bgcolor=color.new(color.black, 80), 
    border_width=1, 
    border_color=color.gray)

// Multi-line ternary (ERROR PRONE)
smoothedSignal = smoothedPrediction > 0.2 ? 1 : 
                smoothedPrediction < -0.2 ? -1 : 0

// Multi-line boolean (ERROR PRONE)
shouldEnterLong = buySignal and strategy.position_size == 0 and 
                 (optimizationMode != "Conservative" or lcConfidence > 0.6)

// Multi-line arithmetic (ERROR PRONE)
weightedSignal = (lcSignal * lcWeight) + 
                (signalCurrent * mtfWeight) + 
                (smoothedSignal * kernelWeight) + 
                (adaptiveSignal * adaptiveWeight)
```

## Comprehensive Pattern Rules

### Rule 1: Function Calls
**Always use single-line format for function calls with multiple parameters**

```pinescript
// ✅ CORRECT
strategy.exit("Long Exit", "Long", stop=stopPrice, limit=targetPrice)

// ❌ INCORRECT
strategy.exit("Long Exit", "Long", 
              stop=stopPrice, 
              limit=targetPrice)
```

### Rule 2: Ternary Operators
**Keep all ternary operators on single lines, no matter how long**

```pinescript
// ✅ CORRECT
signal = condition1 ? value1 : condition2 ? value2 : condition3 ? value3 : defaultValue

// ❌ INCORRECT
signal = condition1 ? value1 : 
         condition2 ? value2 : 
         condition3 ? value3 : defaultValue
```

### Rule 3: Boolean Expressions
**Combine all boolean logic on single lines**

```pinescript
// ✅ CORRECT
complexCondition = buySignal and strategy.position_size == 0 and currentDrawdown < maxDrawdown and consecutiveLosses < maxLosses

// ❌ INCORRECT
complexCondition = buySignal and strategy.position_size == 0 and 
                  currentDrawdown < maxDrawdown and
                  consecutiveLosses < maxLosses
```

### Rule 4: Arithmetic Expressions
**Keep mathematical operations on single lines**

```pinescript
// ✅ CORRECT
result = (value1 * weight1) + (value2 * weight2) + (value3 * weight3) + (value4 * weight4)

// ❌ INCORRECT
result = (value1 * weight1) + 
         (value2 * weight2) + 
         (value3 * weight3) + 
         (value4 * weight4)
```

### Rule 5: Object Constructors
**Use single-line format for object creation**

```pinescript
// ✅ CORRECT
config = RiskConfig.new(maxDrawdown = 15.0, maxPosition = 10.0, maxDailyLoss = 3.0, maxLosses = 5, volatilityMult = 1.5, correlation = 0.8, trailingStop = true, breakevenStop = true, timeExit = true)

// ❌ INCORRECT
config = RiskConfig.new(
    maxDrawdown = 15.0,
    maxPosition = 10.0,
    maxDailyLoss = 3.0,
    maxLosses = 5,
    volatilityMult = 1.5,
    correlation = 0.8,
    trailingStop = true,
    breakevenStop = true,
    timeExit = true
)
```

## Advanced Patterns to Avoid

### Pattern 1: Multi-line plotshape calls
```pinescript
// ❌ INCORRECT
plotshape(condition, title="Signal", style=shape.triangleup, 
         location=location.belowbar, color=color.green, size=size.small)

// ✅ CORRECT
plotshape(condition, title="Signal", style=shape.triangleup, location=location.belowbar, color=color.green, size=size.small)
```

### Pattern 2: Multi-line table.cell calls
```pinescript
// ❌ INCORRECT
table.cell(perfTable, 0, 1, "Metric", bgcolor=color.new(color.gray, 80), 
          text_color=color.white, text_size=size.small)

// ✅ CORRECT
table.cell(perfTable, 0, 1, "Metric", bgcolor=color.new(color.gray, 80), text_color=color.white, text_size=size.small)
```

### Pattern 3: Multi-line complex ternary chains
```pinescript
// ❌ INCORRECT
color = value > 0.8 ? color.new(color.green, 60) : 
        value > 0.6 ? color.new(color.yellow, 80) : 
        value > 0.3 ? color.new(color.orange, 80) : color.new(color.red, 80)

// ✅ CORRECT
color = value > 0.8 ? color.new(color.green, 60) : value > 0.6 ? color.new(color.yellow, 80) : value > 0.3 ? color.new(color.orange, 80) : color.new(color.red, 80)
```

## Search Patterns for Cleanup

Use these regex patterns to find problematic code:

1. **Multi-line ternary operators**: `\?\s*$` or `:\s*$`
2. **Multi-line boolean expressions**: `and\s*$` or `or\s*$`
3. **Multi-line arithmetic**: `\+\s*$` or `\*\s*$`
4. **Multi-line function calls**: `=.*\(\s*$`
5. **Incomplete lines**: `,\s*$`

## Automated Cleanup Process

1. **Search** for problematic patterns using regex
2. **Identify** the complete multi-line statement
3. **Combine** all lines into a single line
4. **Remove** unnecessary whitespace and indentation
5. **Test** for syntax errors
6. **Repeat** until no more patterns found

## Benefits of Single-Line Format

1. **Eliminates** line continuation syntax errors
2. **Improves** code reliability and portability
3. **Reduces** invisible character issues
4. **Simplifies** code maintenance
5. **Ensures** Pine Script v6 compatibility
6. **Prevents** parsing ambiguities

## Rule 6: No Emojis in Code
**FORBIDDEN: Use of emojis in Pine Script code, comments, or documentation**

```pinescript
// ❌ INCORRECT - Emojis not allowed
// 🚀 Buy signal detected
// 📊 Performance metrics
// ✅ Entry condition met

// ✅ CORRECT - Use descriptive text only  
// Buy signal detected
// Performance metrics
// Entry condition met
```

### Reasons for Emoji Prohibition:
1. **Encoding Issues**: Emojis can cause character encoding problems
2. **Platform Compatibility**: Not all systems display emojis consistently
3. **Professional Standards**: Institutional code should be emoji-free
4. **Parsing Problems**: May interfere with Pine Script compilation
5. **Code Clarity**: Text descriptions are more precise than emojis

## Exception: Comments and Documentation

The only acceptable multi-line patterns are:
- Comment blocks (`//`)
- Documentation sections  
- Visual separators (`// ═══`)

## Final Recommendation

**ALWAYS use single-line format for any executable Pine Script code. When in doubt, put it on one line!**

This approach guarantees Pine Script v6 compatibility and eliminates the most common source of syntax errors in complex trading strategies. 