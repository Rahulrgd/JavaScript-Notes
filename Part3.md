# JavaScript Notes – Part 3: Objects, Prototypes, Arrays, and Events

In this part, we will dive deeper into **Objects, Prototypes, Arrays, Higher-Order Functions, and Event Handling** in JavaScript.

---

## 1. Objects in JavaScript

### 1.1 Creating Objects
JavaScript objects store key-value pairs and are similar to Java’s `Map` or `POJO (Plain Old Java Object)`.

#### Object Literal
```js
const person = {
    name: "Alice",
    age: 25,
    greet: function() {
        console.log(`Hello, my name is ${this.name}`);
    }
};

person.greet(); // Hello, my name is Alice
```

#### Java Equivalent:
```java
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    void greet() {
        System.out.println("Hello, my name is " + this.name);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person("Alice", 25);
        p.greet(); // Hello, my name is Alice
    }
}
```

---

### 1.2 Accessing Object Properties
```js
console.log(person.name); // Alice
console.log(person["age"]); // 25
```

### 1.3 Adding & Deleting Properties
```js
person.country = "USA"; // Add property
delete person.age; // Remove property
```

---

## 2. Prototypes in JavaScript

JavaScript uses **prototype-based inheritance** instead of class-based inheritance like Java.

### 2.1 Prototype Inheritance
```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function() {
    console.log(`Hello, I am ${this.name}`);
};

const alice = new Person("Alice", 25);
alice.greet(); // Hello, I am Alice
```

### Java Equivalent (Class-based Inheritance)
```java
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    void greet() {
        System.out.println("Hello, I am " + this.name);
    }
}

public class Main {
    public static void main(String[] args) {
        Person alice = new Person("Alice", 25);
        alice.greet(); // Hello, I am Alice
    }
}
```

---

### 2.2 ES6 Class Syntax (Alternative to Prototype)
JavaScript introduced `class` in ES6 for a more Java-like syntax.
```js
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    greet() {
        console.log(`Hello, I am ${this.name}`);
    }
}

const bob = new Person("Bob", 30);
bob.greet(); // Hello, I am Bob
```

---

## 3. Arrays in JavaScript

### 3.1 Creating Arrays
```js
let fruits = ["Apple", "Banana", "Cherry"];
console.log(fruits[0]); // Apple
```

### 3.2 Array Methods
```js
fruits.push("Mango"); // Add at end
fruits.pop(); // Remove from end
fruits.unshift("Grapes"); // Add at beginning
fruits.shift(); // Remove from beginning
```

### 3.3 Iterating Over an Array
#### Using `forEach`
```js
fruits.forEach(fruit => console.log(fruit));
```
#### Using `for...of`
```js
for (let fruit of fruits) {
    console.log(fruit);
}
```

---

## 4. Higher-Order Functions (Array Methods)

### 4.1 `map()`
Applies a function to each array element and returns a new array.
```js
const numbers = [1, 2, 3, 4];
const squared = numbers.map(num => num ** 2);
console.log(squared); // [1, 4, 9, 16]
```

### 4.2 `filter()`
Filters elements based on a condition.
```js
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4]
```

### 4.3 `reduce()`
Reduces an array to a single value.
```js
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum); // 10
```

### Java Equivalent (Using Streams)
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4);

        List<Integer> squared = numbers.stream()
                                       .map(n -> n * n)
                                       .collect(Collectors.toList());
        System.out.println(squared); // [1, 4, 9, 16]

        List<Integer> evenNumbers = numbers.stream()
                                           .filter(n -> n % 2 == 0)
                                           .collect(Collectors.toList());
        System.out.println(evenNumbers); // [2, 4]

        int sum = numbers.stream().reduce(0, Integer::sum);
        System.out.println(sum); // 10
    }
}
```

---

## 5. Event Handling in JavaScript

### 5.1 Handling Click Events
```html
<button id="btn">Click Me</button>

<script>
    document.getElementById("btn").addEventListener("click", function() {
        alert("Button Clicked!");
    });
</script>
```

### 5.2 Handling Keyboard Events
```js
document.addEventListener("keydown", function(event) {
    console.log(`Key pressed: ${event.key}`);
});
```

---

## Summary of Part 3:
✔ Objects and Prototypes  
✔ ES6 Classes (Alternative to Prototype)  
✔ Array Methods (`map()`, `filter()`, `reduce()`)  
✔ Higher-Order Functions  
✔ Event Handling in JavaScript  

---

**Coming Next in Part 4:**  
- **Asynchronous JavaScript (Callbacks, Promises, Async/Await)**  
- **Error Handling in JavaScript**  
- **Fetching Data with APIs (AJAX & Fetch API)**  

Would you like to explore any topic in more depth? 😊