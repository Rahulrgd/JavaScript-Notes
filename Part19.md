# JavaScript Notes – Part 19: Working with APIs (REST & GraphQL)  

In this part, we will cover:  
- What is an API?  
- REST API vs. GraphQL API  
- Making REST API requests  
- Making GraphQL API requests  
- Using Fetch with Authentication (Tokens & Headers)  
- Practical API Examples  

---

## 1. What is an API?  

An **API (Application Programming Interface)** allows applications to communicate with each other. In web development, APIs enable the frontend (JavaScript) to fetch data from a backend server.  

There are two common types of APIs:  
1. **REST API** – Uses **URLs and HTTP methods** (GET, POST, etc.).  
2. **GraphQL API** – Uses **queries** to fetch specific data.  

---

## 2. REST API vs. GraphQL API  

| Feature        | REST API | GraphQL API |
|--------------|---------|------------|
| Structure   | Multiple endpoints | Single endpoint |
| Data Fetching | Returns fixed data structure | Flexible, fetch only needed fields |
| Over-fetching | Yes (unnecessary data included) | No (only requested fields) |
| Under-fetching | Yes (may require multiple requests) | No (one request can get all data) |
| Example URL | `/users/1` | Query: `{ user(id: 1) { name, email } }` |

---

## 3. Making REST API Requests  

### **3.1 GET Request (Fetching Data)**  
```js
async function getUser() {
    let response = await fetch("https://jsonplaceholder.typicode.com/users/1");
    let user = await response.json();
    console.log(user);
}

getUser();
```
- Uses `fetch()` to request user data.  

### **3.2 POST Request (Sending Data)**  
```js
async function createPost() {
    let response = await fetch("https://jsonplaceholder.typicode.com/posts", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
            title: "New Post",
            body: "This is the content",
            userId: 1
        })
    });

    let newPost = await response.json();
    console.log(newPost);
}

createPost();
```
- Sends a new post to the server.  

### **3.3 PUT Request (Updating Data)**  
```js
async function updatePost() {
    let response = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
            id: 1,
            title: "Updated Title",
            body: "Updated Content",
            userId: 1
        })
    });

    let updatedPost = await response.json();
    console.log(updatedPost);
}

updatePost();
```
- Updates an existing post.  

### **3.4 DELETE Request (Removing Data)**  
```js
async function deletePost() {
    let response = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
        method: "DELETE"
    });

    console.log("Post Deleted:", response.ok);
}

deletePost();
```
- Deletes a post from the server.  

---

## 4. Making GraphQL API Requests  

GraphQL uses a **single endpoint** (`/graphql`) and allows us to fetch exactly what we need.  

### **4.1 Fetching Data from a GraphQL API**  
```js
async function getUserGraphQL() {
    let query = `
        {
            user(id: 1) {
                name
                email
            }
        }
    `;

    let response = await fetch("https://example.com/graphql", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ query })
    });

    let data = await response.json();
    console.log(data);
}

getUserGraphQL();
```
- Fetches only `name` and `email` of a user.  

### **4.2 Sending Data with GraphQL Mutation**  
```js
async function createPostGraphQL() {
    let mutation = `
        mutation {
            createPost(title: "GraphQL Post", body: "Content here", userId: 1) {
                id
                title
            }
        }
    `;

    let response = await fetch("https://example.com/graphql", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ query: mutation })
    });

    let data = await response.json();
    console.log(data);
}

createPostGraphQL();
```
- Sends a **GraphQL mutation** to create a new post.  

---

## 5. Using Fetch with Authentication  

Many APIs require authentication using **tokens**.  

### **5.1 Using API Key Authentication**  
```js
async function getData() {
    let response = await fetch("https://api.example.com/data", {
        headers: {
            "Authorization": "API_KEY_HERE"
        }
    });

    let data = await response.json();
    console.log(data);
}

getData();
```
- Passes an **API key** in the `Authorization` header.  

### **5.2 Using Bearer Token Authentication**  
```js
async function getUserProfile() {
    let token = "YOUR_ACCESS_TOKEN";
    let response = await fetch("https://api.example.com/profile", {
        headers: {
            "Authorization": `Bearer ${token}`
        }
    });

    let profile = await response.json();
    console.log(profile);
}

getUserProfile();
```
- Uses an **OAuth Bearer Token** for authentication.  

---

## 6. Practical Example: Fetching and Displaying GitHub User Info  

```html
<input type="text" id="username" placeholder="Enter GitHub username">
<button id="fetchUser">Get User</button>
<div id="userInfo"></div>

<script>
document.getElementById("fetchUser").addEventListener("click", async () => {
    let username = document.getElementById("username").value;
    
    try {
        let response = await fetch(`https://api.github.com/users/${username}`);
        if (!response.ok) throw new Error("User not found");

        let user = await response.json();
        document.getElementById("userInfo").innerHTML = `
            <h3>${user.name}</h3>
            <p>Username: ${user.login}</p>
            <p>Public Repos: ${user.public_repos}</p>
            <img src="${user.avatar_url}" width="100">
        `;
    } catch (error) {
        console.error("Error:", error.message);
    }
});
</script>
```
- Fetches **GitHub user details** by entering a username.  
- Displays **name, repositories, and profile picture**.  

---

## Summary of Part 19:
✔ REST API vs. GraphQL API  
✔ Making GET, POST, PUT, DELETE requests with Fetch  
✔ Fetching data from a GraphQL API  
✔ Using authentication tokens in API requests  
✔ A practical example with GitHub API  

---

### **Coming Next in Part 20:**  
- **WebSockets and Real-time Communication**  
- **Using WebSockets with JavaScript**  
- **Practical Real-time App Example**  

Would you like a small project using API data before moving forward? 😊