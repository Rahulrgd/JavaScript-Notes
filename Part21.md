# JavaScript Notes – Part 21: Server-Side WebSockets (Node.js + WebSocket Server)  

In this part, we will cover:  
- Setting up a WebSocket server with Node.js  
- Handling multiple clients  
- Broadcasting messages to all clients  
- Creating WebSocket rooms (private chat)  

---

## 1. Setting Up a WebSocket Server with Node.js  

To create a WebSocket server, we need **Node.js** and the `ws` library.  

### **1.1 Install WebSocket Library**  
Run the following command in your project directory:  
```sh
npm init -y   # Initialize a Node.js project
npm install ws  # Install WebSocket package
```

### **1.2 Creating a Basic WebSocket Server**  

```js
const WebSocket = require("ws");

// Create WebSocket server on port 8080
const wss = new WebSocket.Server({ port: 8080 });

wss.on("connection", (ws) => {
    console.log("New client connected");

    // Handle incoming messages
    ws.on("message", (message) => {
        console.log(`Received: ${message}`);
        ws.send(`Server received: ${message}`);
    });

    // Handle client disconnect
    ws.on("close", () => {
        console.log("Client disconnected");
    });
});

console.log("WebSocket server running on ws://localhost:8080");
```
🔹 **Explanation:**  
- The WebSocket server listens on `ws://localhost:8080`.  
- It responds to messages sent by connected clients.  
- It detects when a client disconnects.  

---

## 2. Connecting Clients to the WebSocket Server  

Now, let's create a simple client using JavaScript in the browser.  

### **2.1 Connecting to the WebSocket Server**  
```js
const socket = new WebSocket("ws://localhost:8080");

socket.addEventListener("open", () => {
    console.log("Connected to WebSocket server");
    socket.send("Hello, Server!");
});

socket.addEventListener("message", (event) => {
    console.log("Message from server:", event.data);
});

socket.addEventListener("close", () => {
    console.log("Disconnected from server");
});
```
- The client connects to `ws://localhost:8080` and sends a message.  
- It listens for responses from the server.  

---

## 3. Handling Multiple Clients  

When multiple clients connect, we may need to **broadcast** messages to all of them.  

### **3.1 Broadcasting Messages to All Clients**  
Modify the WebSocket server to send messages to all connected clients:  
```js
wss.on("connection", (ws) => {
    console.log("New client connected");

    ws.on("message", (message) => {
        console.log(`Received: ${message}`);

        // Broadcast to all clients
        wss.clients.forEach((client) => {
            if (client.readyState === WebSocket.OPEN) {
                client.send(`Broadcast: ${message}`);
            }
        });
    });

    ws.on("close", () => {
        console.log("Client disconnected");
    });
});
```
🔹 **Now, when one client sends a message, all connected clients receive it.**  

---

## 4. Creating WebSocket Rooms (Private Chat)  

In chat applications, we may need **chat rooms** where only specific users receive messages.  

### **4.1 Managing Rooms**  
```js
const rooms = {};  // Object to store rooms

wss.on("connection", (ws) => {
    let currentRoom = null;

    ws.on("message", (message) => {
        let data = JSON.parse(message);

        if (data.type === "join") {
            currentRoom = data.room;
            if (!rooms[currentRoom]) rooms[currentRoom] = new Set();
            rooms[currentRoom].add(ws);
            console.log(`Client joined room: ${currentRoom}`);
        } else if (data.type === "message") {
            if (currentRoom) {
                rooms[currentRoom].forEach((client) => {
                    if (client !== ws && client.readyState === WebSocket.OPEN) {
                        client.send(JSON.stringify({ room: currentRoom, message: data.message }));
                    }
                });
            }
        }
    });

    ws.on("close", () => {
        if (currentRoom) rooms[currentRoom].delete(ws);
        console.log("Client disconnected");
    });
});
```

🔹 **How it works:**  
1. Clients send `{ type: "join", room: "room1" }` to join a room.  
2. Messages sent to the room `{ type: "message", message: "Hello" }` are only sent to members of that room.  

---

## 5. Client Code for Room-based Chat  

Modify the client-side JavaScript to join a room and send messages:  

```js
const socket = new WebSocket("ws://localhost:8080");

socket.addEventListener("open", () => {
    console.log("Connected to server");

    // Join a room
    socket.send(JSON.stringify({ type: "join", room: "room1" }));
});

document.getElementById("sendMessage").addEventListener("click", () => {
    let message = document.getElementById("messageInput").value;
    socket.send(JSON.stringify({ type: "message", message }));
});

socket.addEventListener("message", (event) => {
    let data = JSON.parse(event.data);
    console.log(`Room: ${data.room}, Message: ${data.message}`);
});
```
- This client joins `"room1"` and can chat only with users in that room.  

---

## Summary of Part 21:
✔ Setting up a WebSocket server with Node.js  
✔ Handling multiple WebSocket clients  
✔ Broadcasting messages to all clients  
✔ Implementing WebSocket rooms for private chat  

---

### **Coming Next in Part 22:**  
- **WebRTC for Peer-to-Peer Communication**  
- **Building a Simple Video Chat App**  

Would you like to enhance the chat app before moving forward? 🚀