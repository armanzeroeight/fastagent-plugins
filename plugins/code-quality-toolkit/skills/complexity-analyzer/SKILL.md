---
name: complexity-analyzer
description: Analyzes cyclomatic and cognitive complexity, identifies overly complex functions. Use when assessing code complexity or identifying functions that need simplification.
---

# Complexity Analyzer

## Quick Start

Analyze complexity with tools:

```bash
# JavaScript
npx eslint --plugin complexity --rule 'complexity: [error, 10]' src/

# Python
pip install radon
radon cc src/ -a -nb

# General
npx complexity-report src/
```

## Instructions

### Step 1: Calculate Cyclomatic Complexity

Count decision points + 1:
- if, else, elif
- for, while loops
- case statements
- && and || operators
- ternary operators
- catch blocks

**Example:**
```javascript
function example(x) {  // +1 (base)
  if (x > 0) {         // +1
    return x;
  } else if (x < 0) {  // +1
    return -x;
  }
  return 0;
}
// Complexity: 3
```

### Step 2: Identify Complex Functions

| Complexity | Rating | Action |
|------------|--------|--------|
| 1-10 | Simple | No action needed |
| 11-20 | Moderate | Consider refactoring |
| 21-50 | Complex | Should refactor |
| 50+ | Very Complex | Must refactor |

### Step 3: Analyze Cognitive Complexity

Cognitive complexity considers:
- Nesting depth (each level adds to complexity)
- Breaks in linear flow (if, loops, catch)
- Recursion

```javascript
// Cyclomatic: 4, Cognitive: 7
function sumOfPrimes(max) {        // +0
  let total = 0;
  for (let i = 2; i <= max; i++) { // +1 (loop)
    let isPrime = true;
    for (let j = 2; j < i; j++) {  // +2 (nested loop)
      if (i % j === 0) {           // +3 (nested if)
        isPrime = false;
        break;                     // +1 (break)
      }
    }
    if (isPrime) {                 // +1 (if)
      total += i;
    }
  }
  return total;
}
```

### Step 4: Generate Report

```markdown
## Complexity Report

### High Complexity (>20)

**processOrder()** - src/orders.js:45
- Cyclomatic: 28
- Cognitive: 35
- Lines: 127
- Recommendation: Extract validation, calculation, and persistence logic

**validateUser()** - src/auth.js:12
- Cyclomatic: 22
- Cognitive: 28
- Lines: 89
- Recommendation: Extract individual validation rules

### Moderate Complexity (11-20)

[List functions with complexity 11-20]
```

## Simplification Strategies

### Extract Method
```javascript
// Before: Complexity 15
function processData(data) {
  if (data.type === 'A') {
    // 20 lines of processing
  } else if (data.type === 'B') {
    // 20 lines of processing
  }
}

// After: Complexity 3
function processData(data) {
  if (data.type === 'A') return processTypeA(data);
  if (data.type === 'B') return processTypeB(data);
}
```

### Replace Nested Conditionals
```javascript
// Before: Cognitive 12
function getPrice(user, product) {
  if (user) {
    if (user.isPremium) {
      if (product.onSale) {
        return product.price * 0.7;
      } else {
        return product.price * 0.9;
      }
    }
  }
  return product.price;
}

// After: Cognitive 4
function getPrice(user, product) {
  if (!user || !user.isPremium) return product.price;
  if (product.onSale) return product.price * 0.7;
  return product.price * 0.9;
}
```

### Use Polymorphism
```javascript
// Before: Complexity 8
function getArea(shape) {
  if (shape.type === 'circle') {
    return Math.PI * shape.radius ** 2;
  } else if (shape.type === 'square') {
    return shape.side ** 2;
  }
  // ... more types
}

// After: Complexity 1 per class
class Circle {
  getArea() { return Math.PI * this.radius ** 2; }
}
class Square {
  getArea() { return this.side ** 2; }
}
```
