# **JavaScript Notes – Part 25: WebRTC Multi-Peer Communication & Group Calls**  

In this part, we will cover:  
✔ **Understanding Multi-Peer WebRTC**  
✔ **Creating a WebRTC Mesh Network** for group calls  
✔ **Managing multiple peer connections**  
✔ **Handling peer disconnections**  

---

## **1. Understanding Multi-Peer WebRTC (Group Calls)**  

WebRTC supports **one-to-one** video calls by default.  
For **group calls**, we need a way to **connect multiple peers**:  

### **Approaches for Multi-Peer WebRTC**
| Approach | Description | Pros | Cons |
|----------|------------|------|------|
| **Mesh Network** | Each peer connects to every other peer | No central server, P2P communication | High bandwidth usage |
| **SFU (Selective Forwarding Unit)** | A central server relays media streams | Efficient bandwidth usage | Requires a media server |
| **MCU (Multipoint Control Unit)** | A server processes and mixes media streams | Less processing on clients | High server cost |

---

## **2. Implementing a WebRTC Mesh Network for Group Calls**  

In a **mesh network**, each participant connects to **every other participant** directly.  

### **2.1 Managing Multiple Peer Connections**  

Each participant maintains a **list of peer connections**:  
```js
const peerConnections = {}; // Stores connections for each participant
```

---

### **2.2 Connecting New Peers**  
When a new user joins, they establish **WebRTC connections** with all existing participants.  

```js
function addPeer(peerId, stream) {
    const peerConnection = new RTCPeerConnection();

    peerConnection.onicecandidate = (event) => {
        if (event.candidate) {
            sendSignal(peerId, { type: "candidate", candidate: event.candidate });
        }
    };

    peerConnection.ontrack = (event) => {
        displayStream(peerId, event.streams[0]);
    };

    stream.getTracks().forEach(track => peerConnection.addTrack(track, stream));

    peerConnections[peerId] = peerConnection;
}
```
🔹 **Steps Explained:**  
1. **Create a new RTCPeerConnection**  
2. **Send ICE candidates** (for NAT traversal)  
3. **Attach remote video stream** when received  
4. **Add local video/audio tracks**  

---

### **2.3 Sending and Receiving Signaling Messages**  
To **exchange connection details**, we use a signaling server (like WebSocket).  

#### **2.3.1 Sending a Signal**  
```js
function sendSignal(peerId, message) {
    socket.emit("signal", { peerId, message });
}
```

#### **2.3.2 Receiving a Signal**
```js
socket.on("signal", (data) => {
    const { peerId, message } = data;
    
    if (message.type === "offer") {
        handleOffer(peerId, message);
    } else if (message.type === "answer") {
        handleAnswer(peerId, message);
    } else if (message.type === "candidate") {
        handleCandidate(peerId, message);
    }
});
```
🔹 **Signaling is required** to establish WebRTC connections.  

---

### **2.4 Handling Connection Requests**  

#### **2.4.1 Handling Offers**
```js
async function handleOffer(peerId, offer) {
    const peerConnection = addPeer(peerId, localStream);
    
    await peerConnection.setRemoteDescription(new RTCSessionDescription(offer));
    const answer = await peerConnection.createAnswer();
    await peerConnection.setLocalDescription(answer);
    
    sendSignal(peerId, { type: "answer", answer });
}
```

#### **2.4.2 Handling Answers**
```js
async function handleAnswer(peerId, answer) {
    await peerConnections[peerId].setRemoteDescription(new RTCSessionDescription(answer));
}
```

#### **2.4.3 Handling ICE Candidates**
```js
async function handleCandidate(peerId, message) {
    if (message.candidate) {
        await peerConnections[peerId].addIceCandidate(new RTCIceCandidate(message.candidate));
    }
}
```
🔹 **These functions establish connections** between peers dynamically.  

---

## **3. Displaying Video Streams from Multiple Peers**  

Each peer needs a **video element** to display the stream.  

### **3.1 HTML Structure**
```html
<div id="videos"></div>
```

### **3.2 Function to Add Video Elements**
```js
function displayStream(peerId, stream) {
    let video = document.createElement("video");
    video.id = `video-${peerId}`;
    video.srcObject = stream;
    video.autoplay = true;
    document.getElementById("videos").appendChild(video);
}
```
🔹 **Creates a new `<video>` element for each connected peer.**  

---

## **4. Handling Peer Disconnections**  

When a peer leaves, we need to:  
✔ **Remove the peer connection**  
✔ **Remove the video element**  

### **4.1 Detect Peer Disconnection**
```js
socket.on("peer-left", (peerId) => {
    if (peerConnections[peerId]) {
        peerConnections[peerId].close();
        delete peerConnections[peerId];
    }

    const videoElement = document.getElementById(`video-${peerId}`);
    if (videoElement) videoElement.remove();
});
```
🔹 **Removes the disconnected user's video stream.**  

---

## **5. Summary of Part 25**  

✔ **Multi-peer WebRTC (Group Calls)** using a **mesh network**  
✔ **Handling multiple peer connections dynamically**  
✔ **Sending and receiving signaling messages**  
✔ **Managing peer disconnections**  

---

### **Coming Next in Part 26:**  
- **WebRTC Screen Sharing in Group Calls**  
- **Implementing SFU for better performance**  

Would you like any modifications or additional features? 🚀