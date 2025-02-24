# JavaScript Notes – Part 15: DOM Manipulation  

In this part, we will cover:  
- Selecting elements (`getElementById`, `querySelector`, etc.)  
- Modifying content and attributes  
- Creating and removing elements dynamically  
- Handling CSS classes  
- Best practices for efficient DOM manipulation  

---

## 1. Selecting Elements  

JavaScript provides multiple ways to select elements from the DOM.  

### **1.1 `getElementById()` (Single Element)**  
```js
let title = document.getElementById("main-title");
console.log(title.textContent);
```
- Selects an element by its `id`.  
- Returns **null** if the element doesn't exist.  

### **1.2 `getElementsByClassName()` (Collection of Elements)**  
```js
let items = document.getElementsByClassName("item");
console.log(items[0].textContent);
```
- Returns an **HTMLCollection** (like an array but lacks array methods).  

### **1.3 `getElementsByTagName()` (Collection of Elements by Tag)**  
```js
let paragraphs = document.getElementsByTagName("p");
console.log(paragraphs.length);
```
- Returns an **HTMLCollection** of elements with the specified tag.  

### **1.4 `querySelector()` (First Matching Element - Recommended)**  
```js
let firstItem = document.querySelector(".item");
console.log(firstItem.textContent);
```
- Selects **the first** matching element.  

### **1.5 `querySelectorAll()` (NodeList - Recommended)**  
```js
let allItems = document.querySelectorAll(".item");
allItems.forEach(item => console.log(item.textContent));
```
- Returns a **NodeList**, which supports array methods like `.forEach()`.  

---

## 2. Modifying Content & Attributes  

### **2.1 Changing Inner Content**  
```js
let title = document.getElementById("main-title");
title.textContent = "Updated Title";  // Changes text
title.innerHTML = "<span>Updated Title</span>"; // Allows HTML
```
- `textContent` → Changes only the text.  
- `innerHTML` → Can insert HTML (⚠️ Be cautious of security risks).  

### **2.2 Changing Attributes**  
```js
let image = document.getElementById("profile-pic");
image.setAttribute("src", "new-image.jpg");
console.log(image.getAttribute("src"));
```
- `setAttribute("attr", value)` → Changes an attribute.  
- `getAttribute("attr")` → Gets the value of an attribute.  

### **2.3 Modifying Styles**  
```js
let btn = document.getElementById("submit-btn");
btn.style.backgroundColor = "blue";
btn.style.color = "white";
```
- Use `element.style.property = "value";`  
- **CSS properties use camelCase** (`backgroundColor`, not `background-color`).  

---

## 3. Creating and Removing Elements  

### **3.1 Creating a New Element**  
```js
let newItem = document.createElement("li");
newItem.textContent = "New Item";
document.getElementById("list").appendChild(newItem);
```
- `createElement("tag")` → Creates an element.  
- `appendChild(newElement)` → Adds the element to a parent.  

### **3.2 Removing an Element**  
```js
let itemToRemove = document.querySelector(".item");
itemToRemove.remove();
```
- `.remove()` directly removes an element from the DOM.  

---

## 4. Handling CSS Classes  

### **4.1 Adding and Removing Classes**  
```js
let box = document.getElementById("box");
box.classList.add("highlight");
box.classList.remove("hidden");
```
- `classList.add("class-name")` → Adds a class.  
- `classList.remove("class-name")` → Removes a class.  

### **4.2 Toggling Classes**  
```js
let btn = document.getElementById("toggle-btn");
btn.addEventListener("click", function () {
    this.classList.toggle("active");
});
```
- `classList.toggle("class-name")` → Adds **if missing**, removes **if present**.  

---

## 5. Best Practices for DOM Manipulation  

✅ **Use `querySelector()` and `querySelectorAll()`** instead of older methods.  
✅ **Minimize direct DOM manipulation** (use document fragments when creating many elements).  
✅ **Use event delegation** instead of adding event listeners to each item.  
✅ **Cache frequently accessed elements** in variables.  

---

## Summary of Part 15:
✔ Selecting elements using different methods  
✔ Modifying text, attributes, and styles  
✔ Creating and removing elements dynamically  
✔ Handling CSS classes efficiently  

---

### **Coming Next in Part 16:**  
- **Local Storage & Session Storage**  
- **Saving and Retrieving Data in the Browser**  
- **Practical Examples of Persistent Storage**  

Would you like a hands-on task before moving forward? 😊