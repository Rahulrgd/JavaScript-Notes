# **JavaScript Notes – Part 26: WebRTC Screen Sharing & Optimizing Group Calls**  

In this part, we will cover:  
✔ **Implementing Screen Sharing in WebRTC**  
✔ **Allowing users to toggle between webcam and screen sharing**  
✔ **Optimizing WebRTC for large group calls using SFU (Selective Forwarding Unit)**  

---

## **1. Implementing Screen Sharing in WebRTC**  

WebRTC allows users to **share their screen** using the `getDisplayMedia()` API.  
This enables **presentations, live coding, or game streaming**.  

---

### **1.1 Capturing Screen Video**  

```js
async function startScreenShare() {
    try {
        const screenStream = await navigator.mediaDevices.getDisplayMedia({ video: true });
        replaceVideoTrack(screenStream);
    } catch (error) {
        console.error("Error accessing screen:", error);
    }
}
```
🔹 `getDisplayMedia({ video: true })` → Captures the screen  
🔹 `replaceVideoTrack(screenStream)` → Sends the screen video to peers  

---

### **1.2 Replacing Video Track in WebRTC**  

Once the screen stream is captured, we **replace** the existing webcam video track.  

```js
function replaceVideoTrack(newStream) {
    const videoTrack = newStream.getVideoTracks()[0];

    Object.values(peerConnections).forEach(peerConnection => {
        const sender = peerConnection.getSenders().find(s => s.track.kind === "video");
        if (sender) sender.replaceTrack(videoTrack);
    });

    document.getElementById("localVideo").srcObject = newStream; // Update local preview
}
```
🔹 **Finds all active WebRTC connections and replaces the webcam video**  

---

### **1.3 Stopping Screen Share & Returning to Webcam**  

When screen sharing stops, switch back to the **webcam**.  

```js
async function stopScreenShare() {
    localStream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
    replaceVideoTrack(localStream);
}
```

---

### **1.4 Adding UI Buttons for Screen Share**  

#### **HTML**
```html
<button onclick="startScreenShare()">Start Screen Share</button>
<button onclick="stopScreenShare()">Stop Screen Share</button>
```
🔹 Clicking **"Start Screen Share"** switches from the webcam to the screen.  
🔹 Clicking **"Stop Screen Share"** switches back to the webcam.  

---

## **2. Optimizing WebRTC for Large Group Calls using SFU**  

In **large calls**, the **mesh network** (each peer connecting to every other peer) becomes inefficient.  
A **Selective Forwarding Unit (SFU)** is a server that:  
✔ Receives media streams from participants  
✔ **Forwards only the necessary streams** to each participant  

### **2.1 How SFU Improves Performance**  

| Approach | Description | Bandwidth Usage | Best Use Case |
|----------|------------|----------------|---------------|
| **Mesh (P2P)** | Each peer connects to every other peer | Very high | Small groups (3-5 people) |
| **SFU (Selective Forwarding Unit)** | Server relays streams between peers | Medium | Medium/Large groups (5-50 people) |
| **MCU (Multipoint Control Unit)** | Server mixes streams into a single one | Low | Large webinars (>50 people) |

### **2.2 Implementing SFU in WebRTC**  

Instead of direct **peer-to-peer connections**, we use a **server (SFU)**.  
This requires **WebRTC + WebSocket signaling** to manage streams.  

#### **Server-Side: Node.js + WebSocket**
```js
const io = require("socket.io")(server);

io.on("connection", (socket) => {
    socket.on("offer", (data) => {
        socket.to(data.peerId).emit("offer", data);
    });

    socket.on("answer", (data) => {
        socket.to(data.peerId).emit("answer", data);
    });

    socket.on("candidate", (data) => {
        socket.to(data.peerId).emit("candidate", data);
    });
});
```
🔹 **Handles WebRTC signaling (offers, answers, ICE candidates)**  

---

#### **Client-Side: Sending Stream to SFU**
```js
const peerConnection = new RTCPeerConnection();

navigator.mediaDevices.getUserMedia({ video: true, audio: true }).then(stream => {
    stream.getTracks().forEach(track => peerConnection.addTrack(track, stream));
});
```
🔹 **The client sends the stream to the SFU, and the SFU forwards it to others.**  

---

## **3. Summary of Part 26**  

✔ **Implemented screen sharing using WebRTC**  
✔ **Allowed switching between screen and webcam**  
✔ **Explored SFU for optimizing group calls**  

---

### **Coming Next in Part 27:**  
- **Using WebRTC for Live Streaming (Broadcast Mode) 🚀**  
- **Building a YouTube-style WebRTC Live Stream App**  

Would you like any additional enhancements? 🚀