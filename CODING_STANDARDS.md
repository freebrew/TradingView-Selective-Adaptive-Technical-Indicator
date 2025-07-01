# Coding Standards - Institutional Trading System

## Overview
This document establishes mandatory coding standards for the Four Dropdown Selective Adaptive LC trading system to ensure professional-grade, maintainable, and compatible code.

## MANDATORY RULES

### Rule 1: NO EMOJIS ALLOWED
**STRICTLY FORBIDDEN: Use of emojis anywhere in the codebase**

#### Prohibited Locations:
- Pine Script code files (.pinescript)
- Comments within code
- Documentation files (.md)
- Commit messages
- Variable names
- Function names
- String literals
- Error messages

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
1. **Character Encoding**: Emojis can cause UTF-8/ASCII encoding conflicts
2. **Platform Compatibility**: Inconsistent rendering across different systems
3. **Professional Standards**: Institutional code must be emoji-free
4. **Parsing Issues**: May interfere with Pine Script compilation
5. **Code Clarity**: Text descriptions are more precise and searchable
6. **Version Control**: Emojis can cause git diff/merge issues

### Rule 2: Pine Script v6 Compliance
- Use only Pine Script v6 compatible functions
- Implement custom functions for missing Pine Script features
- Follow single-line format for all executable code
- No multi-line function calls or expressions

### Rule 3: Professional Naming Conventions
```pinescript
// Variables: camelCase
var float currentPrice = 0.0
var int signalCount = 0

// Functions: camelCase with descriptive names
calculateMovingAverage(source, length) =>
    ta.sma(source, length)

// Constants: UPPER_CASE
MAX_LOOKBACK_BARS = 5000
DEFAULT_RSI_LENGTH = 14

// Types: PascalCase
type TradingSignal
    float confidence
    int direction
    string reason
```

### Rule 4: Comment Standards
```pinescript
// Single-line comments: Start with capital, end without period
// This is a proper comment format

// Multi-line documentation blocks
// Function: calculateRiskMetrics
// Purpose: Determines position sizing based on volatility
// Parameters: equity (float), riskPercent (float)
// Returns: Position size in base currency units
```

### Rule 5: Error Handling
- Always validate inputs
- Use defensive programming practices
- Implement proper boundary checks
- Handle edge cases explicitly

```pinescript
// Input validation
rsiLength = input.int(14, "RSI Length", minval=2, maxval=200)
if rsiLength < 2 or rsiLength > 200
    runtime.error("RSI Length must be between 2 and 200")
```

### Rule 6: Code Organization
- Group related functionality together
- Use consistent spacing and indentation
- Separate major sections with comment dividers
- Follow logical code flow (inputs → calculations → outputs)

### Rule 7: Performance Standards
- Minimize redundant calculations
- Use efficient algorithms
- Cache expensive computations
- Limit historical data lookbacks

### Rule 8: Documentation Requirements
- Every function must have purpose documentation
- Complex algorithms require explanation comments
- All input parameters must be documented
- Trading logic must be clearly explained

## ENFORCEMENT

### Code Review Checklist:
- [ ] No emojis present anywhere in code
- [ ] Pine Script v6 compatibility verified
- [ ] Professional naming conventions followed
- [ ] Proper error handling implemented
- [ ] Documentation complete and clear
- [ ] Performance optimizations applied

### Automated Checks:
1. **Emoji Detection**: Scan all files for Unicode emoji characters
2. **Syntax Validation**: Pine Script v6 compilation test
3. **Naming Convention**: Verify camelCase/PascalCase usage
4. **Comment Quality**: Check for proper documentation

### Violation Consequences:
- **Critical**: Code with emojis will be rejected immediately
- **Major**: Pine Script compatibility issues must be fixed
- **Minor**: Style guide violations require correction

## EXAMPLES

### CORRECT Implementation:
```pinescript
// Risk management configuration
type RiskConfig
    float maxDrawdown
    float positionSize
    bool trailingStop

// Calculate optimal position size
calculatePositionSize(equity, riskPercent, stopDistance) =>
    if equity <= 0 or riskPercent <= 0 or stopDistance <= 0
        runtime.error("Invalid parameters for position size calculation")
    
    riskAmount = equity * (riskPercent / 100)
    positionSize = riskAmount / stopDistance
    math.max(positionSize, 0.01)  // Minimum position size
```

### INCORRECT Implementation:
```pinescript
// FORBIDDEN - Contains emojis
// 🚀 Risk management config
type RiskConfig
    float maxDrawdown
    float positionSize  
    bool trailingStop

// 📊 Calculate optimal position size
calculatePositionSize(equity, riskPercent, stopDistance) =>
    // ✅ Validation
    if equity <= 0 or riskPercent <= 0 or stopDistance <= 0
        runtime.error("❌ Invalid parameters")
    
    riskAmount = equity * (riskPercent / 100)
    positionSize = riskAmount / stopDistance
    math.max(positionSize, 0.01)  // 💡 Minimum position
```

## COMPLIANCE VERIFICATION

All code must pass these checks before commit:

1. **Emoji Scan**: `grep -r "[\u{1F000}-\u{1F9FF}]" .` returns no results
2. **Pine Script Test**: Code compiles without errors on TradingView
3. **Style Check**: Follows all naming and formatting conventions
4. **Documentation**: All functions and complex logic documented

## UPDATES TO THIS DOCUMENT

This document may only be updated through:
1. Team consensus on new standards
2. Pine Script version compatibility requirements  
3. Institutional compliance mandates

**Last Updated**: December 2024
**Version**: 1.0
**Applies To**: All project files and contributors 