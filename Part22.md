# JavaScript Notes – Part 22: WebRTC for Peer-to-Peer Communication  

In this part, we will cover:  
- What is WebRTC?  
- WebRTC vs. WebSockets  
- Setting up a basic WebRTC connection  
- Capturing media streams (audio/video)  
- Sending media over WebRTC  
- Building a simple video chat application  

---

## 1. What is WebRTC?  

**WebRTC (Web Real-Time Communication)** is a technology that allows **peer-to-peer (P2P) communication** between web browsers for:  
✔ Live video and audio streaming  
✔ File sharing  
✔ Real-time data transfer  

### **How WebRTC Works**
WebRTC works in 3 main steps:  
1️⃣ **Signaling** → Establishes a connection using WebSockets or another signaling server.  
2️⃣ **Peer Connection** → Directly connects browsers for communication.  
3️⃣ **Media Stream** → Sends audio/video data between peers.  

---

## 2. WebRTC vs. WebSockets  

| Feature       | WebSockets | WebRTC |
|--------------|-----------|--------|
| Connection Type | Client-Server | Peer-to-Peer |
| Communication | Bidirectional | Bidirectional |
| Use Cases | Chat apps, live notifications | Video/audio calls, file sharing |
| Requires Server? | Yes, for communication | Yes, for signaling only |

🔹 **Key Difference**: WebRTC is **peer-to-peer**, while WebSockets require a server.  

---

## 3. Setting Up a WebRTC Connection  

To establish a WebRTC connection, we need:  
✅ A **signaling server** (WebSocket or HTTP-based)  
✅ **RTCPeerConnection** API for direct peer-to-peer connection  
✅ **MediaStream API** to capture audio/video  

---

## 4. Capturing Media Streams (Audio/Video)  

The **getUserMedia()** API allows access to the user's webcam and microphone.  

```js
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
    .then((stream) => {
        document.getElementById("localVideo").srcObject = stream;
    })
    .catch((error) => {
        console.error("Error accessing media devices:", error);
    });
```
🔹 **Explanation:**  
- `{ video: true, audio: true }` → Requests access to camera and microphone.  
- The video stream is assigned to a `<video>` element.  

### **4.1 HTML for Video Display**
```html
<video id="localVideo" autoplay playsinline></video>
```
- `autoplay` → Automatically plays the video.  
- `playsinline` → Ensures playback inside the browser (useful for mobile).  

---

## 5. Creating a WebRTC Peer Connection  

A **RTCPeerConnection** object allows two users to communicate directly.  

### **5.1 Setting Up a Peer Connection**
```js
const peerConnection = new RTCPeerConnection();
```
- The **peer connection** helps exchange media streams.  

### **5.2 Adding a Local Media Stream**
```js
navigator.mediaDevices.getUserMedia({ video: true, audio: true })
    .then((stream) => {
        document.getElementById("localVideo").srcObject = stream;
        stream.getTracks().forEach(track => peerConnection.addTrack(track, stream));
    });
```
🔹 **Explanation:**  
- `getTracks()` → Gets video/audio tracks from the stream.  
- `addTrack()` → Adds tracks to the peer connection.  

---

## 6. Exchanging Signaling Messages  

Since WebRTC is **peer-to-peer**, we need a **signaling server** (using WebSockets) to exchange connection details.  

### **6.1 Creating an Offer (Caller)**
```js
peerConnection.createOffer()
    .then((offer) => peerConnection.setLocalDescription(offer))
    .then(() => {
        sendSignalingMessage({ type: "offer", sdp: peerConnection.localDescription });
    });
```
🔹 **Explanation:**  
- `createOffer()` → Generates an offer for connection.  
- `setLocalDescription()` → Stores the offer.  
- `sendSignalingMessage()` → Sends the offer to the other peer (via WebSocket).  

---

