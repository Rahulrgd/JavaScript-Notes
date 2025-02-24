# JavaScript Notes – Part 20: WebSockets & Real-time Communication  

In this part, we will cover:  
- What are WebSockets?  
- WebSockets vs. HTTP Requests  
- Creating a WebSocket connection  
- Sending and receiving messages  
- Handling WebSocket events  
- Practical Example: Building a Real-time Chat App  

---

## 1. What are WebSockets?  

**WebSockets** enable **real-time, two-way communication** between a client and a server **over a single persistent connection**.  

🔹 **Key Features of WebSockets:**  
✔ Real-time communication (low latency)  
✔ Full-duplex (client and server can send data anytime)  
✔ Uses a single, long-lived connection  

---

## 2. WebSockets vs. HTTP Requests  

| Feature          | HTTP (Fetch/AJAX) | WebSockets |
|-----------------|------------------|------------|
| Connection Type | Opens & closes per request | Persistent connection |
| Communication   | Request → Response | Bidirectional |
| Latency        | High (overhead per request) | Low (real-time updates) |
| Use Case       | Fetching data | Real-time messaging, live updates |

---

## 3. Creating a WebSocket Connection  

The **WebSocket API** is built into JavaScript.  

### **3.1 Opening a WebSocket Connection**  
```js
const socket = new WebSocket("wss://example.com/socket");

// Event: Connection opened
socket.addEventListener("open", () => {
    console.log("Connected to WebSocket server");
    socket.send("Hello Server!");
});

// Event: Receiving a message
socket.addEventListener("message", (event) => {
    console.log("Message from server:", event.data);
});

// Event: Connection closed
socket.addEventListener("close", () => {
    console.log("WebSocket connection closed");
});

// Event: Error handling
socket.addEventListener("error", (error) => {
    console.error("WebSocket error:", error);
});
```
- `new WebSocket(url)` → Creates a connection.  
- `.send(message)` → Sends data to the server.  
- `.onmessage` → Listens for messages from the server.  
- `.onclose` → Detects when the connection closes.  
- `.onerror` → Handles errors.  

---

## 4. Sending & Receiving Data  

### **4.1 Sending Messages**  
```js
socket.send("Hello from Client!");
```

### **4.2 Receiving Messages**  
```js
socket.onmessage = (event) => {
    console.log("Received:", event.data);
};
```

### **4.3 Sending JSON Data**  
```js
socket.send(JSON.stringify({ type: "chat", message: "Hello!" }));
```

### **4.4 Receiving JSON Data**  
```js
socket.onmessage = (event) => {
    let data = JSON.parse(event.data);
    console.log("Received Message:", data.message);
};
```
- **Why JSON?** JSON allows structured data exchange (e.g., chat messages, user actions).  

---

## 5. Handling WebSocket Events  

### **5.1 Detecting Connection Open/Close**  
```js
socket.onopen = () => console.log("Connected");
socket.onclose = () => console.log("Disconnected");
```

### **5.2 Handling Errors**  
```js
socket.onerror = (error) => console.error("WebSocket Error:", error);
```

---

## 6. Practical Example: Real-time Chat App  

### **6.1 HTML Structure**  
```html
<input type="text" id="messageInput" placeholder="Type a message">
<button id="sendMessage">Send</button>
<div id="chatBox"></div>
<script src="chat.js"></script>
```

### **6.2 JavaScript (chat.js)**  
```js
const socket = new WebSocket("wss://example.com/chat");

socket.addEventListener("open", () => {
    console.log("Connected to chat server");
});

document.getElementById("sendMessage").addEventListener("click", () => {
    let message = document.getElementById("messageInput").value;
    socket.send(JSON.stringify({ message }));
});

socket.addEventListener("message", (event) => {
    let data = JSON.parse(event.data);
    let chatBox = document.getElementById("chatBox");
    chatBox.innerHTML += `<p>${data.message}</p>`;
});
```
- **When the user sends a message**, it is sent over WebSockets.  
- **Incoming messages** are displayed in the chat box.  

---

## Summary of Part 20:
✔ Understanding WebSockets & Real-time Communication  
✔ Creating a WebSocket Connection  
✔ Sending & Receiving Messages  
✔ Handling WebSocket Events  
✔ Building a Basic Real-time Chat App  

---

### **Coming Next in Part 21:**  
- **Server-Side WebSockets (Node.js + WebSocket Server)**  
- **Handling Multiple Clients in WebSockets**  
- **Building a More Advanced Chat App with WebSocket Rooms**  

Would you like to expand the chat app with more features before moving forward? 😊