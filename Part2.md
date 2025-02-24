# JavaScript Notes – Part 2: Functions, Scope, Closures & ES6 Features

In this part, we will explore **functions, scope, closures, and ES6 features** in JavaScript, drawing comparisons to Java when necessary.

---

## 1. Functions in JavaScript

Functions are reusable blocks of code used to perform a specific task.

### 1.1 Function Declaration (Named Function)
```js
function greet(name) {
    return "Hello, " + name + "!";
}

console.log(greet("Alice")); // Hello, Alice!
```
**Java Equivalent:**
```java
public class Main {
    static String greet(String name) {
        return "Hello, " + name + "!";
    }

    public static void main(String[] args) {
        System.out.println(greet("Alice"));
    }
}
```

### 1.2 Function Expression (Anonymous Function)
A function can be assigned to a variable.
```js
const greet = function(name) {
    return "Hello, " + name + "!";
};

console.log(greet("Bob"));
```

### 1.3 Arrow Functions (ES6)
Arrow functions provide a shorter syntax for writing functions.
```js
const greet = (name) => "Hello, " + name + "!";
console.log(greet("Charlie"));
```
If there is only one parameter, parentheses can be omitted:
```js
const square = num => num * num;
console.log(square(5)); // 25
```

---

## 2. Function Parameters & Default Values
JavaScript allows default parameters.
```js
function greet(name = "Guest") {
    return "Hello, " + name + "!";
}

console.log(greet()); // Hello, Guest!
console.log(greet("David")); // Hello, David!
```
**Java Equivalent (Method Overloading since Java doesn’t support default parameters):**
```java
public class Main {
    static String greet() {
        return "Hello, Guest!";
    }

    static String greet(String name) {
        return "Hello, " + name + "!";
    }

    public static void main(String[] args) {
        System.out.println(greet()); // Hello, Guest!
        System.out.println(greet("David")); // Hello, David!
    }
}
```

---

## 3. JavaScript Scope

Scope determines where a variable can be accessed.

### 3.1 Global Scope
A variable declared outside functions is globally accessible.
```js
let globalVar = "I am global";

function show() {
    console.log(globalVar); // Accessible
}

show();
console.log(globalVar); // Accessible
```

### 3.2 Function Scope
Variables declared inside a function are not accessible outside.
```js
function demo() {
    let localVar = "I am local";
    console.log(localVar); // Accessible
}

demo();
console.log(localVar); // Error: localVar is not defined
```

### 3.3 Block Scope (let & const)
`let` and `const` are block-scoped, but `var` is not.
```js
if (true) {
    let a = 10;
    var b = 20;
}

console.log(b); // 20 (var is function-scoped)
console.log(a); // Error: a is not defined
```

**Java Equivalent:**
```java
public class Main {
    public static void main(String[] args) {
        if (true) {
            int x = 10; // Block scoped
        }
        // System.out.println(x); // Error: x is not defined
    }
}
```

---

## 4. Closures in JavaScript

A closure is a function that remembers the variables from its outer scope even after the outer function has finished executing.

### 4.1 Example of Closure
```js
function outer() {
    let counter = 0;

    return function inner() {
        counter++;
        console.log(counter);
    };
}

const increment = outer();
increment(); // 1
increment(); // 2
increment(); // 3
```

**Java Equivalent (Using an Inner Class):**
```java
class Outer {
    private int counter = 0;

    public Runnable inner() {
        return () -> {
            counter++;
            System.out.println(counter);
        };
    }
}

public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Runnable increment = outer.inner();

        increment.run(); // 1
        increment.run(); // 2
        increment.run(); // 3
    }
}
```

Closures are heavily used in **event handling, data hiding, and functional programming**.

---

## 5. ES6 Features

### 5.1 `let` and `const`
- `let` allows block-scoped variables.
- `const` is for constant variables.

```js
let x = 10;
const y = 20;
```

### 5.2 Template Literals
Use backticks (`) to include variables inside a string.
```js
let name = "Alice";
console.log(`Hello, ${name}!`); // Hello, Alice!
```

### 5.3 Destructuring Assignment
Extract values from arrays or objects easily.
```js
const person = { name: "Bob", age: 25 };
const { name, age } = person;
console.log(name, age); // Bob 25
```

### 5.4 Spread & Rest Operators
#### Spread (`...`) - Expands an array/object.
```js
let nums = [1, 2, 3];
let newNums = [...nums, 4, 5];
console.log(newNums); // [1, 2, 3, 4, 5]
```

#### Rest (`...`) - Gathers remaining arguments into an array.
```js
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}

console.log(sum(1, 2, 3, 4)); // 10
```

### 5.5 Arrow Functions
Already covered earlier.

### 5.6 Promises (Async Code Handling)
Used for handling asynchronous operations.
```js
function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve("Data received"), 2000);
    });
}

fetchData().then(console.log); // Prints "Data received" after 2s
```

### 5.7 Modules (`import` and `export`)
**Exporting:**
```js
// file: utils.js
export function greet(name) {
    return `Hello, ${name}!`;
}
```

**Importing:**
```js
// file: main.js
import { greet } from "./utils.js";

console.log(greet("Alice"));
```

---

## Summary of Part 2:
✔ Functions in JavaScript (Declaration, Expression, Arrow)  
✔ Function parameters & default values  
✔ Variable scope (Global, Function, Block)  
✔ Closures (functions remembering outer variables)  
✔ ES6 Features (`let`, `const`, template literals, destructuring, spread/rest, promises, modules)

---

**Coming Next in Part 3:**  
- **Objects & Prototypes**  
- **Array Methods**  
- **Higher-Order Functions**  
- **Event Handling in JavaScript**  

Would you like any specific topic explained in more detail? 😊