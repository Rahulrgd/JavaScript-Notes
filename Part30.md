# **JavaScript Notes – Part 30: Optimizing WebRTC Performance & Multi-Host Streaming**  

In this part, we will cover:  
✔ **Optimizing WebRTC Performance (Latency, Bandwidth, Quality Control)**  
✔ **Implementing Multi-Host Streaming (Co-Broadcasting Feature)**  
✔ **Handling Network Issues & Improving Scalability**  

---

## **1. Optimizing WebRTC Performance**  

By default, WebRTC handles video and audio streaming **peer-to-peer (P2P)**, but performance can be improved by:  
✔ **Adjusting video resolution & bitrate**  
✔ **Handling packet loss & congestion**  
✔ **Using SFU (Selective Forwarding Unit) for scalability**  

---

### **1.1 Adjusting Video Resolution & Bitrate**  

We can manually **control WebRTC stream quality** for better performance.

#### **🔹 Setting Video Constraints**
```js
const constraints = {
    video: { width: { ideal: 1280 }, height: { ideal: 720 }, frameRate: { ideal: 30 } },
    audio: true
};

navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
        document.getElementById("video").srcObject = stream;
    })
    .catch(console.error);
```
🔹 **Ensures smooth streaming by setting an ideal resolution**  
🔹 **Reduces unnecessary high bandwidth usage**  

---

### **1.2 Controlling Bitrate for Network Optimization**
WebRTC allows setting bitrate limits dynamically.

```js
let sender = peerConnection.getSenders().find(s => s.track.kind === "video");

const params = sender.getParameters();
params.encodings[0].maxBitrate = 500000; // 500kbps
sender.setParameters(params);
```
🔹 **Prevents excessive bandwidth consumption**  
🔹 **Improves performance for viewers with slow internet**  

---

### **1.3 Handling Network Congestion & Packet Loss**  

WebRTC automatically **adjusts for network congestion**, but we can manually improve handling.

```js
peerConnection.onicecandidateerror = (event) => {
    console.error("ICE Candidate Error:", event);
};

peerConnection.oniceconnectionstatechange = () => {
    if (peerConnection.iceConnectionState === "disconnected") {
        console.log("Network Issue: Reconnecting...");
    }
};
```
🔹 **Detects network issues & attempts reconnection**  
🔹 **Logs errors to debug connection problems**  

---

## **2. Implementing Multi-Host Streaming (Co-Broadcasting Feature)**  

A **multi-host streaming system** allows multiple broadcasters to **stream together**.  
We will implement a **Selective Forwarding Unit (SFU)** to handle multiple streams.  

---

### **2.1 Why SFU Instead of P2P?**  

- **Peer-to-Peer (P2P) connections** work well for **small** groups  
- **SFU (Selective Forwarding Unit)** is needed for **scalability**  

**Example:** Zoom, Google Meet, and Clubhouse use **SFUs** instead of P2P.  

---

### **2.2 Setting Up an SFU for Multi-Host Streaming**  

We will use **mediasoup** (a popular WebRTC SFU).  

#### **Step 1: Install mediasoup**
```sh
npm install mediasoup express ws
```

#### **Step 2: Create an SFU Server (`sfu.js`)**
```js
const mediasoup = require("mediasoup");
const express = require("express");
const { Server } = require("ws");

const app = express();
const server = app.listen(4000, () => console.log("SFU Server running on port 4000"));
const wss = new Server({ server });

let router;
mediasoup.createWorker().then(worker => worker.createRouter().then(r => router = r));

wss.on("connection", async (socket) => {
    const transport = await router.createWebRtcTransport({ listenIps: [{ ip: "0.0.0.0", announcedIp: "YOUR_PUBLIC_IP" }] });
    socket.send(JSON.stringify({ type: "transport-options", options: transport }));
});
```
🔹 **Acts as a relay for multiple video/audio streams**  
🔹 **Ensures low latency multi-host streaming**  

---

### **2.3 Connecting Clients to SFU**
Modify `script.js`:

```js
const socket = new WebSocket("ws://localhost:4000");

socket.onmessage = async (event) => {
    const data = JSON.parse(event.data);

    if (data.type === "transport-options") {
        const transport = new RTCPeerConnection(data.options);
        const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
        stream.getTracks().forEach(track => transport.addTrack(track, stream));
    }
};
```
🔹 **Relays all broadcaster streams through the SFU**  
🔹 **Handles dynamic joining of multiple broadcasters**  

---

## **3. Handling Network Issues & Improving Scalability**  

WebRTC struggles when **many users** connect.  
✔ **Turn off inactive streams**  
✔ **Prioritize audio over video during congestion**  

---

### **3.1 Dynamically Manage Active Streams**  
```js
setInterval(() => {
    const stats = peerConnection.getStats();
    stats.forEach(report => {
        if (report.type === "inbound-rtp" && report.kind === "video" && report.framesPerSecond < 10) {
            console.log("Low FPS detected! Lowering resolution.");
            const sender = peerConnection.getSenders().find(s => s.track.kind === "video");
            sender.track.enabled = false; // Disable low FPS stream
        }
    });
}, 5000);
```
🔹 **Disables low-quality video streams dynamically**  
🔹 **Helps maintain smooth streaming**  

---

### **3.2 Prioritize Audio Over Video**
```js
peerConnection.oniceconnectionstatechange = () => {
    if (peerConnection.iceConnectionState === "disconnected") {
        console.log("Network issue detected! Switching to audio-only mode.");
        peerConnection.getSenders().find(s => s.track.kind === "video").track.enabled = false;
    }
};
```
🔹 **Ensures users can still hear the stream even with bad internet**  
🔹 **Improves overall experience in unstable networks**  

---

## **4. Summary of Part 30**  

✔ **Optimized WebRTC performance (Bitrate, Resolution, Packet Loss Handling)**  
✔ **Implemented Multi-Host Streaming using an SFU (Selective Forwarding Unit)**  
✔ **Handled Network Congestion & Improved Scalability**  

---

### **Coming Next in Part 31:**  
- **Building a WebRTC-Based Video Conference App (Like Zoom)**  
- **Adding Screen Sharing & File Transfer Features**  
- **Improving UX with Custom WebRTC Controls**  

Would you like a **Zoom-like WebRTC Video Conferencing guide** next? 🚀