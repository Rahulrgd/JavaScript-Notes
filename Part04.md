# JavaScript Notes – Part 4: Asynchronous JavaScript, Promises, and API Calls  

In this part, we will cover **Asynchronous JavaScript, Callbacks, Promises, Async/Await, Error Handling, and API Calls**.

---

## 1. Synchronous vs Asynchronous JavaScript

JavaScript is **single-threaded**, meaning it executes one task at a time. However, using asynchronous programming, we can handle time-consuming operations without blocking the main thread.

### Example of Synchronous Execution:
```js
console.log("Start");
console.log("Processing...");
console.log("End");
```
**Output:**
```
Start
Processing...
End
```

### Example of Asynchronous Execution:
```js
console.log("Start");

setTimeout(() => {
    console.log("Processing...");
}, 2000);

console.log("End");
```
**Output:**
```
Start
End
Processing... (after 2 seconds)
```

---

## 2. Callbacks in JavaScript

A **callback** is a function passed as an argument to another function and executed later.

### Example of a Callback:
```js
function fetchData(callback) {
    setTimeout(() => {
        console.log("Data received");
        callback();
    }, 2000);
}

function processData() {
    console.log("Processing data...");
}

fetchData(processData);
```

### Callback Hell (Nested Callbacks)
When multiple callbacks are nested, it creates difficult-to-read code.
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
**Solution:** **Use Promises**

---

## 3. Promises in JavaScript

A **Promise** represents an asynchronous operation that will either **resolve** (success) or **reject** (failure).

### Creating a Promise:
```js
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        let success = true; // Change to false to simulate rejection
        if (success) {
            resolve("Data fetched successfully!");
        } else {
            reject("Error fetching data.");
        }
    }, 2000);
});

// Using the Promise
myPromise
    .then(response => console.log(response)) // Runs if resolved
    .catch(error => console.log(error)) // Runs if rejected
    .finally(() => console.log("Operation Completed"));
```

**Java Equivalent (Using `CompletableFuture`):**
```java
import java.util.concurrent.CompletableFuture;

public class Main {
    public static void main(String[] args) {
        CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(2000);
                return "Data fetched successfully!";
            } catch (InterruptedException e) {
                return "Error fetching data.";
            }
        }).thenAccept(System.out::println);
    }
}
```

---

## 4. Async/Await (ES8)

`async` and `await` simplify handling Promises, making code look synchronous.

### Example of Async/Await:
```js
function fetchData() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("Data fetched successfully!");
        }, 2000);
    });
}

async function getData() {
    console.log("Fetching data...");
    let result = await fetchData();
    console.log(result);
}

getData();
```
**Output:**
```
Fetching data...
(Data fetched successfully! after 2 seconds)
```

### Error Handling with Try...Catch:
```js
async function getData() {
    try {
        let result = await fetchData();
        console.log(result);
    } catch (error) {
        console.log("Error:", error);
    }
}

getData();
```

---

## 5. Fetch API (Making API Calls)

The **Fetch API** is used to make network requests.

### 5.1 Fetching Data from an API
```js
fetch("https://jsonplaceholder.typicode.com/posts/1")
    .then(response => response.json()) // Convert response to JSON
    .then(data => console.log(data))
    .catch(error => console.log("Error:", error));
```

### 5.2 Fetching Data with Async/Await
```js
async function fetchData() {
    try {
        let response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
        let data = await response.json();
        console.log(data);
    } catch (error) {
        console.log("Error:", error);
    }
}

fetchData();
```

---

## 6. Error Handling in JavaScript

### 6.1 Try...Catch Block
```js
try {
    let result = someUndefinedFunction();
} catch (error) {
    console.log("An error occurred:", error.message);
}
```

### 6.2 Throwing Custom Errors
```js
function divide(a, b) {
    if (b === 0) {
        throw new Error("Cannot divide by zero");
    }
    return a / b;
}

try {
    console.log(divide(10, 2)); // 5
    console.log(divide(10, 0)); // Throws error
} catch (error) {
    console.log("Error:", error.message);
}
```

---

## Summary of Part 4:
✔ Synchronous vs Asynchronous Execution  
✔ Callbacks and Callback Hell  
✔ Promises (resolve, reject, then, catch)  
✔ Async/Await (Handling Promises more cleanly)  
✔ Fetch API (Making HTTP Requests)  
✔ Error Handling (Try-Catch, Throwing Errors)  

---

**Coming Next in Part 5:**  
- **DOM Manipulation (Selecting, Modifying Elements, Events)**  
- **Local Storage & Session Storage**  
- **Timers (setTimeout & setInterval)**  

Would you like me to include real-world examples for API calls in the next part? 😊