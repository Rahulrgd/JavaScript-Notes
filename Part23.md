# JavaScript Notes – Part 23: Screen Sharing and Video Recording with WebRTC  

In this part, we will cover:  
✔ Implementing screen sharing with WebRTC  
✔ Recording video streams using `MediaRecorder`  
✔ Saving recorded videos locally  
✔ Enhancing video call performance  

---

## 1. Screen Sharing with WebRTC  

WebRTC allows sharing the user's screen using the `getDisplayMedia()` API.  

### **1.1 Capturing the Screen**
```js
async function startScreenShare() {
    try {
        const screenStream = await navigator.mediaDevices.getDisplayMedia({ video: true });
        document.getElementById("screenVideo").srcObject = screenStream;
    } catch (error) {
        console.error("Error starting screen share:", error);
    }
}
```
🔹 **Explanation:**  
- `getDisplayMedia({ video: true })` → Prompts the user to select a screen or window to share.  
- Assigns the captured stream to a `<video>` element.  

### **1.2 HTML for Screen Video**
```html
<video id="screenVideo" autoplay playsinline></video>
<button onclick="startScreenShare()">Start Screen Share</button>
```
🔹 This allows users to start screen sharing with a button click.  

---

## 2. Streaming the Screen to Other Users  

To share the screen with peers in a WebRTC call:  

### **2.1 Replace Video Track in WebRTC Connection**
```js
async function shareScreen() {
    const screenStream = await navigator.mediaDevices.getDisplayMedia({ video: true });
    const screenTrack = screenStream.getTracks()[0];

    // Replace existing video track with the screen track
    const sender = peerConnection.getSenders().find(s => s.track.kind === "video");
    if (sender) sender.replaceTrack(screenTrack);
    
    screenTrack.onended = () => {
        // Revert back to webcam when screen sharing stops
        navigator.mediaDevices.getUserMedia({ video: true, audio: true }).then((stream) => {
            sender.replaceTrack(stream.getTracks().find(t => t.kind === "video"));
        });
    };
}
```
🔹 **This allows dynamic switching between webcam and screen sharing.**  

---

## 3. Recording Video Streams  

WebRTC supports **recording video calls** using `MediaRecorder`.  

### **3.1 Setting Up MediaRecorder**  
```js
let mediaRecorder;
let recordedChunks = [];

function startRecording(stream) {
    mediaRecorder = new MediaRecorder(stream);
    
    mediaRecorder.ondataavailable = (event) => {
        if (event.data.size > 0) recordedChunks.push(event.data);
    };
    
    mediaRecorder.onstop = saveRecording;
    
    mediaRecorder.start();
}
```
🔹 **Explanation:**  
- `MediaRecorder(stream)` → Starts recording a media stream.  
- `ondataavailable` → Saves video data in chunks.  

---

### **3.2 Saving the Recording**  
```js
function saveRecording() {
    const blob = new Blob(recordedChunks, { type: "video/webm" });
    const url = URL.createObjectURL(blob);

    const a = document.createElement("a");
    a.href = url;
    a.download = "recorded-video.webm";
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
}
```
🔹 **This creates a downloadable video file after recording.**  

---

### **3.3 HTML for Recording Controls**
```html
<video id="recordedVideo" autoplay playsinline></video>
<button onclick="startRecording(localStream)">Start Recording</button>
<button onclick="mediaRecorder.stop()">Stop & Save</button>
```
🔹 The **recording starts/stops** with button clicks.  

---

## 4. Optimizing Video Call Performance  

WebRTC video calls can be optimized by:  

### **4.1 Adjusting Video Quality**
```js
const constraints = {
    video: { width: 1280, height: 720, frameRate: 30 },
    audio: true
};
navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
        document.getElementById("localVideo").srcObject = stream;
    });
```
🔹 **Higher resolution (e.g., 1080p) increases bandwidth usage.**  

---

### **4.2 Using Simulcast for Adaptive Streaming**
```js
const videoSender = peerConnection.getSenders().find(s => s.track.kind === "video");
videoSender.setParameters({
    encodings: [
        { rid: "low", maxBitrate: 200000 },
        { rid: "medium", maxBitrate: 500000 },
        { rid: "high", maxBitrate: 1500000 }
    ]
});
```
🔹 **Simulcast sends different video qualities for better adaptability.**  

---

### **4.3 Managing Bandwidth**
```js
peerConnection.setParameters({
    bandwidth: 1000000 // Limit bandwidth to 1Mbps
});
```
🔹 **Prevents network congestion in low-bandwidth conditions.**  

---

## Summary of Part 23:
✔ Implementing screen sharing with WebRTC  
✔ Recording video calls using `MediaRecorder`  
✔ Saving recorded videos locally  
✔ Optimizing WebRTC performance  

---

### **Coming Next in Part 24:**  
- **WebRTC Data Channels for File Transfer**  
- **End-to-End Encryption for Secure Calls**  

Would you like to add more features before proceeding? 🚀