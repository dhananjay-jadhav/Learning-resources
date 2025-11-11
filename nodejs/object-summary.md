# JavaScript Objects Summary

A JavaScript **object** is an unordered collection of key-value pairs. Each pair is a **property**.

-   **Keys** are strings.
-   **Values** can be any data type (string, number, function, another object, etc.).

## Creating Objects

The most common way is using **object literal** notation (`{}`).

```javascript
// An empty object
let empty = {};

// An object with properties
let person = {
    firstName: 'John',
    lastName: 'Doe'
};
```

## Accessing Properties

There are two ways to access an object's properties:

1.  **Dot Notation (`.`):**
    `objectName.propertyName`

    ```javascript
    console.log(person.firstName); // "John"
    ```

2.  **Bracket Notation (`[]`):**
    `objectName['propertyName']`

    ```javascript
    console.log(person['firstName']); // "John"
    ```

    Bracket notation is required when property names:
    -   Contain spaces or special characters.
    -   Are stored in a variable.

    ```javascript
    let address = {
        'building no': 3960,
        street: 'North 1st street'
    };
    console.log(address['building no']); // 3960
    ```

Accessing a property that doesn't exist returns `undefined`.

## Manipulating Properties

### Modifying a Property

Use the assignment operator (`=`).

```javascript
person.firstName = 'Jane';
console.log(person.firstName); // "Jane"
```

### Adding a New Property

Assign a value to a new property name.

```javascript
person.age = 25;
console.log(person.age); // 25
```

### Deleting a Property

Use the `delete` operator.

```javascript
delete person.age;
console.log(person.age); // undefined
```

## Checking for Properties

Use the `in` operator to check if a property exists in an object. It returns `true` or `false`.

```javascript
let employee = {
    firstName: 'Peter',
    employeeId: 1
};

console.log('ssn' in employee);        // false
console.log('employeeId' in employee); // true
```
