# JavaScript Notes – Part 1: Introduction & Basics

JavaScript (JS) is a lightweight, interpreted, and dynamic programming language primarily used for web development. It enables interactive web pages and is an essential part of web applications.

## 1. What is JavaScript?
- **Client-Side Scripting Language**: Runs in the browser to add interactivity.
- **Dynamically Typed**: Variables do not require explicit type declarations.
- **Prototype-Based Object Orientation**: Objects inherit properties from prototypes instead of classes.
- **Single-Threaded & Asynchronous**: Uses an event loop to handle asynchronous operations.

### Java vs. JavaScript: Key Differences
| Feature         | Java                        | JavaScript                   |
|---------------|---------------------------|-----------------------------|
| Compiled or Interpreted | Compiled (JVM) | Interpreted (Browser/Node.js) |
| Typing System | Statically Typed | Dynamically Typed |
| Concurrency | Multi-threaded | Single-threaded (event loop) |
| OOP Model | Class-based | Prototype-based |
| Usage | Backend (Spring Boot, etc.) | Frontend & Backend (Node.js) |

---

## 2. Writing Your First JavaScript Code

### Running JavaScript:
- **Inside an HTML file**:
  ```html
  <script>
      console.log("Hello, JavaScript!");
  </script>
  ```
- **External JavaScript File (`script.js`)**:
  ```js
  console.log("Hello, JavaScript!");
  ```
  Include it in HTML:
  ```html
  <script src="script.js"></script>
  ```

- **Running in Browser Console**: Open Developer Tools (`F12` or `Ctrl+Shift+I` in Chrome) → Console.

---

## 3. Variables & Data Types

### 3.1 Declaring Variables
JavaScript uses `var`, `let`, and `const`:
```js
var x = 10; // Function-scoped (avoid using)
let y = 20; // Block-scoped (preferred)
const z = 30; // Constant variable (cannot be reassigned)
```

#### Java Equivalent:
```java
int x = 10;
final int z = 30; // Similar to 'const' in JS
```

### 3.2 Data Types in JavaScript
JavaScript has **primitive** and **reference** types.

#### 3.2.1 Primitive Types
| Type | Example |
|------|---------|
| Number | `let num = 10;` |
| String | `let str = "Hello";` |
| Boolean | `let flag = true;` |
| Undefined | `let x; // Undefined` |
| Null | `let y = null;` |
| Symbol | `let sym = Symbol("id");` |
| BigInt | `let big = 123n;` |

#### 3.2.2 Reference Types (Objects)
| Type | Example |
|------|---------|
| Object | `let obj = { name: "John", age: 30 };` |
| Array | `let arr = [1, 2, 3];` |
| Function | `let greet = function() { return "Hello"; };` |

#### Java vs. JavaScript Data Types:
| Java | JavaScript |
|------|-----------|
| int, double, float | Number (all are treated as `Number`) |
| char, String | String |
| boolean | Boolean |
| null | null |
| Arrays, Lists | Array, Object |

---

## 4. Operators in JavaScript

JavaScript supports various operators, similar to Java.

### 4.1 Arithmetic Operators
```js
let a = 10, b = 3;
console.log(a + b); // Addition
console.log(a - b); // Subtraction
console.log(a * b); // Multiplication
console.log(a / b); // Division
console.log(a % b); // Modulus
console.log(a ** b); // Exponentiation (10^3)
```

### 4.2 Comparison Operators
```js
console.log(10 == "10");  // true  (loose equality, type coercion)
console.log(10 === "10"); // false (strict equality, no coercion)
console.log(10 != 20);    // true
console.log(10 > 5);      // true
console.log(10 >= 10);    // true
```

### Java Equivalent:
```java
System.out.println(10 == 10);  // true
System.out.println(10 != 20);  // true
System.out.println(10 > 5);    // true
```

### 4.3 Logical Operators
```js
console.log(true && false); // false (AND)
console.log(true || false); // true  (OR)
console.log(!true);         // false (NOT)
```

### Java Equivalent:
```java
System.out.println(true && false); // false
System.out.println(true || false); // true
System.out.println(!true);         // false
```

---

## 5. Control Flow (if-else & switch)

### 5.1 If-Else Statement
```js
let age = 18;
if (age >= 18) {
    console.log("You are an adult.");
} else {
    console.log("You are a minor.");
}
```

### Java Equivalent:
```java
int age = 18;
if (age >= 18) {
    System.out.println("You are an adult.");
} else {
    System.out.println("You are a minor.");
}
```

### 5.2 Switch Statement
```js
let day = 2;
switch (day) {
    case 1:
        console.log("Monday");
        break;
    case 2:
        console.log("Tuesday");
        break;
    default:
        console.log("Invalid day");
}
```

### Java Equivalent:
```java
int day = 2;
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    default:
        System.out.println("Invalid day");
}
```

---

## 6. Loops in JavaScript

### 6.1 For Loop
```js
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

### 6.2 While Loop
```js
let i = 0;
while (i < 5) {
    console.log(i);
    i++;
}
```

### 6.3 Do-While Loop
```js
let i = 0;
do {
    console.log(i);
    i++;
} while (i < 5);
```

### Java Equivalent:
```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}

int i = 0;
do {
    System.out.println(i);
    i++;
} while (i < 5);
```

---

## Summary of Part 1:
- JavaScript is an interpreted, dynamically typed, prototype-based language.
- `let` and `const` are preferred over `var` for variable declarations.
- JS has primitive and reference data types.
- Operators work similarly to Java but `==` allows type coercion.
- Control structures (`if-else`, `switch`, loops) are almost identical to Java.

---

This concludes **Part 1** of JavaScript notes. In **Part 2**, we will cover **Functions, Scope, Closures, and ES6 Features**. 🚀

Would you like any modifications or explanations in more depth? 😊