# JavaScript Arithmetic Operators: Summary

## Overview

This document provides a summary of how to use JavaScript arithmetic operators to perform calculations and describes their behavior with different types of operands.

---

## Supported Arithmetic Operators

| Operator       | Sign |
|----------------|------|
| Addition       | +    |
| Subtraction    | -    |
| Multiplication | *    |
| Division       | /    |

- Operands can be literals or variables.
- Operators typically return a numerical value.

---

## Addition Operator (`+`)

- Returns the sum of two values.
- Can also concatenate strings.

**Numerical Addition Example:**
```javascript
let sum = 10 + 20;
console.log(sum); // 30
```

**Addition with Variables:**
```javascript
let netPrice = 9.99, shippingFee = 1.99;
let grossPrice = netPrice + shippingFee;
console.log(grossPrice); // 11.98
```

**String Concatenation:**
- Two strings: concatenated together.
- Mixed string and number: number is converted to string.

```javascript
let result = 10 + '20';
console.log(result); // '1020'
```

**Special Values Table:**

| First Value | Second Value | Result   | Explanation                 |
|-------------|--------------|----------|-----------------------------|
| NaN         | Any          | NaN      | If either value is NaN      |
| Infinity    | Infinity     | Infinity | Infinity + Infinity         |
| -Infinity   | -Infinity    | -Infinity| -Infinity + -Infinity       |
| Infinity    | -Infinity    | NaN      | Infinity + -Infinity        |
| +0          | +0           | +0       | +0 + +0                     |
| -0          | +0           | +0       | -0 + +0                     |
| -0          | -0           | -0       | -0 + -0                     |

---

## Subtraction Operator (`-`)

- Subtracts one value from another.
- Non-numbers (string, boolean, null, undefined) are converted to numbers using `Number()`.

**Example:**
```javascript
let result = 30 - 10;
console.log(result); // 20
```

**Special Values Table:**

| First Value | Second Value | Result   | Explanation                 |
|-------------|--------------|----------|-----------------------------|
| NaN         | Any          | NaN      | If either value is NaN      |
| Infinity    | Infinity     | NaN      | Infinity - Infinity         |
| -Infinity   | -Infinity    | NaN      | -Infinity - -Infinity       |
| Infinity    | -Infinity    | Infinity | Infinity - (-Infinity)      |
| +0          | +0           | +0       | +0 - +0                     |
| +0          | -0           | +0       | +0 - -0                     |
| -0          | -0           | +0       | -0 - -0                     |

---

## Multiplication Operator (`*`)

- Multiplies operands and returns the result.
- If operand is not a number, it's converted using `Number()`.

**Example:**
```javascript
let result = '5' * 2;
console.log(result); // 10
```

**Special Values Table:**

| First Value | Second Value | Result   | Explanation                        |
|-------------|--------------|----------|------------------------------------|
| NaN         | Any          | NaN      | If either value is NaN             |
| Infinity    | 0            | NaN      | Infinity * 0                       |
| Infinity    | Positive     | Infinity | Infinity * 100                     |
| Infinity    | Negative     | -Infinity| Infinity * -100                    |
| Infinity    | Infinity     | Infinity | Infinity * Infinity                |

---

## Division Operator (`/`)

- Divides first operand by the second.
- Non-numbers are converted using `Number()`.

**Example:**
```javascript
let result = '20' / 2;
console.log(result); // 10
```

**Special Values Table:**

| First Value | Second Value | Result   | Explanation                        |
|-------------|--------------|----------|------------------------------------|
| NaN         | Any          | NaN      | If either value is NaN             |
| Any         | 0            | Infinity | 1 / 0 = Infinity                   |
| Infinity    | Infinity     | NaN      | Infinity / Infinity                |
| 0           | 0            | NaN      | 0 / 0                              |
| Infinity    | Positive     | Infinity | Infinity / 2                       |
| Infinity    | Negative     | -Infinity| Infinity / -2                      |

---

## Using Arithmetic Operators with Objects

- JavaScript tries to use the object's `valueOf()` method for calculations.
- If `valueOf()` isn't available, it uses the `toString()` method.

**`valueOf()` Example:**
```javascript
let energy = { valueOf() { return 100; } };

console.log(energy - 10);    // 90
console.log(energy + 100);   // 200
console.log(energy / 2);     // 50
console.log(energy * 1.5);   // 150
```

**`toString()` Example:**
```javascript
let energy = { toString() { return 50; } };

console.log(energy - 10);    // 40
console.log(energy + 100);   // 150
console.log(energy / 2);     // 25
console.log(energy * 1.5);   // 75
```

---

## Notes

- Arithmetic operators in JavaScript perform implicit type conversion when necessary.
- Special numeric values (`NaN`, `Infinity`) propagate according to JavaScript semantics.

---


 return Math.abs(sum1 - sum2);