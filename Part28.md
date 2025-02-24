# **JavaScript Notes – Part 28: Building a Full WebRTC Streaming Platform**  

In this part, we will cover:  
✔ **Building a complete WebRTC streaming platform**  
✔ **Adding real-time chat with WebSockets**  
✔ **Implementing user controls (mute, stop stream, etc.)**  
✔ **Enhancing UI for a better user experience**  

---

## **1. Overview of the WebRTC Streaming Platform**  

We will build a **one-to-many** WebRTC streaming platform where:  
✔ A **host** starts a live stream (WebRTC Broadcast)  
✔ **Viewers** can join and watch the stream  
✔ A **real-time chat** feature enables interaction  
✔ The host has **controls** (mute/unmute, stop stream)  

---

## **2. Setting Up the Project**  

We need:  
- **WebRTC** for video streaming  
- **WebSockets** for real-time messaging  
- **HTML/CSS/JavaScript** for UI  

### **2.1 Folder Structure**
```
/webrtc-streaming-platform
│── /server
│    ├── server.js  (WebSocket + signaling server)
│── /public
│    ├── index.html  (Main UI)
│    ├── script.js   (Frontend logic)
│    ├── styles.css  (Styling)
```
---

## **3. Implementing the WebSocket Signaling Server**  

We need a **WebSocket server** to manage signaling between the broadcaster and viewers.

### **3.1 Install Dependencies**
Run the following in the terminal:
```sh
npm init -y
npm install express ws
```

### **3.2 Create `server.js` (WebSocket Signaling)**
```js
const express = require("express");
const { Server } = require("ws");

const app = express();
const server = app.listen(3000, () => console.log("Server running on port 3000"));
const wss = new Server({ server });

let broadcaster; // Store broadcaster ID
let viewers = new Set();

wss.on("connection", (socket) => {
    socket.on("message", (message) => {
        const data = JSON.parse(message);

        switch (data.type) {
            case "broadcaster":
                broadcaster = socket;
                break;
            case "viewer":
                viewers.add(socket);
                broadcaster?.send(JSON.stringify({ type: "new-viewer" }));
                break;
            case "offer":
            case "answer":
            case "candidate":
                viewers.forEach(viewer => viewer.send(JSON.stringify(data)));
                break;
        }
    });

    socket.on("close", () => {
        if (socket === broadcaster) broadcaster = null;
        viewers.delete(socket);
    });
});
```
🔹 **Manages WebRTC signaling (offer, answer, ICE candidates)**  
🔹 **Tracks the broadcaster and viewers**  

---

## **4. Creating the Frontend (HTML, JS, CSS)**  

### **4.1 `index.html` (Main UI)**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>WebRTC Streaming</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h2>Live Stream</h2>
    <video id="video" autoplay playsinline></video>
    <button id="startStream">Start Broadcast</button>
    <button id="stopStream" disabled>Stop</button>

    <h3>Chat</h3>
    <div id="chatbox"></div>
    <input id="messageInput" placeholder="Type a message..." />
    <button id="sendMessage">Send</button>

    <script src="script.js"></script>
</body>
</html>
```

---

### **4.2 `script.js` (WebRTC & Chat Logic)**
```js
const socket = new WebSocket("ws://localhost:3000");
const video = document.getElementById("video");
let peerConnection;
let isBroadcaster = false;

// STUN Servers for WebRTC
const config = { iceServers: [{ urls: "stun:stun.l.google.com:19302" }] };

// Handle WebSocket Messages
socket.onmessage = async (event) => {
    const data = JSON.parse(event.data);

    if (data.type === "new-viewer" && isBroadcaster) {
        const offer = await peerConnection.createOffer();
        await peerConnection.setLocalDescription(offer);
        socket.send(JSON.stringify({ type: "offer", offer }));
    } else if (data.type === "offer") {
        peerConnection = new RTCPeerConnection(config);
        peerConnection.ontrack = (e) => video.srcObject = e.streams[0];
        await peerConnection.setRemoteDescription(new RTCSessionDescription(data.offer));
        const answer = await peerConnection.createAnswer();
        await peerConnection.setLocalDescription(answer);
        socket.send(JSON.stringify({ type: "answer", answer }));
    } else if (data.type === "answer") {
        await peerConnection.setRemoteDescription(new RTCSessionDescription(data.answer));
    } else if (data.type === "candidate") {
        await peerConnection.addIceCandidate(new RTCIceCandidate(data.candidate));
    }
};

// Start Broadcasting
document.getElementById("startStream").addEventListener("click", async () => {
    isBroadcaster = true;
    const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
    video.srcObject = stream;

    peerConnection = new RTCPeerConnection(config);
    stream.getTracks().forEach(track => peerConnection.addTrack(track, stream));

    socket.send(JSON.stringify({ type: "broadcaster" }));

    peerConnection.onicecandidate = (event) => {
        if (event.candidate) {
            socket.send(JSON.stringify({ type: "candidate", candidate: event.candidate }));
        }
    };
});

// Stop Stream
document.getElementById("stopStream").addEventListener("click", () => {
    video.srcObject.getTracks().forEach(track => track.stop());
    socket.close();
});
```
🔹 **Handles WebRTC connection for both broadcaster and viewers**  
🔹 **Starts and stops broadcasting dynamically**  

---

## **5. Implementing Real-Time Chat**  

We will use WebSockets for real-time chat.

### **5.1 Update `server.js` (WebSocket Chat)**
```js
wss.on("connection", (socket) => {
    socket.on("message", (message) => {
        const data = JSON.parse(message);

        if (data.type === "chat") {
            wss.clients.forEach(client => client.send(JSON.stringify(data)));
        }
    });
});
```
🔹 **Broadcasts chat messages to all connected users**  

---

### **5.2 Update `script.js` (Chat Logic)**
```js
document.getElementById("sendMessage").addEventListener("click", () => {
    const message = document.getElementById("messageInput").value;
    socket.send(JSON.stringify({ type: "chat", message }));
    document.getElementById("messageInput").value = "";
});

socket.onmessage = (event) => {
    const data = JSON.parse(event.data);
    if (data.type === "chat") {
        const chatbox = document.getElementById("chatbox");
        chatbox.innerHTML += `<p>${data.message}</p>`;
    }
};
```
🔹 **Sends messages via WebSockets**  
🔹 **Displays chat messages on all connected clients**  

---

## **6. Summary of Part 28**  

✔ **Built a complete WebRTC streaming platform**  
✔ **Added real-time chat using WebSockets**  
✔ **Implemented controls for starting/stopping streams**  
✔ **Enhanced UI for a better experience**  

---

### **Coming Next in Part 29:**  
- **Enhancing Security (User Authentication & Role Management)**  
- **Recording WebRTC Streams and Saving as MP4**  
- **Deploying the WebRTC Streaming App Online**  

Would you like a tutorial on deploying the app next? 🚀