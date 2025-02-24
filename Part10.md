# JavaScript Notes – Part 10: DOM Manipulation  

In this part, we will cover **Document Object Model (DOM) Manipulation in JavaScript**, including:  
- Selecting elements  
- Modifying elements  
- Creating and removing elements  
- Traversing the DOM  

---

## 1. Introduction to DOM  

The **Document Object Model (DOM)** is a **tree structure** that represents HTML elements. JavaScript allows us to interact with and modify the DOM dynamically.  

Example HTML:  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>DOM Example</title>
</head>
<body>
    <h1 id="heading">Hello, World!</h1>
    <p class="text">This is a paragraph.</p>
    <ul id="list">
        <li>Item 1</li>
        <li>Item 2</li>
    </ul>
    <button id="btn">Click Me</button>

    <script src="script.js"></script>
</body>
</html>
```
---

## 2. Selecting Elements  

### **2.1 Using `getElementById`**  
```js
const heading = document.getElementById("heading");
console.log(heading.textContent); // "Hello, World!"
```

### **2.2 Using `getElementsByClassName` (Returns HTMLCollection)**  
```js
const paragraphs = document.getElementsByClassName("text");
console.log(paragraphs[0].textContent); // "This is a paragraph."
```

### **2.3 Using `getElementsByTagName` (Returns HTMLCollection)**  
```js
const listItems = document.getElementsByTagName("li");
console.log(listItems.length); // 2
```

### **2.4 Using `querySelector` (First Matching Element)**  
```js
const firstItem = document.querySelector("#list li");
console.log(firstItem.textContent); // "Item 1"
```

### **2.5 Using `querySelectorAll` (Returns NodeList)**  
```js
const allItems = document.querySelectorAll("#list li");
console.log(allItems.length); // 2
```

---

## 3. Modifying Elements  

### **3.1 Changing Text Content**  
```js
heading.textContent = "Welcome to JavaScript!";
```

### **3.2 Changing HTML Content**  
```js
document.getElementById("list").innerHTML = "<li>New Item</li>";
```

### **3.3 Modifying Attributes**  
```js
document.getElementById("btn").setAttribute("disabled", "true");
```

### **3.4 Modifying Styles**  
```js
document.getElementById("heading").style.color = "blue";
```

---

## 4. Creating and Removing Elements  

### **4.1 Creating Elements**  
```js
const newItem = document.createElement("li");
newItem.textContent = "Item 3";
document.getElementById("list").appendChild(newItem);
```

### **4.2 Removing Elements**  
```js
const list = document.getElementById("list");
list.removeChild(list.firstElementChild); // Removes first <li>
```

### **4.3 Replacing Elements**  
```js
const newHeading = document.createElement("h1");
newHeading.textContent = "New Heading!";
document.body.replaceChild(newHeading, heading);
```

---

## 5. Traversing the DOM  

### **5.1 Accessing Parent Element**  
```js
console.log(document.getElementById("list").parentElement);
```

### **5.2 Accessing Children Elements**  
```js
console.log(document.getElementById("list").children);
```

### **5.3 Accessing First and Last Child**  
```js
console.log(document.getElementById("list").firstElementChild.textContent);
console.log(document.getElementById("list").lastElementChild.textContent);
```

### **5.4 Accessing Siblings**  
```js
const item2 = document.querySelector("#list li:nth-child(2)");
console.log(item2.previousElementSibling.textContent); // "Item 1"
console.log(item2.nextElementSibling); // null (no next sibling)
```

---

## Summary of Part 10:
✔ Selecting elements (`getElementById`, `querySelector`)  
✔ Modifying elements (text, attributes, styles)  
✔ Creating, removing, and replacing elements  
✔ Traversing the DOM (parent, child, sibling elements)  

---

**Coming Next in Part 11:**  
- **Handling Forms & User Input**  
- **Form Validation in JavaScript**  
- **Local Storage & Session Storage**  

Would you like an interactive example using event listeners with DOM manipulation? 😊