# JavaScript Notes – Part 7: Object-Oriented Programming (OOP)  

In this part, we will cover **Object-Oriented Programming (OOP) in JavaScript**, including **Classes, Constructors, Prototypes, Inheritance, and Encapsulation**.  

---

## 1. Introduction to OOP in JavaScript  

Object-Oriented Programming (OOP) is a programming paradigm that allows **structuring code using objects**. JavaScript supports OOP through **prototypes** and the ES6 **class** syntax.

**Four Pillars of OOP:**
1. **Encapsulation** – Hiding details and exposing only necessary parts.
2. **Abstraction** – Hiding implementation complexity.
3. **Inheritance** – Reusing code from parent classes.
4. **Polymorphism** – Overriding or modifying methods in derived classes.

---

## 2. Objects in JavaScript  

Objects store properties and methods.  

### **2.1 Creating Objects (Object Literal Syntax)**  
```js
const person = {
    name: "Alice",
    age: 25,
    greet: function () {
        console.log(`Hello, my name is ${this.name}`);
    }
};

person.greet(); // Hello, my name is Alice
```
- **`this`** refers to the current object.

---

## 3. Constructor Functions (Before ES6)  

Before ES6 classes, objects were created using constructor functions.

```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function () {
        console.log(`Hello, my name is ${this.name}`);
    };
}

const person1 = new Person("Alice", 25);
person1.greet(); // Hello, my name is Alice
```

---

## 4. ES6 Classes  

ES6 introduced the `class` keyword, making object creation more readable.  

### **4.1 Defining a Class**  
```js
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    greet() {
        console.log(`Hello, my name is ${this.name}`);
    }
}

const person1 = new Person("Alice", 25);
person1.greet(); // Hello, my name is Alice
```

---

## 5. Inheritance in JavaScript  

Inheritance allows a class to extend another class.

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    makeSound() {
        console.log("Some generic sound...");
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name); // Calls the parent constructor
        this.breed = breed;
    }

    makeSound() {
        console.log("Bark! Bark!");
    }
}

const dog = new Dog("Buddy", "Golden Retriever");
dog.makeSound(); // Bark! Bark!
console.log(dog.name); // Buddy
```
- `super(name)` calls the parent class constructor.

---

## 6. Encapsulation in JavaScript  

Encapsulation means **hiding data** using **private properties**.

### **6.1 Private Properties (`#`)**
```js
class BankAccount {
    #balance; // Private property

    constructor(owner, balance) {
        this.owner = owner;
        this.#balance = balance;
    }

    deposit(amount) {
        this.#balance += amount;
        console.log(`Deposited: $${amount}`);
    }

    getBalance() {
        return `Balance: $${this.#balance}`;
    }
}

const account = new BankAccount("Alice", 1000);
account.deposit(500);
console.log(account.getBalance()); // Balance: $1500
console.log(account.#balance); // Error: Private field '#balance' must be declared in an enclosing class
```
- The `#balance` property is **private** and cannot be accessed outside the class.

---

## 7. Static Methods and Properties  

Static members belong to the **class itself** rather than an instance.

```js
class MathUtil {
    static square(num) {
        return num * num;
    }
}

console.log(MathUtil.square(5)); // 25
```
- `square()` can be called **without creating an object**.

---

## 8. Polymorphism in JavaScript  

Polymorphism allows **method overriding**.

```js
class Vehicle {
    start() {
        console.log("Vehicle is starting...");
    }
}

class Car extends Vehicle {
    start() {
        console.log("Car is starting...");
    }
}

const myCar = new Car();
myCar.start(); // Car is starting...
```
- The `start()` method is overridden in the `Car` class.

---

## Summary of Part 7:
✔ Objects & Classes  
✔ Constructor Functions (Pre-ES6)  
✔ Inheritance (`extends`, `super()`)  
✔ Encapsulation (`private` properties)  
✔ Static Methods  
✔ Polymorphism (Overriding Methods)  

---

**Coming Next in Part 8:**  
- **Asynchronous JavaScript (Callbacks, Promises, Async/Await)**  
- **Event Loop and Execution Context**  

Would you like me to add real-world examples for OOP concepts in JavaScript? 😊