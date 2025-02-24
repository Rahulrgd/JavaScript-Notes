# JavaScript Notes – Part 9: Error Handling & Event Handling  

In this part, we will cover **Error Handling in JavaScript (`try...catch`, `throw`, Custom Errors)** and **Event Handling (`addEventListener`, Event Propagation, Bubbling & Capturing)**.  

---

## 1. Error Handling in JavaScript  

JavaScript provides error-handling mechanisms to prevent unexpected crashes in programs.

### **1.1 try...catch**  

The `try...catch` block captures errors to prevent them from stopping execution.

```js
try {
    let result = 10 / 0;
    console.log(result); // Infinity (no error)

    let num = undefinedVariable; // This causes an error
} catch (error) {
    console.log("Error occurred:", error.message);
}

console.log("Code continues..."); // This still executes
```

---

### **1.2 throw Statement**  

You can manually throw errors using `throw`.

```js
function divide(a, b) {
    if (b === 0) {
        throw new Error("Cannot divide by zero");
    }
    return a / b;
}

try {
    console.log(divide(10, 0));
} catch (error) {
    console.log("Caught Error:", error.message);
}
```

---

### **1.3 Custom Errors**  

You can create **custom error classes** by extending `Error`.

```js
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}

function validateAge(age) {
    if (age < 18) {
        throw new ValidationError("Age must be 18 or above.");
    }
}

try {
    validateAge(16);
} catch (error) {
    console.log(`${error.name}: ${error.message}`);
}
```

---

## 2. Event Handling in JavaScript  

JavaScript handles user interactions through **events**.

### **2.1 Adding Event Listeners**  

```html
<button id="btn">Click Me</button>
<script>
    document.getElementById("btn").addEventListener("click", function () {
        alert("Button Clicked!");
    });
</script>
```
- `addEventListener` attaches an event to an element.
- It **does not overwrite** existing event listeners.

---

### **2.2 Removing Event Listeners**  

```js
function sayHello() {
    console.log("Hello!");
}

document.getElementById("btn").addEventListener("click", sayHello);
document.getElementById("btn").removeEventListener("click", sayHello);
```
- The function reference must be **the same** for `removeEventListener` to work.

---

### **2.3 Event Object (`event`)**  

The `event` object provides details about an event.

```js
document.getElementById("btn").addEventListener("click", function (event) {
    console.log("Event Type:", event.type);
    console.log("Event Target:", event.target);
});
```

---

## 3. Event Propagation: Bubbling & Capturing  

### **3.1 Event Bubbling (Default Behavior)**  
In **bubbling**, the event starts from the target element and propagates **upwards** to ancestors.

```html
<div id="parent">
    <button id="child">Click Me</button>
</div>

<script>
    document.getElementById("parent").addEventListener("click", () => {
        console.log("Parent Clicked!");
    });

    document.getElementById("child").addEventListener("click", () => {
        console.log("Child Clicked!");
    });
</script>
```
**Clicking the button outputs:**  
```
Child Clicked!
Parent Clicked!
```

---

### **3.2 Event Capturing (Use `true` in addEventListener)**  

In **capturing mode**, events propagate **from ancestors to the target**.

```js
document.getElementById("parent").addEventListener("click", () => {
    console.log("Parent Clicked!");
}, true);

document.getElementById("child").addEventListener("click", () => {
    console.log("Child Clicked!");
}, true);
```
**Clicking the button outputs:**  
```
Parent Clicked!
Child Clicked!
```

---

### **3.3 Stopping Event Propagation**  

To **stop bubbling** or **capturing**, use `event.stopPropagation()`.

```js
document.getElementById("child").addEventListener("click", function (event) {
    event.stopPropagation();
    console.log("Child Clicked!");
});
```
Now, clicking the button will **not trigger the parent’s event.**

---

### **3.4 Prevent Default Behavior**  

Use `event.preventDefault()` to prevent the default action.

```html
<a href="https://example.com" id="link">Click Me</a>

<script>
    document.getElementById("link").addEventListener("click", function (event) {
        event.preventDefault();
        console.log("Link Click prevented!");
    });
</script>
```
Now, clicking the link **won't navigate** to the URL.

---

## Summary of Part 9:
✔ Error Handling (`try...catch`, `throw`, Custom Errors)  
✔ Event Handling (`addEventListener`, `event` object)  
✔ Event Propagation (Bubbling, Capturing)  
✔ Stopping Event Propagation & Preventing Default Actions  

---

**Coming Next in Part 10:**  
- **JavaScript DOM Manipulation**  
- **Creating, Modifying, and Removing Elements Dynamically**  
- **Traversing the DOM (Parent, Children, Siblings)**  

Would you like a real-world project example for event handling? 😊