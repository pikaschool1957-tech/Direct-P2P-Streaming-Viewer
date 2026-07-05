# Direct P2P Streaming Viewer

A lightweight, browser-based peer-to-peer video streaming application built with **WebRTC** and **PeerJS**. Stream video directly between two peers without any centralized server involvement.

## 🎯 Features

- **Direct P2P Connection**: Uses WebRTC for direct peer-to-peer communication
- **No Server Required**: Leverages PeerJS cloud broker for signaling only
- **Bi-directional Streaming**: Support for both sending and receiving video streams
- **Simple UI**: Clean, intuitive interface for easy connection setup
- **Real-time Status Updates**: Live connection status feedback
- **Low Latency**: Direct connection minimizes streaming delay

## 📋 How It Works

1. **Peer Discovery**: Each peer gets a unique ID assigned by PeerJS
2. **Connection Setup**: Share your Peer ID with the other user
3. **Stream Initiation**: Enter the remote peer's ID and click "Start Streaming Video"
4. **Live Viewing**: Video streams are received and displayed in real-time

### Connection Flow

```
User A                           User B
  ↓                                ↓
Peer A (ID) ←→ Signaling ←→ Peer B (ID)
  ↓           PeerJS Cloud         ↓
 Camera                         Display
```

## 🚀 Getting Started

### Prerequisites

- Modern web browser with WebRTC support (Chrome, Firefox, Safari, Edge)
- Webcam/microphone access
- Internet connection
- PeerJS library (`peerjs.min.js`)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/pikaschool1957-tech/Direct-P2P-Streaming-Viewer.git
   cd Direct-P2P-Streaming-Viewer
   ```

2. **Download PeerJS Library**
   - Download `peerjs.min.js` from [PeerJS CDN](https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js)
   - Place it in the project root directory
   - Or uncomment the CDN link in `index.html` (line 7)

3. **Open in Browser**
   - Open `index.html` in your web browser
   - Or use a local server:
     ```bash
     python -m http.server 8000
     # Then visit http://localhost:8000
     ```

## 📖 Usage

### Step-by-Step Guide

1. **Open the Application**
   - Two users each open `index.html` in their browsers
   - Each receives a unique Peer ID (shown in cyan text)

2. **Share Your ID**
   - Copy your Peer ID
   - Send it to the person you want to stream with

3. **Connect to Remote Peer**
   - Paste the remote peer's ID in the input field
   - Click "Start Streaming Video"
   - Grant camera/microphone permissions when prompted

4. **View Stream**
   - Wait for the connection to establish
   - Video will display in the player
   - Status will update to show connection state

### Connection States

- **"Status: Ready. Waiting for connection or call."** - Application initialized and ready
- **"Status: Requesting camera access..."** - Asking browser for camera permissions
- **"Status: Initiating P2P call..."** - Connecting to remote peer
- **"Status: P2P Connected (Streaming out)"** - Successfully sending video
- **"Status: P2P Connected (Receiving Stream)"** - Successfully receiving video
- **"Status: Error (error-type)"** - Connection error occurred

## 🔧 Technical Details

### Technologies Used

- **WebRTC**: Real-time communication protocol for media streaming
- **PeerJS**: JavaScript library that simplifies WebRTC peer connections
- **HTML5 Video**: Native browser video element for playback

### Key Components

| Component | Purpose |
|-----------|---------|
| `Peer()` | Initializes WebRTC peer connection |
| `peer.call()` | Initiates outgoing media stream |
| `peer.on('call')` | Listens for incoming media streams |
| `getUserMedia()` | Accesses device camera/microphone |
| `MediaStream` | Handles audio/video data |

### Browser Support

| Browser | Support | Notes |
|---------|---------|-------|
| Chrome | ✅ Full | Recommended |
| Firefox | ✅ Full | Recommended |
| Safari | ✅ Full | iOS 11+ |
| Edge | ✅ Full | Chromium-based |

## ⚙️ Configuration

### PeerJS Options

Edit the Peer initialization in `index.html` (line 41):

```javascript
const peer = new Peer({
    debug: 1  // Set to 0 for production (less logging)
});
```

### Optional Custom PeerJS Server

For production deployments, consider self-hosting PeerJS:

```javascript
const peer = new Peer({
    host: 'your-peerjs-server.com',
    port: 443,
    secure: true
});
```

## 🛡️ Privacy & Security

- **Local Processing**: Media streams are processed locally in the browser
- **Direct P2P**: Data doesn't pass through a central server
- **Encrypted Connection**: WebRTC uses DTLS-SRTP encryption
- **No Recording**: The application doesn't store or record streams

## ⚠️ Known Limitations

- **NAT Traversal**: May have issues with strict NAT/firewall (TURN servers can help)
- **One-way by Default**: Incoming calls use one-way streaming (can be modified for bidirectional)
- **Browser Only**: Requires web browser (no native app)
- **Connection Dependency**: Relies on internet connectivity and signaling service

## 🔄 Bidirectional Streaming

To enable both peers to send AND receive video, modify the `peer.on('call')` handler:

```javascript
peer.on('call', (call) => {
    statusEl.innerText = "Status: Incoming call. Connecting P2P stream...";
    
    // Get local stream
    const localStream = await navigator.mediaDevices.getUserMedia({ 
        video: true, 
        audio: true 
    });
    
    // Answer with local stream
    call.answer(localStream);
    
    call.on('stream', (incomingStream) => {
        remoteVideo.srcObject = incomingStream;
        statusEl.innerText = "Status: P2P Connected (Bidirectional)";
    });
});
```

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| **"Peer ID not loading"** | Check browser console for PeerJS errors; ensure `peerjs.min.js` is loaded |
| **"Camera access denied"** | Grant camera permissions in browser settings |
| **"Connection fails"** | Check internet; peer may be offline; verify Peer ID is correct |
| **"No video display"** | Ensure sender has granted camera access; check browser permissions |
| **"Audio/video is broken"** | Try a different browser; check device microphone/camera |

## 📝 License

This project is open source. Include appropriate license information here.

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs and issues
- Suggest features and improvements
- Submit pull requests
- Improve documentation

## 📚 Resources

- [WebRTC Documentation](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API)
- [PeerJS Documentation](https://peerjs.com/)
- [WebRTC Best Practices](https://www.html5rocks.com/en/tutorials/webrtc/basics/)

## 💡 Future Enhancements

- [ ] Multi-party streaming support
- [ ] Screen sharing capability
- [ ] Recording and playback features
- [ ] Advanced codec selection
- [ ] Connection statistics dashboard
- [ ] Chat integration
- [ ] Mobile app version

---

**Questions or issues?** Feel free to open an issue on GitHub or reach out to the maintainers.
