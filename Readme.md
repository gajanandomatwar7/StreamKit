# P2P File Sharing

A premium, client-to-client peer-to-peer (P2P) file sharing application built with **WebRTC (PeerJS)**, **Node.js**, **Express**, and **Redis**. Designed with a sleek, minimalist aesthetic inspired by Nothing Tech.

## Features

- **Direct P2P Transfer**: File chunks are streamed directly between browser clients via WebRTC Data Channels without routing traffic through a central server.
- **Large File Support**: Segmented WebRTC stream chunking (64KB packets) with backpressure buffering limits ensures steady, crash-free downloads for large files.
- **Client-Side Compilation**: Leverages browser **IndexedDB** database cache for storing chunk streams locally, assembling them only on download completion to optimize memory usage.
- **Interactive Background**: Beautiful Canvas-based dot-matrix background that responds organically to mouse movements with dynamic size breathing and magnetic bouncing distortion.
- **State-Reactive Theme Colors**: The background dot-matrix color dynamically switches to reflect connection status:
  - **Green**: Connected and active file transfer in progress.
  - **Yellow**: Connected but idle, transfer paused, or during reconnection.
  - **Red**: Disconnected or session reset.
- **Session Auto-Resume**: Uses browser LocalStorage to cache host settings, allowing the uploader to quickly resume hosting links on refresh.
- **Auto-Redirection**: Receivers are automatically redirected back to the homepage after 3 seconds if the uploader resets or terminates the session.
- **Responsive Layout**: Clean responsive layout tailored for Android, iOS, iPad, MacBook, and standard laptops.

---

## Tech Stack

- **Frontend**: HTML5, Vanilla CSS3 (Custom-designed CSS), Vanilla Javascript (ES6), HTML Canvas, PeerJS Client.
- **Backend**: Node.js, Express, PeerJS Server (Signaling server).
- **Session Cache**: Redis (Storing peer address maps).

---

## Installation & Setup

### Prerequisites
- [Node.js](https://nodejs.org/) (v16+)
- [Redis](https://redis.io/) server running locally or accessible via URL.

### 1. Clone the repository
```bash
git clone <your-repo-url>
cd P2P-file-Sharing
```

### 2. Install dependencies
```bash
npm install
```

### 3. Configure environment variables
Create a `.env` file in the root directory:
```env
PORT=3001
REDIS_URL=redis://127.0.0.1:6379
STUN_SERVER=stun:stun.l.google.com:19302
```

### 4. Run the application
Start the development server:
```bash
npm start
```
The server will be running at [http://localhost:3001](http://localhost:3001).

---
## How It Works
1. **Uploader Dashboard**:
   - The sender drops or selects a file.
   - The server registers a room mapping to the uploader's PeerJS ID in Redis.
   - The sender receives a dynamic shareable link (e.g., `http://localhost:3001/download/xyz`).
2. **Receiver Connection**:
   - When a receiver opens the link, the client queries the server for the uploader's active PeerJS address.
   - The receiver directly connects to the uploader via PeerJS (WebRTC).
3. **Data Channel Transfer**:
   - The receiver clicks **Download**.
   - The uploader slices the local file into chunks and streams them directly to the receiver.
   - The receiver stores the incoming bytes dynamically in IndexedDB.
   - Once all chunks arrive, the receiver compiles the chunks into a blob and triggers a browser file download automatically.
