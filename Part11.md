# JavaScript Notes – Part 11: Handling Forms & User Input  

In this part, we will cover **handling forms in JavaScript**, including:  
- Capturing user input  
- Form validation  
- Preventing default form submission  
- Using localStorage and sessionStorage  

---

## 1. Capturing User Input  

JavaScript can capture form input values dynamically.

### **1.1 Getting Input Field Value**  
```html
<input type="text" id="username" placeholder="Enter name">
<button onclick="getUserInput()">Submit</button>

<script>
    function getUserInput() {
        let name = document.getElementById("username").value;
        console.log("User entered:", name);
    }
</script>
```
- The `.value` property retrieves the entered value.

### **1.2 Handling Form Submission**  
```html
<form id="userForm">
    <input type="text" id="name" placeholder="Enter name">
    <button type="submit">Submit</button>
</form>

<script>
    document.getElementById("userForm").addEventListener("submit", function(event) {
        event.preventDefault(); // Prevent page reload
        let name = document.getElementById("name").value;
        console.log("Submitted name:", name);
    });
</script>
```
- `event.preventDefault()` stops the form from reloading the page.

---

## 2. Basic Form Validation  

### **2.1 Checking if a Field is Empty**  
```js
function validateForm() {
    let name = document.getElementById("name").value;
    if (name.trim() === "") {
        alert("Name cannot be empty!");
        return false;
    }
    return true;
}
```

### **2.2 Validating Email Format**  
```js
function validateEmail() {
    let email = document.getElementById("email").value;
    let regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!regex.test(email)) {
        alert("Invalid email format!");
        return false;
    }
    return true;
}
```

### **2.3 Showing Validation Errors in the UI**  
```html
<input type="text" id="username">
<p id="error" style="color: red;"></p>

<script>
    function validate() {
        let username = document.getElementById("username").value;
        let errorMsg = document.getElementById("error");

        if (username.length < 3) {
            errorMsg.textContent = "Username must be at least 3 characters.";
        } else {
            errorMsg.textContent = "";
        }
    }

    document.getElementById("username").addEventListener("input", validate);
</script>
```
- The `input` event updates validation **dynamically** as the user types.

---

## 3. Storing Data in Local Storage & Session Storage  

### **3.1 localStorage (Persistent Storage)**  
Stores data **even after page reload**.  
```js
localStorage.setItem("username", "JohnDoe");
console.log(localStorage.getItem("username")); // "JohnDoe"
localStorage.removeItem("username"); // Remove data
localStorage.clear(); // Clear all data
```

### **3.2 sessionStorage (Data for a Single Session)**  
Data **disappears when the tab is closed**.  
```js
sessionStorage.setItem("sessionUser", "Alice");
console.log(sessionStorage.getItem("sessionUser")); // "Alice"
sessionStorage.removeItem("sessionUser");
sessionStorage.clear();
```

### **3.3 Saving User Input in localStorage**  
```html
<input type="text" id="nameInput" placeholder="Enter name">
<button onclick="saveName()">Save</button>

<script>
    function saveName() {
        let name = document.getElementById("nameInput").value;
        localStorage.setItem("savedName", name);
        alert("Name saved!");
    }

    document.getElementById("nameInput").value = localStorage.getItem("savedName") || "";
</script>
```
- The input field **remembers** the value after refreshing the page.

---

## Summary of Part 11:
✔ Handling user input (`.value`)  
✔ Preventing default form submission  
✔ Form validation (empty fields, email format)  
✔ Storing data in `localStorage` and `sessionStorage`  

---

**Coming Next in Part 12:**  
- **JavaScript Fetch API (GET, POST requests)**  
- **Handling API responses (JSON, errors)**  
- **Building a simple app using Fetch API**  

Would you like a small project on form validation before moving forward? 😊