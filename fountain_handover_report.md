# AirGapQR Fountain Code Implementation Handover Report

## 1. Project Objective
Upgrade the fixed-rate carousel QR transfer tool to a **Rateless Fountain Code (LT-Code)** system to handle large files (10,000+ chunks) more efficiently while maintaining 100% visual and interactive parity with the original V1 (`index.html`).

## 2. Core Product Requirements (Non-Negotiable)
- **UI Aesthetics**: Pixel-perfect clone of V1.
  - Fonts: `system-ui`, `font-black`, `font-bold`.
  - Layout: Dual-thumb slider strictly below the QR code.
  - Footer: Bottom-aligned shortcut hints with grey `key-badge` bubbles.
- **Interactions & Hotkeys**:
  - `S/R`: Switch Tabs.
  - `F/T`: Switch Send Mode (File/Text).
  - `Space`: Play/Pause.
  - `Enter`: Prepare Beam.
  - `Esc`: Reset Transfer.
- **Two-Stage Transfer Protocol**:
  - **Stage 1 (Frame 0 - Manifest)**: Metadata ONLY (TID, Total Count `K`, Block Size `BS`, Filename). Scanned first to initialize the receiver.
  - **Stage 2 (Frame 1-K - Fountain)**: Infinite stream of XOR-mixed packets using a shared Seed + Range logic.

## 3. Technical Architecture (Current)
### A. Sender (Fountain Engine)
- **Chunking**: Data is gzipped and split into uniform blocks, with the last block padded with zeroes to match `state.chunkSize`.
- **Packet Format**:
  - `M|TID|K|BS|Name` (Manifest - Static Frame 0)
  - `F|TID|Seed|RangeS|RangeLen|Data` (Fountain - Frames 1-K)
- **Degree Distribution**: Robust Soliton Distribution (simplified) for picking the number of blocks per packet.
- **PRNG**: `Math.imul` based 32-bit integer LCG for cross-platform bit-accurate sequence generation.

### B. Receiver (Peeling Decoder)
- **Dynamic Allocation**: Learns `blockSize` and `totalBlocks` from the `M` packet.
- **XOR Solver**: Bitwise XOR reduction (Peeling Decoder). When a degree-1 packet is found, it "peels off" that block from all other buffered droplets.
- **Finalization**: Concatenates solved blocks and runs `pako.ungzip`.

## 4. Current Blocked Issue: `unknown compression method`
Despite the `Math.imul` fix for PRNG precision, `pako.ungzip` still fails at the end of some transfers.

### Potential Hypotheses for Next Steps:
1. **Trailing Zero Inflation**: The `full` array concatenated from solved blocks has `k * blockSize` bytes. If index 0..K-1 were all padded, `pako` might be seeing extra garbage after the Gzip footer.
   - *Investigation*: Try calculating the exact expected compressed length and slicing the `full` array before `ungzip`.
2. **Indexing Discrepancy (The most likely culprit)**: In `renderFrame` (Sender) and `processDroplet` (Receiver), check if `indices` mapping for `rangeStart > 0` (Focused mode) is perfectly aligned.
3. **ArrayBuffer vs BinaryString**: Double check the `atob` / `btoa` and `String.fromCharCode` path. For larger chunks, we should ensure no weird encoding shifts are happening.
4. **XOR Corruption**: Verify if a block is being XORed into a `dr.data` multiple times if the droplet pool contains duplicates.

## 5. Summary of Files
- **`fountain.html`**: The unified dev target (UI + Fountain Core).
- **`index.html`**: The UI reference (Static Carousel).
- **`lib/`**: Contains `pako`, `qrious`, `html5-qrcode`.

---
*End of Handover Record - 2026-03-27*
