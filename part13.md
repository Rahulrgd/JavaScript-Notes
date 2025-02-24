# JavaScript Notes – Part 13: Promises in JavaScript  

In this part, we will cover:  
- What are Promises?  
- Creating and using Promises  
- Chaining Promises (`then`, `catch`, `finally`)  
- `Promise.all`, `Promise.any`, `Promise.race`  
- Comparison with async/await  

---

## 1. What is a Promise?  

A **Promise** is an object that represents the **future result** of an **asynchronous** operation.  
A Promise can have **three states**:  

1. **Pending** → Initial state (waiting for result)  
2. **Fulfilled** → Operation was successful  
3. **Rejected** → Operation failed  

### **Analogy:**  
Think of a Promise as **ordering food at a restaurant**:  
- You **place an order** (Promise is **Pending**)  
- If the food is served, the Promise is **Fulfilled**  
- If the restaurant cancels the order, the Promise is **Rejected**  

---

## 2. Creating a Promise  

```js
const myPromise = new Promise((resolve, reject) => {
    let success = true; // Simulate success or failure

    setTimeout(() => {
        if (success) {
            resolve("Order completed!");
        } else {
            reject("Order failed!");
        }
    }, 2000);
});

// Using the Promise
myPromise
    .then(response => console.log(response))  // Runs if resolved
    .catch(error => console.error(error))    // Runs if rejected
    .finally(() => console.log("Process finished")); // Runs always
```
- `resolve("Order completed!")` → Calls `.then()`
- `reject("Order failed!")` → Calls `.catch()`
- `.finally()` runs **regardless** of success or failure  

---

## 3. Chaining Promises  

**Scenario:** Fetching user data and then fetching their orders.  
```js
function getUser() {
    return new Promise((resolve) => {
        setTimeout(() => resolve({ id: 1, name: "Alice" }), 1000);
    });
}

function getOrders(userId) {
    return new Promise((resolve) => {
        setTimeout(() => resolve(["Order 1", "Order 2"]), 1500);
    });
}

// Chaining Promises
getUser()
    .then(user => {
        console.log("User:", user);
        return getOrders(user.id);
    })
    .then(orders => console.log("Orders:", orders))
    .catch(error => console.error(error));
```
- `.then()` can return **another** Promise  
- The next `.then()` waits for the returned Promise  

---

## 4. Handling Multiple Promises  

### **4.1 `Promise.all()` (Waits for all Promises)**  
Useful when **all** tasks must complete.  
```js
let p1 = new Promise(res => setTimeout(() => res("Data 1"), 1000));
let p2 = new Promise(res => setTimeout(() => res("Data 2"), 2000));

Promise.all([p1, p2])
    .then(results => console.log(results)) // ["Data 1", "Data 2"]
    .catch(error => console.error(error));
```
- If **one** Promise fails, `Promise.all` **fails**  

### **4.2 `Promise.any()` (Returns First Successful One)**  
```js
let p1 = new Promise((_, rej) => setTimeout(() => rej("Error 1"), 1000));
let p2 = new Promise(res => setTimeout(() => res("Data 2"), 2000));

Promise.any([p1, p2])
    .then(result => console.log(result)) // "Data 2"
    .catch(error => console.error(error));
```
- Returns **first resolved** Promise  
- If **all fail**, `Promise.any` fails  

### **4.3 `Promise.race()` (Fastest Promise Wins, Success or Fail)**  
```js
let p1 = new Promise((_, rej) => setTimeout(() => rej("Error 1"), 1000));
let p2 = new Promise(res => setTimeout(() => res("Data 2"), 2000));

Promise.race([p1, p2])
    .then(result => console.log(result))
    .catch(error => console.error(error));
```
- Returns **first completed** Promise  
- **Even if it fails**, the race ends  

---

## 5. Promises vs. async/await  

### **Using Promises**  
```js
fetch("https://jsonplaceholder.typicode.com/posts/1")
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error("Error:", error));
```

### **Using async/await (Cleaner Code)**  
```js
async function fetchData() {
    try {
        let response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
        let data = await response.json();
        console.log(data);
    } catch (error) {
        console.error("Error:", error);
    }
}

fetchData();
```
**Why async/await?**  
- **More readable** than `.then() chaining`  
- **Handles errors easily** with `try...catch`  
- **Works with Promises** (it's **not** a replacement)  

---

## Summary of Part 13:
✔ What are Promises?  
✔ Creating and chaining Promises  
✔ Handling multiple Promises (`all`, `any`, `race`)  
✔ Comparing Promises with async/await  

---

### **Coming Next in Part 14:**  
- **JavaScript Events & Event Listeners**  
- **Handling Click, Input, and Keyboard Events**  
- **Event Delegation & Bubbling**  

Would you like a small hands-on task on Promises before we proceed? 😊