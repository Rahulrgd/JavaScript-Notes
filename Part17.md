# JavaScript Notes – Part 17: Async/Await (Deep Dive)  

In this part, we will cover:  
- What is Asynchronous JavaScript?  
- Promises Recap  
- Understanding `async` and `await`  
- Error Handling with `try...catch`  
- Practical Examples  

---

## 1. What is Asynchronous JavaScript?  

JavaScript is **single-threaded**, meaning it executes one statement at a time.  
To handle tasks like fetching data from an API or reading a file, we use **asynchronous programming**.  

🔹 **Synchronous Code Example (Blocking Execution)**  
```js
console.log("Start");
alert("Blocking Alert!"); // Stops everything until user clicks OK
console.log("End");
```
- The `alert()` function **blocks execution** until the user responds.  

🔹 **Asynchronous Code Example (Non-Blocking Execution)**  
```js
console.log("Start");

setTimeout(() => {
    console.log("Async Task Done");
}, 2000);

console.log("End");
```
- `setTimeout` runs **after 2 seconds**, but execution **continues immediately**.  

---

## 2. Promises Recap  

A **Promise** represents a value that may be available **now, later, or never**.  

### **2.1 Creating a Promise**  
```js
let myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        let success = true;
        if (success) resolve("Data Loaded!");
        else reject("Error Occurred!");
    }, 2000);
});

myPromise.then(result => console.log(result)).catch(error => console.error(error));
```
- `resolve(value)` → Completes successfully.  
- `reject(error)` → Fails with an error.  
- `.then(successCallback)` handles success.  
- `.catch(errorCallback)` handles failure.  

---

## 3. Understanding `async` and `await`  

🔹 **What is `async`?**  
- Declares a function as asynchronous.  
- Always **returns a Promise**.  

🔹 **What is `await`?**  
- Waits for a Promise to resolve **before proceeding**.  
- Can **only be used inside `async` functions**.  

### **3.1 Using `async` and `await`**  
```js
async function fetchData() {
    return "Data Retrieved!";
}

fetchData().then(result => console.log(result));
```
- Since `async` functions always return a **Promise**, we use `.then()` to get the value.  

### **3.2 `await` for Simplicity**  
```js
async function fetchData() {
    let data = await new Promise(resolve => setTimeout(() => resolve("Data Retrieved!"), 2000));
    console.log(data);
}

fetchData();
```
- `await` pauses execution **until the Promise resolves**.  

---

## 4. Fetching Data with `async/await`  

### **4.1 Fetching JSON Data from an API**  
```js
async function getUser() {
    let response = await fetch("https://jsonplaceholder.typicode.com/users/1");
    let user = await response.json();
    console.log(user);
}

getUser();
```
- `fetch(url)` returns a **Promise**.  
- `response.json()` also returns a **Promise**, so we use `await`.  

---

## 5. Error Handling with `try...catch`  

If something goes wrong (e.g., network failure), we should handle errors properly.  

```js
async function getUser() {
    try {
        let response = await fetch("https://jsonplaceholder.typicode.com/users/1");
        if (!response.ok) throw new Error("Failed to fetch user!");
        let user = await response.json();
        console.log(user);
    } catch (error) {
        console.error("Error:", error.message);
    }
}

getUser();
```
- `try...catch` prevents crashes if an error occurs.  
- `throw new Error("message")` generates a custom error.  

---

## 6. Parallel Async Calls with `Promise.all`  

If you need to **run multiple async tasks simultaneously**, use `Promise.all()`.  

```js
async function fetchMultiple() {
    let [user, post] = await Promise.all([
        fetch("https://jsonplaceholder.typicode.com/users/1").then(res => res.json()),
        fetch("https://jsonplaceholder.typicode.com/posts/1").then(res => res.json())
    ]);
    
    console.log(user, post);
}

fetchMultiple();
```
- **Executes both requests at the same time**, reducing wait time.  
- Returns an **array** of results when all Promises resolve.  

---

## Summary of Part 17:
✔ Understanding async programming in JavaScript  
✔ `async/await` for cleaner asynchronous code  
✔ Handling errors with `try...catch`  
✔ Running multiple async tasks with `Promise.all()`  

---

### **Coming Next in Part 18:**  
- **AJAX & Fetch API in Detail**  
- **Handling Different Response Types (JSON, Text, Blob, etc.)**  
- **Practical API Request Examples**  

Would you like a hands-on task using `async/await` before moving forward? 😊