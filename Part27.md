# **JavaScript Notes – Part 27: WebRTC Live Streaming (Broadcast Mode)**  

In this part, we will cover:  
✔ **Understanding WebRTC Live Streaming (Broadcast Mode)**  
✔ **Setting up a WebRTC broadcast using a single sender (host) and multiple viewers**  
✔ **Optimizing WebRTC for large-scale live streaming**  
✔ **Using Media Servers (e.g., Janus, MediaSoup) for scalability**  

---

## **1. Understanding WebRTC Live Streaming (Broadcast Mode)**  

WebRTC **peer-to-peer connections** are great for small groups, but for **live streaming to a large audience**, a **broadcast model** is required.  

### **Broadcasting vs. Group Calls**
| Feature         | Peer-to-Peer Calls | Broadcast Mode |
|----------------|-------------------|---------------|
| Number of Viewers | 2-10 | Hundreds to thousands |
| Latency | Low (real-time) | Slight delay (optimized for stability) |
| Server Required? | No (except signaling) | Yes (for media relay) |

---

## **2. Setting Up WebRTC Live Streaming (One-to-Many)**  

A **host (broadcaster)** sends a video stream, and **viewers** receive it.  

### **2.1 Host (Broadcaster) Code**  

The host captures their video and shares it via WebRTC.  

```js
const peerConnections = {}; // Store connections for viewers

async function startBroadcast() {
    const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
    document.getElementById("hostVideo").srcObject = stream; // Show preview

    socket.on("viewer-join", (viewerId) => {
        const peerConnection = new RTCPeerConnection();

        stream.getTracks().forEach(track => peerConnection.addTrack(track, stream));

        peerConnection.onicecandidate = (event) => {
            if (event.candidate) {
                socket.emit("candidate", { viewerId, candidate: event.candidate });
            }
        };

        peerConnections[viewerId] = peerConnection;

        peerConnection.createOffer().then(offer => {
            peerConnection.setLocalDescription(offer);
            socket.emit("offer", { viewerId, offer });
        });
    });
}
```
🔹 **Captures the host's video and sends it to viewers who join.**  
🔹 **Each new viewer gets an offer for WebRTC connection.**  

---

### **2.2 Viewer (Receiver) Code**  

When a viewer joins, they receive the broadcast.  

```js
const peerConnection = new RTCPeerConnection();

socket.on("offer", async (data) => {
    await peerConnection.setRemoteDescription(new RTCSessionDescription(data.offer));
    const answer = await peerConnection.createAnswer();
    await peerConnection.setLocalDescription(answer);
    socket.emit("answer", { hostId: data.hostId, answer });
});

peerConnection.ontrack = (event) => {
    document.getElementById("viewerVideo").srcObject = event.streams[0];
};
```
🔹 **Receives the host's video and displays it.**  

---

## **3. Handling Multiple Viewers Dynamically**  

When a viewer joins:  
✔ The **server notifies the broadcaster**  
✔ The **broadcaster adds a new WebRTC connection**  

```js
socket.on("viewer-left", (viewerId) => {
    if (peerConnections[viewerId]) {
        peerConnections[viewerId].close();
        delete peerConnections[viewerId];
    }
});
```
🔹 **Removes disconnected viewers from the session.**  

---

## **4. Scaling WebRTC Live Streaming with a Media Server**  

For **large-scale broadcasts**, **peer-to-peer** connections are inefficient.  
Instead, we use **media servers** to relay the video.  

### **4.1 Why Use a Media Server?**
| Solution | Pros | Cons |
|----------|------|------|
| WebRTC Mesh | No server needed | High bandwidth usage |
| SFU (Selective Forwarding Unit) | Efficient for group calls | Requires a media server |
| **Media Server (Janus, MediaSoup, Jitsi)** | Scalable for 1000s of viewers | Needs server setup |

### **4.2 Using Janus WebRTC Server for Broadcasting**  

Janus acts as a **relay server** between the broadcaster and viewers.  

#### **Broadcaster (Janus Client)**
```js
const janus = new Janus({
    server: "wss://your-janus-server.com",
    success: () => {
        janus.attach({
            plugin: "janus.plugin.videoroom",
            success: (pluginHandle) => {
                pluginHandle.createOffer({
                    media: { video: true, audio: true },
                    success: (jsep) => {
                        pluginHandle.send({ message: { request: "publish" }, jsep });
                    }
                });
            }
        });
    }
});
```
🔹 **Connects to Janus and starts a video broadcast.**  

#### **Viewer (Janus Client)**
```js
janus.attach({
    plugin: "janus.plugin.videoroom",
    success: (pluginHandle) => {
        pluginHandle.send({ message: { request: "join", room: 1234 } });
    },
    onmessage: (msg, jsep) => {
        if (jsep) {
            pluginHandle.createAnswer({
                jsep,
                success: (jsep) => {
                    pluginHandle.send({ message: { request: "start" }, jsep });
                }
            });
        }
    },
    onremotestream: (stream) => {
        document.getElementById("viewerVideo").srcObject = stream;
    }
});
```
🔹 **Viewers connect to the Janus server instead of direct P2P connections.**  

---

## **5. Summary of Part 27**  

✔ **Built a WebRTC broadcast system (One-to-Many)**  
✔ **Handled multiple viewers dynamically**  
✔ **Optimized WebRTC for large-scale streaming using Janus**  

---

### **Coming Next in Part 28:**  
- **Building a Full WebRTC Streaming Platform with Chat & Controls 🚀**  
- **Integrating WebSockets for Live Chat**  

Would you like a full **WebRTC streaming project** tutorial next? 🚀