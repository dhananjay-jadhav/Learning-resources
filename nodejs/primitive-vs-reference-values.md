# JavaScript: Primitive vs. Reference Values

In JavaScript, values are categorized into two types: **primitive** and **reference**. Understanding the difference is crucial for managing data and avoiding common bugs.

-   **Primitive Values**: Atomic, immutable pieces of data.
-   **Reference Values**: Objects, which are collections of properties.

## Memory Management: Stack and Heap

The JavaScript engine uses two memory locations to store data: the **stack** and the **heap**.

-   **Stack**: A region of memory for static data, where the size is known at compile time. This includes primitive values and references (pointers) to objects. Memory is allocated and deallocated in a last-in, first-out (LIFO) manner.

-   **Heap**: A larger, less organized region of memory for dynamic data, where the size can change. This is where objects and functions are stored.

### How Values are Stored

1.  **Primitive Values**: Stored directly on the **stack**.

    ```javascript
    let name = 'John';
    let age = 25;
    ```

    Here, the string `'John'` and the number `25` are stored on the stack along with their variable names.

2.  **Reference Values**: The actual object is stored on the **heap**. A **reference** (or pointer) to that object is stored on the **stack**.

    ```javascript
    let person = {
      name: 'John',
      age: 25,
    };
    ```

    In this case:
    -   The `person` variable on the stack holds a memory address.
    -   This address points to the object `{ name: 'John', age: 25 }` which resides on the heap.

## Key Differences

### Mutability and Properties

-   **Reference Values are Mutable**: You can add, change, or delete properties at any time.

    ```javascript
    let person = {
      name: 'John',
      age: 25,
    };

    // Add a new property
    person.ssn = '123-45-6789';

    // Change an existing property
    person.name = 'John Doe';

    // Delete a property
    delete person.age;

    console.log(person); // { name: 'John Doe', ssn: '123-45-6789' }
    ```

-   **Primitive Values are Immutable**: They cannot be altered. You also cannot add properties to them.

    ```javascript
    let name = 'John';
    name.alias = 'Knight';

    console.log(name); // 'John'
    console.log(name.alias); // undefined
    ```
    While it seems like you're adding a property, JavaScript creates a temporary wrapper object and then discards it, leaving the original primitive unchanged.

### Copying and Assignment

The behavior of copying values differs significantly between primitive and reference types.

-   **Copying Primitive Values (By Value)**: When you assign a primitive value from one variable to another, the value is **copied**. Each variable gets its own separate copy.

    ```javascript
    let age = 25;
    let newAge = age; // The value 25 is copied to newAge

    newAge = newAge + 1;

    console.log(age);    // 25 (unchanged)
    console.log(newAge); // 26
    ```
    `age` and `newAge` are completely independent.

-   **Copying Reference Values (By Reference)**: When you assign a reference value, only the **reference (memory address) is copied**, not the object itself. Both variables point to the exact same object on the heap.

    ```javascript
    let person = {
      name: 'John',
      age: 25,
    };

    let member = person; // The reference is copied, not the object

    // Modify the object using the 'member' reference
    member.age = 26;

    // The change is visible through the 'person' reference as well
    console.log(person.age); // 26
    console.log(member.age); // 26
    ```
    Since both `person` and `member` refer to the same object, a change made through one variable is reflected in the other.


