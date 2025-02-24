# JavaScript Notes – Part 12: Fetch API & HTTP Requests  

In this part, we will cover:  
- Fetch API for making HTTP requests  
- Handling JSON responses  
- Error handling in Fetch  
- Making **GET** and **POST** requests  
- Using Fetch with async/await  

---

## 1. What is the Fetch API?  

The **Fetch API** allows us to make HTTP requests in JavaScript.  
It is a modern alternative to `XMLHttpRequest` and works with **Promises**.  

Basic syntax:  
```js
fetch(url, options)
    .then(response => response.json()) // Convert response to JSON
    .then(data => console.log(data))   // Handle JSON data
    .catch(error => console.error("Error:", error)); // Handle errors
```

---

## 2. Making a GET Request  

Let's fetch data from a public API:  
```js
fetch("https://jsonplaceholder.typicode.com/posts/1")
    .then(response => response.json()) 
    .then(data => console.log(data)) 
    .catch(error => console.error("Error:", error));
```

### **Using async/await (Recommended Way)**  
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
- `await fetch(url)` waits for the response  
- `await response.json()` converts it to JSON  
- `try...catch` handles errors  

---

## 3. Handling API Errors  

### **Checking Response Status**  
```js
async function fetchData() {
    try {
        let response = await fetch("https://jsonplaceholder.typicode.com/posts/1000"); // Invalid ID

        if (!response.ok) {
            throw new Error(`HTTP Error! Status: ${response.status}`);
        }

        let data = await response.json();
        console.log(data);
    } catch (error) {
        console.error("Error:", error.message);
    }
}

fetchData();
```
- `.ok` checks if the request was successful  
- `.status` gives the HTTP status code (e.g., 404, 500)  

---

## 4. Making a POST Request  

**POST requests** send data to the server.  

```js
async function createPost() {
    let newPost = {
        title: "New Post",
        body: "This is the content of the post.",
        userId: 1
    };

    try {
        let response = await fetch("https://jsonplaceholder.typicode.com/posts", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(newPost)
        });

        let data = await response.json();
        console.log("Created Post:", data);
    } catch (error) {
        console.error("Error:", error);
    }
}

createPost();
```
- `method: "POST"` specifies a POST request  
- `headers: { "Content-Type": "application/json" }` tells the server we're sending JSON  
- `body: JSON.stringify(newPost)` converts the object to JSON  

---

## 5. Making a DELETE Request  

```js
async function deletePost(id) {
    try {
        let response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
            method: "DELETE"
        });

        if (!response.ok) {
            throw new Error(`HTTP Error! Status: ${response.status}`);
        }

        console.log("Post deleted successfully.");
    } catch (error) {
        console.error("Error:", error);
    }
}

deletePost(1);
```

---

## 6. Fetch API vs. Axios (Comparison)  

| Feature        | Fetch API | Axios |
|---------------|----------|-------|
| Built-in      | ✅ Yes  | ❌ No (Requires installation) |
| Handles JSON  | ❌ No (Needs `.json()`) | ✅ Yes (Automatic) |
| Error Handling | ❌ Manual | ✅ Automatic |
| Supports Older Browsers | ❌ No | ✅ Yes |

---

## Summary of Part 12:
✔ Fetch API for HTTP requests  
✔ Handling API responses (JSON, errors)  
✔ Making **GET, POST, DELETE** requests  
✔ Using async/await for cleaner code  

---

### **Coming Next in Part 13:**  
- **Promises in JavaScript**  
- **Chaining Promises & Handling Errors**  
- **Promise.all, Promise.any, Promise.race**  

Would you like a hands-on mini-project using the Fetch API? 😊