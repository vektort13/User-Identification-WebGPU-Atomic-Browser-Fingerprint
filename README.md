# Vektor T13 - WebGPU Atomic Browser Fingerprint

**Vektor T13 Technologies** • **detect.expert**  
Developed by **Dmytro Momot**

A self‑contained, **offline** WebGPU demo that collects a device signal based on an *AtomicIncrement* pattern: multiple compute‑shader workgroups compete for a single `atomicAdd` counter, and the resulting per‑workgroup increment distribution forms a hardware‑dependent fingerprint.

> ⚠️ WebGPU requires a **secure context** (`https://` or `http://localhost`).  
> ✅ This page performs **no network I/O** (`connect-src 'none'` via CSP).  
> 💾 The local “gallery” is stored in `localStorage` for this origin.

## Features

- **Atomic workgroup distribution**
  - Compute shader uses a single global atomic counter (`atomicAdd`) and records how increments are distributed across workgroups.
- **Stable feature vector + hash**
  - For each config: median across trials → normalize → downsample to N bins
  - Concatenate all configs → quantize to bytes → `SHA-256` via `SubtleCrypto.digest()`
- **Local enrollment & identification**
  - Save samples to a local gallery (`localStorage`)
  - Compare current sample against enrolled ones using:
    - Cosine similarity
    - Jensen–Shannon distance
- **Hardware & API hints**
  - Reads `GPUAdapter.info` (fallback to deprecated `requestAdapterInfo()` if available)
  - Displays WebGPU features and compute limits
- **UI language switcher**
  - Russian / English / Simplified Chinese / Vietnamese (top‑right)

## How to use

1. Host the file via **localhost** or HTTPS (WebGPU won’t work from `file://`).
   Example:
   ```bash
   python -m http.server 8000
