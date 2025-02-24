# JavaScript Notes – Part 18: AJAX & Fetch API  

In this part, we will cover:  
- What is AJAX?  
- Introduction to the Fetch API  
- Making GET and POST requests  
- Handling different response types (JSON, text, blob, etc.)  
- Error handling in Fetch  
- Practical API examples  

---

## 1. What is AJAX?  

**AJAX (Asynchronous JavaScript and XML)** is a technique for updating parts of a webpage **without reloading** the entire page. It allows JavaScript to communicate with a server **asynchronously**.  

🔹 **Before AJAX (Synchronous Request Example)**  
```html
<a href="user-data.html">Get User Data</a>
```
- Clicking this **reloads the page** to get new data.  

🔹 **With AJAX (Asynchronous Request Example)**  
```js
fetch("https://jsonplaceholder.typicode.com/users/1")
    .then(response => response.json())
    .then(data => console.log(data));
```
- The page **does not reload**; it updates dynamically.  

---

## 2. Introduction to Fetch API  

The **Fetch API** is a modern way to make HTTP requests in JavaScript. It is **promise-based** and replaces older techniques like `XMLHttpRequest`.  

🔹 **Basic Syntax:**  
```js
fetch(url)
    .then(response => response.json()) // Convert response to JSON
    .then(data => console.log(data))   // Process the data
    .catch(error => console.error("Error:", error)); // Handle errors
```

---

## 3. Making GET Requests  

### **3.1 Fetching JSON Data**  
```js
fetch("https://jsonplaceholder.typicode.com/posts/1")
    .then(response => response.json())
    .then(data => console.log("Post Title:", data.title))
    .catch(error => console.error("Error fetching post:", error));
```
- `response.json()` converts the response into a **JavaScript object**.  

### **3.2 Handling Different Response Types**  
```js
fetch("https://example.com/data.txt") // Fetch plain text
    .then(response => response.text())
    .then(text => console.log("Text Data:", text));

fetch("https://example.com/image.jpg") // Fetch image as Blob
    .then(response => response.blob())
    .then(blob => console.log("Image Blob:", blob));
```
- `.text()` → Gets raw text response  
- `.blob()` → Gets binary data (images, files)  

---

## 4. Making POST Requests (Sending Data)  

We use **POST** when sending data to a server.  

### **4.1 Sending JSON Data**  
```js
fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify({
        title: "New Post",
        body: "This is the post content",
        userId: 1
    })
})
.then(response => response.json())
.then(data => console.log("Created Post:", data))
.catch(error => console.error("Error:", error));
```
- `method: "POST"` → Defines HTTP method.  
- `headers: { "Content-Type": "application/json" }` → Tells the server we're sending JSON.  
- `body: JSON.stringify(data)` → Converts object to JSON before sending.  

---

## 5. Handling Errors in Fetch  

Fetch **does not reject the promise** for HTTP errors (like `404` or `500`).  
We must manually check `response.ok`.  

```js
fetch("https://jsonplaceholder.typicode.com/invalid-url")
    .then(response => {
        if (!response.ok) throw new Error(`HTTP Error! Status: ${response.status}`);
        return response.json();
    })
    .then(data => console.log(data))
    .catch(error => console.error("Fetch Error:", error));
```
- If `response.ok` is `false`, we manually throw an error.  

---

## 6. Using `async/await` with Fetch  

Instead of `.then()`, we can use **async/await** for cleaner code.  

```js
async function getPost() {
    try {
        let response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
        if (!response.ok) throw new Error("Failed to fetch post!");
        
        let post = await response.json();
        console.log("Post Title:", post.title);
    } catch (error) {
        console.error("Error:", error.message);
    }
}

getPost();
```
- `await fetch(url)` waits for the response.  
- `await response.json()` converts the response to JSON.  
- `try...catch` handles errors.  

---

## 7. Practical API Example  

### **7.1 Fetching and Displaying Data in the DOM**  
```html
<button id="loadUser">Load User</button>
<div id="userInfo"></div>

<script>
document.getElementById("loadUser").addEventListener("click", async () => {
    try {
        let response = await fetch("https://jsonplaceholder.typicode.com/users/1");
        if (!response.ok) throw new Error("Failed to fetch user!");
        
        let user = await response.json();
        document.getElementById("userInfo").innerHTML = `
            <h3>${user.name}</h3>
            <p>Email: ${user.email}</p>
            <p>Phone: ${user.phone}</p>
        `;
    } catch (error) {
        console.error("Error:", error.message);
    }
});
</script>
```
- Fetches user data **on button click**.  
- Displays the user’s **name, email, and phone** in the DOM.  

---

## Summary of Part 18:
✔ Understanding AJAX & Fetch API  
✔ Making GET and POST requests  
✔ Handling different response types  
✔ Proper error handling with `response.ok`  
✔ Using `async/await` with Fetch  
✔ A practical example of fetching and displaying data  

---

### **Coming Next in Part 19:**  
- **Working with APIs (REST & GraphQL)**  
- **Making Requests to Public APIs**  
- **Using Fetch with Authentication (Tokens & Headers)**  

Would you like a hands-on challenge to practice Fetch API before moving forward? 😊