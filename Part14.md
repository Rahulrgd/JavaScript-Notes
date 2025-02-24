# JavaScript Notes – Part 14: Events & Event Listeners  

In this part, we will cover:  
- What are JavaScript Events?  
- Adding Event Listeners (`click`, `input`, `keydown`, etc.)  
- Event Bubbling & Capturing  
- Event Delegation  
- Removing Event Listeners  

---

## 1. What are JavaScript Events?  

JavaScript **events** occur when the user interacts with a web page.  
Examples of events:  
- Clicking a button (`click`)  
- Typing in an input field (`input`, `keydown`)  
- Moving the mouse (`mouseover`)  
- Submitting a form (`submit`)  

---

## 2. Adding Event Listeners  

### **2.1 `onclick` Attribute (Not Recommended)**  
```html
<button onclick="showMessage()">Click Me</button>

<script>
    function showMessage() {
        alert("Button Clicked!");
    }
</script>
```
🔴 **Why Not Recommended?**  
- Mixes HTML and JavaScript  
- Cannot add multiple handlers  

---

### **2.2 Using `addEventListener()` (Recommended)**  
```html
<button id="btn">Click Me</button>

<script>
    document.getElementById("btn").addEventListener("click", function () {
        alert("Button Clicked!");
    });
</script>
```
✅ **Advantages:**  
- Keeps JavaScript separate from HTML  
- Can add multiple event listeners  

---

### **2.3 Handling Input Events**  
```html
<input type="text" id="nameInput" placeholder="Type something">
<p id="output"></p>

<script>
    document.getElementById("nameInput").addEventListener("input", function () {
        document.getElementById("output").textContent = "You typed: " + this.value;
    });
</script>
```
- The `input` event fires whenever text changes  
- `this.value` gets the input value  

---

### **2.4 Handling Keyboard Events (`keydown`, `keyup`)**  
```html
<input type="text" id="textInput" placeholder="Type here...">

<script>
    document.getElementById("textInput").addEventListener("keydown", function (event) {
        console.log("Key Pressed:", event.key);
    });
</script>
```
- `keydown` fires when a key is **pressed**  
- `keyup` fires when a key is **released**  

---

## 3. Event Bubbling & Capturing  

### **3.1 What is Event Bubbling?**  
- When an event on a child element **bubbles up** to its parent.  
- Example: Clicking a button inside a `<div>` also triggers the `<div>`'s click event.  

```html
<div id="parent">
    <button id="child">Click Me</button>
</div>

<script>
    document.getElementById("parent").addEventListener("click", function () {
        alert("Parent Clicked!");
    });

    document.getElementById("child").addEventListener("click", function (event) {
        alert("Child Clicked!");
        event.stopPropagation(); // Prevents bubbling
    });
</script>
```
- `event.stopPropagation()` stops the event from bubbling.  

---

### **3.2 Event Capturing (Trickling)**  
Capturing occurs **before** bubbling (from parent to child).  
```js
document.getElementById("parent").addEventListener("click", function () {
    alert("Parent Capturing!");
}, true); // `true` enables capturing phase
```
- The event is handled **first** in the parent, then in the child.  

---

## 4. Event Delegation  

### **Why Use Event Delegation?**  
If you have **many child elements**, adding separate event listeners is inefficient.  
Instead, attach a single event listener to a **parent** and use **event delegation**.  

```html
<ul id="list">
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>

<script>
    document.getElementById("list").addEventListener("click", function (event) {
        if (event.target.tagName === "LI") {
            alert("You clicked: " + event.target.textContent);
        }
    });
</script>
```
- The event listener is **only on `<ul>`**, not on each `<li>`.  
- `event.target` gives the **actual clicked element**.  

---

## 5. Removing Event Listeners  

To remove an event listener, use `.removeEventListener()`.  

```html
<button id="start">Start</button>
<button id="stop">Stop</button>

<script>
    function sayHello() {
        console.log("Hello!");
    }

    document.getElementById("start").addEventListener("click", function () {
        document.addEventListener("click", sayHello);
    });

    document.getElementById("stop").addEventListener("click", function () {
        document.removeEventListener("click", sayHello);
    });
</script>
```
- `removeEventListener()` needs the **exact** function reference.  
- Cannot remove an **anonymous** function.  

---

## Summary of Part 14:
✔ JavaScript events (`click`, `input`, `keydown`, etc.)  
✔ `addEventListener()` and `removeEventListener()`  
✔ Event Bubbling & Capturing  
✔ Event Delegation for efficiency  

---

### **Coming Next in Part 15:**  
- **DOM Manipulation (Creating, Modifying, Removing Elements)**  
- **Handling Classes and Attributes**  
- **Best Practices for Efficient DOM Manipulation**  

Would you like a mini-project using event handling before moving forward? 😊