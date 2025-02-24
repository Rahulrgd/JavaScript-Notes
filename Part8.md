# JavaScript Notes – Part 8: Asynchronous JavaScript  

In this part, we will cover **Asynchronous Programming in JavaScript**, including **Callbacks, Promises, Async/Await, and the Event Loop.**  

---

## 1. Introduction to Asynchronous JavaScript  

JavaScript is **single-threaded**, meaning it executes **one task at a time** in a synchronous manner. However, some tasks (e.g., fetching data, reading files, timers) take time to complete. JavaScript uses **asynchronous programming** to handle such tasks efficiently.  

---

## 2. Callbacks (Pre-ES6)  

A **callback** is a function passed as an argument to another function and is executed later.

```js
function fetchData(callback) {
    setTimeout(() => {
        console.log("Data fetched");
        callback();
    }, 2000);
}

function processData() {
    console.log("Processing data...");
}

fetchData(processData);
```
### **Problems with Callbacks (Callback Hell)**  
When multiple callbacks are nested, it leads to unreadable code known as **callback hell**.

```js
setTimeout(() => {
    console.log("Step 1");
    setTimeout(() => {
        console.log("Step 2");
        setTimeout(() => {
            console.log("Step 3");
        }, 1000);
    }, 1000);
}, 1000);
```
To solve this problem, **Promises** were introduced in ES6.

---

## 3. Promises (ES6)  

A **Promise** is an object that represents **a future value** (pending, fulfilled, or rejected).  

### **3.1 Creating a Promise**  

```js
const fetchData = new Promise((resolve, reject) => {
    setTimeout(() => {
        let success = true;
        if (success) {
            resolve("Data fetched successfully!");
        } else {
            reject("Error fetching data!");
        }
    }, 2000);
});

fetchData
    .then((message) => console.log(message)) // On success
    .catch((error) => console.log(error)); // On failure
```

### **3.2 Chaining Promises**  

```js
function step1() {
    return new Promise((resolve) => {
        setTimeout(() => {
            console.log("Step 1 completed");
            resolve();
        }, 1000);
    });
}

function step2() {
    return new Promise((resolve) => {
        setTimeout(() => {
            console.log("Step 2 completed");
            resolve();
        }, 1000);
    });
}

step1().then(step2).then(() => console.log("All steps completed"));
```

---

## 4. Async/Await (ES8)  

`async` and `await` provide a cleaner way to handle asynchronous code compared to promises.

### **4.1 Using Async/Await**  

```js
async function fetchData() {
    return "Data fetched!";
}

fetchData().then(console.log); // Data fetched!
```

### **4.2 Waiting for an Async Operation**  

```js
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

async function fetchData() {
    console.log("Fetching data...");
    await delay(2000);
    console.log("Data fetched!");
}

fetchData();
```
- `await` **pauses execution** until the promise resolves.

### **4.3 Handling Errors with try...catch**  

```js
async function fetchData() {
    try {
        let response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
        let data = await response.json();
        console.log(data);
    } catch (error) {
        console.log("Error fetching data:", error);
    }
}

fetchData();
```

---

## 5. JavaScript Event Loop  

The **Event Loop** is responsible for handling asynchronous tasks.  

### **5.1 Execution Order in JavaScript**  

```js
console.log("Start");

setTimeout(() => {
    console.log("Inside setTimeout");
}, 0);

console.log("End");
```

**Output:**  
```
Start  
End  
Inside setTimeout
```
- Even though `setTimeout` is set to `0` ms, it is placed in the **callback queue** and executes **after** synchronous code finishes.

---

## Summary of Part 8:
✔ Callbacks & Callback Hell  
✔ Promises & Chaining  
✔ Async/Await for cleaner async code  
✔ JavaScript Event Loop  

---

**Coming Next in Part 9:**  
- **JavaScript Error Handling (`try...catch`, Custom Errors)**  
- **Event Handling & Bubbling in JavaScript**  

Would you like an in-depth example of JavaScript's concurrency model? 😊