# JavaScript Fundamentals

This document provides a quick brush-up on fundamental JavaScript concepts.

## "Hello, World!" Example

To include JavaScript in an HTML page, you use the `<script>` tag. It's best practice to place it at the end of the `<body>` tag to ensure the HTML content is fully loaded before the script executes.

Here's a basic HTML setup:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>JavaScript Hello, World!</title>
  </head>
  <body>
    <!-- The script is included here -->
    <script src="js/app.js"></script>
  </body>
</html>
```

To display a simple alert in the browser, you can use the `alert()` function inside your `.js` file:

```javascript
// js/app.js
alert('Hello, World!');
```

## JavaScript Syntax

### Statements

A statement is an instruction for the JavaScript engine. Simple statements end with a semicolon (`;`).

```javascript
let message = 'Hello';
console.log(message);
```

### Blocks

A block is a sequence of statements grouped together within curly braces `{}`.

```javascript
if (true) {
  let message = 'Inside a block';
  console.log(message);
}
```

### Identifiers

An identifier is a name for a variable, function, class, etc.

-   Must start with a letter (a-z, A-Z), an underscore (`_`), or a dollar sign (`$`).
-   Can be followed by letters, numbers (0-9), underscores, or dollar signs.
-   Are case-sensitive (`message` is different from `Message`).

### Expressions

An expression is a piece of code that evaluates to a value.

```javascript
2 + 1; // Evaluates to 3
```



KeyWords & Reserve Words 

break	case	catch
continue	debugger	default
else	export	extends
function	if	import
new	return	super
throw	try	null
void	while	with
class	delete	finally
in	switch	typeof
yield	const	do
for	instanceof	this
var

enum	implements	let
protected	private	public
await	interface	package
implements	public	


Use whitespace, including carriage return, space, newline, and tab to format the code. The JavaScript engine disregards whitespace.
Use a semicolon (;) to terminate a simple statement.
Use the curly braces ({}) to create a block that groups one or more simple statements.
A single-line comment starts with // followed by text, while a block comment begins with /* and ends with */. The JavaScript engine ignores comments.
Identifiers are names you choose for variables, functions, classes, and so on.
Avoid using reserved keywords for identifiers.



## JavaScript Data Types

JavaScript has several primitive data types and one complex data type.

To determine the type of a value, you can use the `typeof` operator.

```javascript
let name = 'John';
console.log(typeof name); // "string"
```

### Primitive Data Types

There are 7 primitive data types:

-   `null`
-   `undefined`
-   `boolean`
-   `number`
-   `string`
-   `symbol` (introduced in ES2015)
-   `bigint` (introduced in ES2020)

#### `null` vs. `undefined`

-   **`undefined`**: Represents a variable that has been declared but not yet assigned a value.
-   **`null`**: Represents the intentional absence of any object value. It's an assignment value.

JavaScript considers `null` and `undefined` to be equal in value but not in type.

```javascript
console.log(null == undefined); // true (loose equality)
console.log(null === undefined); // false (strict equality)
```

#### `number`

The `number` type represents both integer and floating-point numbers.

```javascript
let integer = 100;
let float = 3.14;
```

**Special Numeric Values:**

-   `Infinity`: Represents positive infinity (e.g., `1 / 0`).
-   `-Infinity`: Represents negative infinity.
-   `NaN`: Represents "Not-a-Number", the result of an invalid or unrepresentable mathematical operation (e.g., `'a' / 2`).

**Numeric Limits:**

JavaScript numbers have limits.

```javascript
console.log(Number.MAX_VALUE); // 1.7976931348623157e+308
console.log(Number.MIN_VALUE); // 5e-324
```

Exceeding `MAX_VALUE` results in `Infinity`.

```javascript
console.log(Number.MAX_VALUE + Number.MAX_VALUE); // Infinity
```

**Numeric Separators (`_`):**

For better readability, you can use an underscore as a numeric separator.

```javascript
const budget = 1_000_000_000;
console.log(budget); // 1000000000
```

#### `bigint`

The `bigint` type can represent whole numbers larger than 2^53 - 1, which is the limit for the `number` type. A `bigint` is created by appending `n` to the end of an integer.

```javascript
let bigNumber = 9007199254740991n;
console.log(typeof bigNumber); // 'bigint'
```

#### `string`

A `string` is a sequence of characters used to represent text. Strings are immutable, meaning they cannot be changed after they are created. Any operation that seems to modify a string actually creates a new one.

**Creating Strings:**

```javascript
let str = 'JavaScript';
str = str + ' String'; // Creates a new string 'JavaScript String'
console.log(str);
```

**Template Literals (String Interpolation):**

Template literals (using backticks `` ` ``) allow embedding expressions inside a string.

```javascript
let name = 'John';
let message = `Hi, I'm ${name}.`;
console.log(message); // "Hi, I'm John."
```

**String Comparison:**

Strings are compared based on their character's numeric values (Unicode). This can lead to seemingly unexpected results.

```javascript
console.log('a' < 'b'); // true
console.log('a' < 'B'); // false (uppercase 'B' has a smaller numeric value)
```

#### `boolean`

The `boolean` type has two literal values: `true` and `false`. They are case-sensitive.

**Type Conversion to Boolean (Falsy and Truthy Values):**

When converting other data types to a boolean, certain values become `false`. These are called "falsy" values.

-   `""` (empty string)
-   `0`, `NaN`
-   `null`
-   `undefined`

All other values are considered "truthy" and will be converted to `true`.

```javascript
console.log(Boolean('Hi')); // true (non-empty string)
console.log(Boolean(''));   // false (empty string)

console.log(Boolean(20));   // true (non-zero number)
console.log(Boolean(0));    // false

console.log(Boolean({}));   // true (any object)
console.log(Boolean(null)); // false
```

### Complex Data Type

#### `object`

The `object` type is a collection of key-value pairs. It's a complex data type used to store collections of data and more complex entities.

```javascript
let person = {
    firstName: 'John',
    lastName: 'Doe'
};
```

---

## JavaScript Keywords & Reserved Words

Keywords are reserved words that have special meaning in JavaScript. You cannot use them as identifiers (e.g., variable names, function names).

**Keywords:**
`break`, `case`, `catch`, `class`, `const`, `continue`, `debugger`, `default`, `delete`, `do`, `else`, `export`, `extends`, `finally`, `for`, `function`, `if`, `import`, `in`, `instanceof`, `new`, `return`, `super`, `switch`, `this`, `throw`, `try`, `typeof`, `var`, `void`, `while`, `with`, `yield`

**Reserved for future use:**
`enum`, `implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `await`

---

## Code Formatting and Comments

-   **Whitespace**: JavaScript ignores whitespace (spaces, tabs, newlines). Use it to format your code for readability.
-   **Semicolons (`;`)**: Use a semicolon to terminate a simple statement.
-   **Blocks (`{}`)**: Use curly braces to group multiple statements into a block.
-   **Comments**:
    -   Single-line comment: `// This is a comment`
    -   Multi-line comment: `/* This is a multi-line comment */`

---



