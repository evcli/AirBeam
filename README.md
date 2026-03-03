# AirBeam 🚀

AirBeam is a web-based, fully offline tool to transfer files and text between devices using high-capacity QR codes. No internet or cables required.

## ✨ Features
- **Total Privacy**: All data is processed in your browser.
- **100% Offline**: All dependencies are bundled in the `lib` folder.
- **Files & Text**: Supports any file type and a convenient "Text Mode" with direct copy.
- **Fast & Reliable**: Uses Gzip compression and optimized QR chunking.

## � How to Run
Run a simple local server to start (required for camera access):

```bash
cd AirBeam
python3 -m http.server 8080
```
Then open `http://localhost:8080` in your browser.

## 📖 How to Use
1. **Sender**: Choose **File** or **Text**, click **Prepare Beam**, then click **PLAY**.
2. **Receiver**: Go to the **RECEIVER** tab, click **SCAN NOW**, and point your camera at the sender's screen.
3. **Finish**: Once complete, **Download** the file or **Copy** the received text.

## ⌨️ Keyboard Shortcuts & Automation
AirBeam supports keyboard shortcuts and URL parameters for integration with tools like AutoHotKey:

### Shortcuts
- `S` / `R`: Switch Tab
- `F` / `T`: Switch Send Mode (File/Text)
- `Enter`: Prepare Beam
- `Space`: Play/Pause QR
- `Esc`: Reset / Cancel

### Automation (URL Parameters)
- `index.html?mode=file&auto=1`: Opens file dialog immediately.
- `index.html?mode=text`: Opens text mode and focuses input.
- `index.html?mode=text&text=HELLO&auto=1`: Pre-fills and prepares "HELLO".

## 🛠 Libs
- Tailwind CSS 3.4.1
- Pako 2.0.3
- QRious 4.0.2
- Html5-QRCode 2.3.8