### **6.2 Handling the Offer (Receiver)**
```js
peerConnection.setRemoteDescription(new RTCSessionDescription(receivedOffer));
peerConnection.createAnswer()
    .then((answer) => peerConnection.setLocalDescription(answer))
    .then(() => {
        sendSignalingMessage({ type: "answer", sdp: peerConnection.localDescription });
    });
```
🔹 **Explanation:**  
- `setRemoteDescription()` → Stores the received offer.  
- `createAnswer()` → Generates a response.  
- `setLocalDescription()` → Saves the answer.  

---

### **6.3 Handling the Answer (Caller)**
```js
peerConnection.setRemoteDescription(new RTCSessionDescription(receivedAnswer));
```
🔹 **This finalizes the connection between the two peers.**  

---

## 7. Handling ICE Candidates (Network Info)  

WebRTC requires **ICE candidates** (IP and network details) to establish a direct connection.  

### **7.1 Sending ICE Candidates**
```js
peerConnection.onicecandidate = (event) => {
    if (event.candidate) {
        sendSignalingMessage({ type: "candidate", candidate: event.candidate });
    }
};
```
### **7.2 Receiving ICE Candidates**
```js
peerConnection.addIceCandidate(new RTCIceCandidate(receivedCandidate));
```
🔹 **ICE Candidates help find the best route for the connection.**  

---

## 8. Displaying Remote Video  

Once a peer connection is established, we need to display the remote video.  

```js
peerConnection.ontrack = (event) => {
    document.getElementById("remoteVideo").srcObject = event.streams[0];
};
```

### **8.1 HTML for Remote Video**
```html
<video id="remoteVideo" autoplay playsinline></video>
```
🔹 This displays the remote user's video stream.  

---

## 9. Full WebRTC Video Chat Implementation  

### **9.1 Client Code (Browser)**
```js
const socket = new WebSocket("ws://localhost:8080");
const peerConnection = new RTCPeerConnection();

navigator.mediaDevices.getUserMedia({ video: true, audio: true })
    .then((stream) => {
        document.getElementById("localVideo").srcObject = stream;
        stream.getTracks().forEach(track => peerConnection.addTrack(track, stream));
    });

socket.onmessage = async (event) => {
    let message = JSON.parse(event.data);

    if (message.type === "offer") {
        await peerConnection.setRemoteDescription(new RTCSessionDescription(message.sdp));
        const answer = await peerConnection.createAnswer();
        await peerConnection.setLocalDescription(answer);
        socket.send(JSON.stringify({ type: "answer", sdp: answer }));
    } else if (message.type === "answer") {
        await peerConnection.setRemoteDescription(new RTCSessionDescription(message.sdp));
    } else if (message.type === "candidate") {
        await peerConnection.addIceCandidate(new RTCIceCandidate(message.candidate));
    }
};

peerConnection.onicecandidate = (event) => {
    if (event.candidate) {
        socket.send(JSON.stringify({ type: "candidate", candidate: event.candidate }));
    }
};

peerConnection.ontrack = (event) => {
    document.getElementById("remoteVideo").srcObject = event.streams[0];
};
```

---

### **9.2 Server Code (Node.js + WebSockets)**
```js
const WebSocket = require("ws");
const wss = new WebSocket.Server({ port: 8080 });

wss.on("connection", (ws) => {
    ws.on("message", (message) => {
        wss.clients.forEach((client) => {
            if (client !== ws && client.readyState === WebSocket.OPEN) {
                client.send(message);
            }
        });
    });
});
```
🔹 **The server acts as a signaling server** to relay messages between peers.  

---

## Summary of Part 22:
✔ Understanding WebRTC & Peer-to-Peer Communication  
✔ Capturing media streams (audio/video)  
✔ Exchanging signaling messages for WebRTC  
✔ Implementing a simple video chat application  

---

### **Coming Next in Part 23:**  
- **Screen Sharing with WebRTC**  
- **Recording Video Streams**  
- **Optimizing Video Call Performance**  

Would you like additional features in the video chat app? 🚀