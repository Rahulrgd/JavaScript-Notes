# **JavaScript Notes – Part 29: Enhancing Security & Recording WebRTC Streams**  

In this part, we will cover:  
✔ **Securing WebRTC streaming (Authentication & Role Management)**  
✔ **Recording WebRTC streams and saving as MP4**  
✔ **Deploying the WebRTC streaming platform online**  

---

## **1. Securing WebRTC Streaming**  

By default, WebRTC **does not handle authentication**. We need to:  
✔ Implement **User Authentication** (Login/Signup)  
✔ Enforce **Role Management** (Host vs. Viewer)  

---

### **1.1 Implementing User Authentication**  

We'll use **JSON Web Tokens (JWT)** to secure the WebSocket connection.  

### **Step 1: Install Dependencies (Backend)**
```sh
npm install express jsonwebtoken bcryptjs cors
```

### **Step 2: Update `server.js` (Add Authentication)**
```js
const express = require("express");
const jwt = require("jsonwebtoken");
const bcrypt = require("bcryptjs");

const app = express();
app.use(express.json());
app.use(require("cors")());

const users = {}; // In-memory store (Use a database in production)

app.post("/register", async (req, res) => {
    const { username, password } = req.body;
    users[username] = { password: await bcrypt.hash(password, 10) };
    res.json({ message: "User registered" });
});

app.post("/login", async (req, res) => {
    const { username, password } = req.body;
    if (!users[username] || !(await bcrypt.compare(password, users[username].password))) {
        return res.status(401).json({ message: "Invalid credentials" });
    }
    const token = jwt.sign({ username }, "secret_key", { expiresIn: "1h" });
    res.json({ token });
});
```
🔹 **Registers & authenticates users securely.**  
🔹 **Uses JWT for session management.**  

---

### **Step 3: Modify WebSocket Server to Require Authentication**
```js
const wss = new Server({ server });

wss.on("connection", (socket, req) => {
    const token = req.url.split("?token=")[1];
    if (!token) return socket.close();

    try {
        const user = jwt.verify(token, "secret_key");
        console.log("User connected:", user.username);
    } catch {
        return socket.close();
    }

    socket.on("message", (message) => {
        const data = JSON.parse(message);
        if (data.type === "chat") {
            wss.clients.forEach(client => client.send(JSON.stringify(data)));
        }
    });
});
```
🔹 **Rejects unauthenticated users.**  
🔹 **Ensures only logged-in users can join.**  

---

## **2. Recording WebRTC Streams (Save as MP4)**  

WebRTC does not natively support recording.  
We use the **MediaRecorder API** to capture and save streams.  

### **Step 1: Modify `script.js` to Enable Recording**
```js
let mediaRecorder;
let recordedChunks = [];

document.getElementById("startStream").addEventListener("click", async () => {
    const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
    document.getElementById("video").srcObject = stream;

    mediaRecorder = new MediaRecorder(stream);
    mediaRecorder.ondataavailable = (event) => recordedChunks.push(event.data);
    mediaRecorder.start();
});

document.getElementById("stopStream").addEventListener("click", () => {
    mediaRecorder.stop();
    const blob = new Blob(recordedChunks, { type: "video/mp4" });
    const url = URL.createObjectURL(blob);

    const a = document.createElement("a");
    a.href = url;
    a.download = "recording.mp4";
    document.body.appendChild(a);
    a.click();
});
```
🔹 **Records WebRTC stream and saves it as an MP4 file.**  
🔹 **Downloads the recording locally.**  

---

## **3. Deploying the WebRTC Streaming Platform**  

### **Step 1: Host the Backend on a Server**
Use **DigitalOcean, AWS, or Heroku**.  
1. Create an account on **Heroku**  
2. Install Heroku CLI:  
   ```sh
   npm install -g heroku
   ```
3. Deploy the app:  
   ```sh
   git init
   heroku create
   git add .
   git commit -m "Deploy WebRTC app"
   git push heroku main
   ```

---

### **Step 2: Deploy Frontend with Vercel**
1. Install Vercel:  
   ```sh
   npm install -g vercel
   ```
2. Deploy:  
   ```sh
   vercel
   ```

---

## **4. Summary of Part 29**  

✔ **Added authentication (JWT) to WebRTC streaming**  
✔ **Enabled recording and saving WebRTC streams as MP4**  
✔ **Deployed the WebRTC app online**  

---

### **Coming Next in Part 30:**  
- **Optimizing WebRTC Performance (Bandwidth, Latency, Quality Control)**  
- **Implementing Multi-Host Streaming (Co-Broadcasting)**  

Would you like an in-depth guide on **multi-host WebRTC streaming** next? 🚀