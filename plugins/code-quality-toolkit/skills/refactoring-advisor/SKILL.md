---
name: refactoring-advisor
description: Provides refactoring recommendations and step-by-step improvement plans. Use when planning refactoring, improving code structure, or reducing technical debt.
---

# Refactoring Advisor

## Quick Start

Plan refactoring based on identified issues:

1. Identify the problem (code smell, complexity)
2. Select appropriate refactoring pattern
3. Create step-by-step plan
4. Ensure tests exist
5. Refactor incrementally

## Instructions

### Step 1: Assess Current State

Analyze the code:
- What is the problem? (long method, god class, etc.)
- What is the impact? (maintainability, testability)
- What is the risk? (how much code depends on it)

### Step 2: Select Refactoring Strategy

| Problem | Strategy | Risk |
|---------|----------|------|
| Long Method | Extract Method | Low |
| God Class | Extract Class | Medium |
| Duplicated Code | Extract Method | Low |
| Switch Statement | Replace with Polymorphism | High |
| Long Parameter List | Introduce Parameter Object | Medium |

### Step 3: Create Refactoring Plan

**Template:**
```markdown
## Refactoring: [Name]

**Problem:** [Description]
**Goal:** [Desired outcome]
**Risk Level:** [Low/Medium/High]

### Prerequisites
- [ ] Tests exist and pass
- [ ] Code is committed
- [ ] Dependencies identified

### Steps
1. [First small change]
2. [Run tests]
3. [Next small change]
4. [Run tests]
...

### Validation
- [ ] All tests pass
- [ ] No functionality changed
- [ ] Code is cleaner
```

### Step 4: Execute Incrementally

**Golden Rule:** Make the change easy, then make the easy change

1. Make one small change
2. Run tests
3. Commit
4. Repeat

## Refactoring Examples

### Extract Method

**Before:**
```javascript
function printOwing(invoice) {
  let outstanding = 0;
  
  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");
  
  for (const o of invoice.orders) {
    outstanding += o.amount;
  }
  
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

**Plan:**
1. Extract banner printing
2. Run tests
3. Extract outstanding calculation
4. Run tests
5. Extract details printing
6. Run tests

**After:**
```javascript
function printOwing(invoice) {
  printBanner();
  const outstanding = calculateOutstanding(invoice);
  printDetails(invoice, outstanding);
}

function printBanner() {
  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");
}

function calculateOutstanding(invoice) {
  return invoice.orders.reduce((sum, o) => sum + o.amount, 0);
}

function printDetails(invoice, outstanding) {
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

### Extract Class

**Before:**
```javascript
class Person {
  name;
  officeAreaCode;
  officeNumber;
  
  getTelephoneNumber() {
    return `(${this.officeAreaCode}) ${this.officeNumber}`;
  }
}
```

**Plan:**
1. Create TelephoneNumber class
2. Move areaCode field
3. Run tests
4. Move number field
5. Run tests
6. Move getTelephoneNumber method
7. Run tests
8. Update Person to use TelephoneNumber
9. Run tests

**After:**
```javascript
class TelephoneNumber {
  areaCode;
  number;
  
  toString() {
    return `(${this.areaCode}) ${this.number}`;
  }
}

class Person {
  name;
  telephoneNumber;
  
  getTelephoneNumber() {
    return this.telephoneNumber.toString();
  }
}
```

## Safety Checklist

Before refactoring:
- [ ] Tests exist and pass
- [ ] Code is in version control
- [ ] You understand what the code does
- [ ] You have time to complete the refactoring

During refactoring:
- [ ] Make small changes
- [ ] Run tests after each change
- [ ] Commit working states
- [ ] Don't add features while refactoring

After refactoring:
- [ ] All tests pass
- [ ] Code review completed
- [ ] Documentation updated
- [ ] No functionality changed
