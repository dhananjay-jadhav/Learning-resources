# JavaScript Arrays: Summary Document

## Overview
This document provides a concise summary of the tutorial on JavaScript arrays and their basic operations. It covers fundamental concepts, creation methods, accessing elements, and key array operations.

---

## Introduction
- **Arrays** in JavaScript are ordered lists of values, each referred to as an element and accessed via an index.
- Arrays can store mixed data types (numbers, strings, booleans, null).
- Array size is **dynamic**—no need to declare the size upfront.

---

## Creating JavaScript Arrays

### 1. Using the `Array` Constructor
```javascript
let scores = new Array();           // empty array
let scores = Array(10);             // array with initial size 10
let scores = new Array(9, 10, 8);   // array with numbers
let signs = new Array('Red');       // array with single element 'Red'
let artists = Array();              // also creates an empty array
```
> **Note:** The use of `Array` constructor is less common in practice.

### 2. Using Array Literal Notation (**Preferred**)
```javascript
let arrayName = [element1, element2, ...];
let colors = ['red', 'green', 'blue']; // array of strings
let emptyArray = [];                   // empty array
```

---

## Accessing Array Elements

- Arrays are **zero-indexed**.
- Access using `arrayName[index]`:
```javascript
let mountains = ['Everest', 'Fuji', 'Nanga Parbat'];
console.log(mountains[0]); // 'Everest'
console.log(mountains[1]); // 'Fuji'
console.log(mountains[2]); // 'Nanga Parbat'
```

- Modify elements:
```javascript
mountains[2] = 'K2';
console.log(mountains);
// Output: ['Everest', 'Fuji', 'K2']
```

---

## Getting Array Size

- Use `.length` property:
```javascript
console.log(mountains.length); // 3
```

---

## Basic Array Operations

1. **Append to End**: `push()`
    ```javascript
    seas.push('Red Sea');
    // ['Black Sea', 'Caribbean Sea', 'North Sea', 'Baltic Sea', 'Red Sea']
    ```
2. **Add to Beginning**: `unshift()`
    ```javascript
    seas.unshift('Red Sea');
    // ['Red Sea', 'Black Sea', 'Caribbean Sea', 'North Sea', 'Baltic Sea']
    ```
3. **Remove from End**: `pop()`
    ```javascript
    const lastElement = seas.pop();
    // lastElement: 'Baltic Sea'
    ```
4. **Remove from Beginning**: `shift()`
    ```javascript
    const firstElement = seas.shift();
    // firstElement: 'Black Sea'
    ```
5. **Find Index of Element**: `indexOf()`
    ```javascript
    let index = seas.indexOf('North Sea');
    // index: 2
    ```
6. **Check if Value is Array**: `Array.isArray()`
    ```javascript
    console.log(Array.isArray(seas)); // true
    ```
