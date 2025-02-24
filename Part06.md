# JavaScript Notes – Part 6: ES6 Features, Modules, and Closures  

In this part, we will cover **ES6 Features (Destructuring, Spread, Rest, Template Literals), JavaScript Modules (Import/Export), and Closures & Scope.**  

---

## 1. ES6 Features  

ES6 (ECMAScript 2015) introduced powerful features that make JavaScript code **cleaner and more efficient**.  

### **1.1 let and const (Block Scope Variables)**  
Before ES6, JavaScript used `var`, which had **function scope**. ES6 introduced `let` and `const` for **block scope**.

```js
if (true) {
    let x = 10;
    console.log(x); // 10
}
console.log(x); // Error: x is not defined
```
- `let` can be reassigned, while `const` **cannot be changed** after declaration.  

```js
const PI = 3.14;
PI = 3.15; // Error: Assignment to constant variable.
```

---

### **1.2 Arrow Functions (`=>`)**  

Arrow functions provide a shorter syntax for writing functions.

#### **Regular Function**  
```js
function add(a, b) {
    return a + b;
}
```

#### **Arrow Function Equivalent**  
```js
const add = (a, b) => a + b;
console.log(add(5, 3)); // 8
```

- If there is **only one parameter**, parentheses can be omitted:  
```js
const greet = name => `Hello, ${name}`;
console.log(greet("Alice")); // Hello, Alice
```

- If there is **no parameter**, use `() => {}`:  
```js
const sayHello = () => console.log("Hello!");
sayHello();
```

🚀 **Arrow Functions & `this`**  
Unlike regular functions, arrow functions **do not have their own `this`**, which is useful for preserving context.

```js
function Person(name) {
    this.name = name;
    setTimeout(() => {
        console.log("Hello, " + this.name); // Works as expected
    }, 1000);
}

new Person("Alice");
```

---

### **1.3 Template Literals (`Backticks` `` ` ` ``)**  
Template literals allow **multi-line strings** and **variable interpolation**.

```js
let name = "Alice";
let message = `Hello, ${name}! Welcome.`;
console.log(message); // Hello, Alice! Welcome.
```

🚀 **Multiline Strings**  
```js
let text = `This is
a multi-line
string.`;
console.log(text);
```

---

### **1.4 Destructuring (Extract Values from Arrays/Objects)**  

#### **Array Destructuring**  
```js
let numbers = [1, 2, 3];
let [a, b, c] = numbers;
console.log(a, b, c); // 1 2 3
```

#### **Object Destructuring**  
```js
let person = { name: "Alice", age: 25 };
let { name, age } = person;
console.log(name, age); // Alice 25
```

---

### **1.5 Spread and Rest Operators (`...`)**  

#### **Spread Operator (`...`) – Expanding Arrays & Objects**  
```js
let arr1 = [1, 2, 3];
let arr2 = [...arr1, 4, 5];
console.log(arr2); // [1, 2, 3, 4, 5]

let obj1 = { name: "Alice" };
let obj2 = { ...obj1, age: 25 };
console.log(obj2); // { name: "Alice", age: 25 }
```

#### **Rest Operator (`...`) – Collecting Function Arguments**  
```js
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num);
}

console.log(sum(1, 2, 3, 4)); // 10
```

---

## 2. JavaScript Modules (Import & Export)  

Before ES6, JavaScript did not have native module support. ES6 introduced **modules** to better structure code.  

### **2.1 Exporting a Module**  

#### **Named Export**  
```js
// file: math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
```

#### **Default Export**  
```js
// file: greet.js
export default function greet(name) {
    return `Hello, ${name}!`;
}
```

---

### **2.2 Importing a Module**  

#### **Importing Named Exports**  
```js
import { add, subtract } from "./math.js";
console.log(add(5, 3)); // 8
console.log(subtract(5, 3)); // 2
```

#### **Importing Default Export**  
```js
import greet from "./greet.js";
console.log(greet("Alice")); // Hello, Alice!
```

---

## 3. Closures & Scope  

### **3.1 Scope in JavaScript**  

- **Global Scope** – Variables accessible everywhere.  
- **Function Scope** – Variables inside a function cannot be accessed outside.  
- **Block Scope** – Variables declared with `let` or `const` inside `{}` are limited to that block.  

```js
let globalVar = "I am global";

function test() {
    let functionVar = "I am inside function";
    console.log(globalVar); // Accessible
}

test();
console.log(functionVar); // Error: functionVar is not defined
```

---

### **3.2 Closures in JavaScript**  

A **closure** is a function that **remembers the variables from its outer scope** even after the outer function has executed.  

```js
function outerFunction(outerVariable) {
    return function innerFunction(innerVariable) {
        console.log(`Outer: ${outerVariable}, Inner: ${innerVariable}`);
    };
}

const newFunc = outerFunction("Hello");
newFunc("World"); // Outer: Hello, Inner: World
```

🚀 **Practical Example: Private Variables**  
```js
function counter() {
    let count = 0;
    return function () {
        count++;
        console.log(`Count: ${count}`);
    };
}

const increment = counter();
increment(); // Count: 1
increment(); // Count: 2
increment(); // Count: 3
```
Each call to `counter()` creates a **new scope**, and the `count` variable is **preserved** inside.

---

## Summary of Part 6:
✔ ES6 Features (`let`, `const`, Arrow Functions, Template Literals)  
✔ Destructuring, Spread & Rest Operators  
✔ JavaScript Modules (`import` & `export`)  
✔ Closures & Scope  

---

**Coming Next in Part 7:**  
- **Object-Oriented Programming (OOP) in JavaScript**  
- **Classes, Constructors, and Prototypes**  
- **Inheritance & Encapsulation**  

Would you like me to include a real-world example using JavaScript modules in the next part? 😊