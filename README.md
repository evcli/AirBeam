# AirGapQR 🚀

AirGapQR is a web-based, fully offline tool to transfer files and text between devices using high-capacity QR codes. No internet or cables required.

## ✨ Features
- **Total Privacy**: All data is processed in your browser.
- **100% Offline**: All dependencies are bundled in the `lib` folder.
- **Files & Text**: Supports any file type and a convenient "Text Mode" with direct copy.
- **Fast & Reliable**: Uses Gzip compression and optimized QR chunking.

## � How to Run
Run a simple local server to start (required for camera access):

```bash
cd AirGapQR
python3 -m http.server 8080
```
Then open `http://localhost:8080` in your browser.

## 📖 How to Use
1. **Sender**: Choose **File** or **Text**, click **Prepare Beam**, then click **PLAY**.
2. **Receiver**: Go to the **RECEIVER** tab, click **SCAN NOW**, and point your camera at the sender's screen.
3. **Finish**: Once complete, **Download** the file or **Copy** the received text.

## ⌨️ Keyboard Shortcuts & Automation
AirGapQR supports keyboard shortcuts and URL parameters for seamless integration with automation tools like AutoHotKey:

### Shortcuts
- `S` / `R`: Switch Tab (**S**ender / **R**eceiver)
- `F`: Switch to **File Mode** and **immediately open file dialog**
- `T`: Switch to **Text Mode** and **immediately focus textarea**
- `Enter`: Prepare Beam (when file/text is ready)
- `Space`: Play/Pause QR playback
- `Esc`: Reset current state / Close dialog

### Automation (URL Parameters)
- `?mode=file` or `?mode=send`: Starts in File Mode.
- `?mode=text`: Starts in Text Mode and focuses input.
- `?mode=text&text=YourContent`: Pre-fills "YourContent" in text mode. Press `Enter` to start.

## 🛠 Libs
- Tailwind CSS 3.4.1
- Pako 2.0.3
- QRious 4.0.2
- Html5-QRCode 2.3.8
