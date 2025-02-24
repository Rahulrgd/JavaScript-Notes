# JavaScript Notes – Part 5: DOM Manipulation, Local Storage, and Timers  

In this part, we will cover **DOM Manipulation (Selecting, Modifying Elements, Events), Local Storage & Session Storage, and Timers (setTimeout & setInterval).**  

---

## 1. DOM (Document Object Model) Manipulation  

The **DOM (Document Object Model)** represents an HTML document as a tree structure where elements are nodes that can be accessed and modified using JavaScript.  

### 1.1 Selecting Elements  
We can select HTML elements in multiple ways:  

#### **Using `getElementById`**  
```html
<p id="myParagraph">Hello, World!</p>

<script>
    let paragraph = document.getElementById("myParagraph");
    console.log(paragraph.textContent); // Hello, World!
</script>
```

#### **Using `getElementsByClassName`** (Returns an HTMLCollection)  
```html
<p class="text">First</p>
<p class="text">Second</p>

<script>
    let elements = document.getElementsByClassName("text");
    console.log(elements[0].textContent); // First
</script>
```

#### **Using `querySelector` and `querySelectorAll`**  
```html
<p class="text">Hello</p>

<script>
    let single = document.querySelector(".text"); // Selects the first element
    let all = document.querySelectorAll(".text"); // Selects all elements
</script>
```

---

### 1.2 Modifying Elements  

#### **Changing Text Content**  
```html
<p id="message">Old Text</p>

<script>
    let msg = document.getElementById("message");
    msg.textContent = "New Text"; // Changes the text
</script>
```

#### **Changing HTML Content**  
```html
<div id="container"></div>

<script>
    document.getElementById("container").innerHTML = "<p>New Paragraph</p>";
</script>
```

#### **Changing Attributes**  
```html
<img id="myImage" src="old.jpg">

<script>
    let img = document.getElementById("myImage");
    img.src = "new.jpg"; // Changing image source
</script>
```

#### **Changing Styles (CSS)**  
```html
<p id="myText">Hello</p>

<script>
    let text = document.getElementById("myText");
    text.style.color = "blue";
    text.style.fontSize = "20px";
</script>
```

---

### 1.3 Creating and Removing Elements  

#### **Creating Elements**  
```html
<div id="container"></div>

<script>
    let newElement = document.createElement("p");
    newElement.textContent = "This is a new paragraph";

    let container = document.getElementById("container");
    container.appendChild(newElement);
</script>
```

#### **Removing Elements**  
```html
<p id="removeMe">Remove this text</p>

<script>
    let element = document.getElementById("removeMe");
    element.remove(); // Removes the element
</script>
```

---

## 2. Event Handling in JavaScript  

### **2.1 Adding Events**  

#### **Using `onclick` Attribute (Not Recommended)**  
```html
<button onclick="alert('Button Clicked!')">Click Me</button>
```

#### **Using `addEventListener` (Recommended)**  
```html
<button id="btn">Click Me</button>

<script>
    document.getElementById("btn").addEventListener("click", function() {
        alert("Button Clicked!");
    });
</script>
```

### **2.2 Event Object (`event`)**  
```html
<button id="btn">Click Me</button>

<script>
    document.getElementById("btn").addEventListener("click", function(event) {
        console.log("Button Clicked at:", event.clientX, event.clientY);
    });
</script>
```

### **2.3 Keyboard Events**  
```js
document.addEventListener("keydown", function(event) {
    console.log(`Key Pressed: ${event.key}`);
});
```

---

## 3. Local Storage and Session Storage  

### **3.1 Local Storage (Persistent Storage)**  
Data in **Local Storage** remains **even after closing the browser**.

#### **Storing Data**  
```js
localStorage.setItem("name", "Alice");
```

#### **Retrieving Data**  
```js
let name = localStorage.getItem("name");
console.log(name); // Alice
```

#### **Removing Data**  
```js
localStorage.removeItem("name");
```

#### **Clearing All Data**  
```js
localStorage.clear();
```

---

### **3.2 Session Storage (Temporary Storage)**  
Session Storage is similar to Local Storage, but **data is removed when the browser is closed**.

#### **Storing Data**  
```js
sessionStorage.setItem("sessionName", "Bob");
```

#### **Retrieving Data**  
```js
let sessionName = sessionStorage.getItem("sessionName");
console.log(sessionName); // Bob
```

#### **Removing Data**  
```js
sessionStorage.removeItem("sessionName");
```

---

## 4. Timers in JavaScript  

### **4.1 `setTimeout` (Execute Code After Delay)**  
```js
setTimeout(() => {
    console.log("Hello after 3 seconds!");
}, 3000);
```

### **4.2 `clearTimeout` (Cancel a Timeout)**  
```js
let timeoutId = setTimeout(() => {
    console.log("This will not run");
}, 3000);

clearTimeout(timeoutId); // Cancels the timeout
```

---

### **4.3 `setInterval` (Execute Code Repeatedly)**  
```js
let counter = 0;
let intervalId = setInterval(() => {
    counter++;
    console.log(`Counter: ${counter}`);
    if (counter === 5) {
        clearInterval(intervalId); // Stop after 5 iterations
    }
}, 1000);
```

---

## Summary of Part 5:
✔ Selecting & Modifying Elements (DOM Manipulation)  
✔ Event Handling (`click`, `keydown`, `event` object)  
✔ Local Storage & Session Storage  
✔ Timers (`setTimeout`, `setInterval`)  

---

**Coming Next in Part 6:**  
- **ES6 Features (Destructuring, Spread, Rest, Template Literals)**  
- **Modules in JavaScript (Import/Export)**  
- **Closures and Scope in JavaScript**  

Would you like real-world examples of DOM manipulation in the next part? 😊