# JavaScript Notes – Part 16: Local Storage & Session Storage  

In this part, we will cover:  
- What is Web Storage?  
- `localStorage` vs `sessionStorage`  
- Storing and retrieving data  
- Removing and clearing storage  
- Storing complex data (objects & arrays)  
- Practical examples  

---

## 1. What is Web Storage?  

Web Storage allows us to store **key-value** data in the browser.  
It is useful for persisting data without needing a database.  

There are two types of web storage:  
1. **`localStorage`** – Data persists **even after closing the browser**.  
2. **`sessionStorage`** – Data is lost when the **tab is closed**.  

---

## 2. `localStorage` vs `sessionStorage`  

| Feature          | `localStorage` | `sessionStorage` |
|-----------------|---------------|-----------------|
| Data Persistence | Until manually removed | Until tab is closed |
| Scope           | All browser tabs/windows | Only the current tab |
| Storage Limit   | 5MB+ | 5MB+ |
| Use Case        | User preferences, settings | Temporary session data |

---

## 3. Storing & Retrieving Data  

### **3.1 Storing Data (`setItem`)**  
```js
localStorage.setItem("username", "Alice");
sessionStorage.setItem("sessionID", "12345");
```
- Stores key-value pairs as **strings**.  

### **3.2 Retrieving Data (`getItem`)**  
```js
let user = localStorage.getItem("username");
console.log(user); // "Alice"
```
- Returns the stored value or `null` if not found.  

### **3.3 Removing Data (`removeItem`)**  
```js
localStorage.removeItem("username");
sessionStorage.removeItem("sessionID");
```
- Removes a specific item.  

### **3.4 Clearing All Data (`clear`)**  
```js
localStorage.clear();
sessionStorage.clear();
```
- Removes **all** stored data.  

---

## 4. Storing Complex Data (Objects & Arrays)  

Since `localStorage` and `sessionStorage` store only **strings**, we must convert objects/arrays to JSON.  

### **4.1 Storing an Object**  
```js
let user = { name: "Alice", age: 25 };
localStorage.setItem("user", JSON.stringify(user));
```
- `JSON.stringify(obj)` converts an object into a **string**.  

### **4.2 Retrieving an Object**  
```js
let userData = JSON.parse(localStorage.getItem("user"));
console.log(userData.name); // "Alice"
```
- `JSON.parse(str)` converts the string back into an **object**.  

---

## 5. Practical Examples  

### **5.1 Saving User Preferences**  
```js
function saveTheme(theme) {
    localStorage.setItem("theme", theme);
    document.body.className = theme;
}

function loadTheme() {
    let savedTheme = localStorage.getItem("theme");
    if (savedTheme) {
        document.body.className = savedTheme;
    }
}

// Load theme on page load
loadTheme();
```
- Saves and loads the user’s selected theme.  

### **5.2 Counting Page Visits**  
```js
let visits = localStorage.getItem("visits") || 0;
visits++;
localStorage.setItem("visits", visits);
console.log(`You have visited this page ${visits} times.`);
```
- Tracks how many times a user visits the page.  

---

## Summary of Part 16:
✔ Difference between `localStorage` and `sessionStorage`  
✔ Storing, retrieving, and removing data  
✔ Handling objects & arrays in storage  
✔ Practical use cases  

---

### **Coming Next in Part 17:**  
- **Async/Await (Deep Dive)**  
- **Error Handling in Async Code**  
- **Practical Examples of Async/Await**  

Would you like a small project using localStorage before we move on? 😊