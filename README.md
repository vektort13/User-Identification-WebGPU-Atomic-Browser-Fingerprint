# Vektor T13 - WebGPU Atomic Browser Fingerprint

Vektor T13 Technologies  
detect.expert  
Developed by Dmytro Momot

A single-file, offline WebGPU demo that derives a browser/device fingerprint from atomic contention behavior inside GPU compute shaders.

---

## Overview

This project demonstrates a hardware-dependent fingerprinting signal based on WebGPU compute execution.

Multiple compute shader workgroups are forced to compete for a single atomic counter using atomicAdd. The resulting distribution of increments across workgroups reflects GPU architecture, driver behavior, scheduling, and execution characteristics.

The measured distribution is transformed into a compact feature vector and hashed to produce a reproducible device signal.

---

## Core idea

The fingerprint is built from atomic contention:

- A global atomic counter is shared by all workgroups
- Each workgroup increments the counter until a fixed limit is reached
- Each workgroup records how many increments it performed
- The per-workgroup increment distribution is collected

This distribution is highly sensitive to GPU execution details and is difficult to normalize across different hardware.

---

## Processing pipeline

For each compute configuration:

- Run multiple trials
- Aggregate results using the median
- Normalize per-workgroup counts
- Downsample into a fixed number of bins
- Concatenate distributions from all configurations
- Quantize the final vector
- Compute a SHA-256 hash

The resulting hash represents the WebGPU Atomic Browser Fingerprint.

---

## Identification workflow

The page supports a local identification mode:

- Fingerprints can be enrolled into a local gallery
- New fingerprints can be compared against enrolled samples
- Similarity metrics:
  - Cosine similarity
  - Jensen–Shannon distance
- A heuristic decision indicates whether the device is likely the same or different

All data is stored locally using localStorage.

---

## Features

- WebGPU compute shader fingerprinting
- AtomicAdd contention analysis
- Multiple compute presets and workgroup layouts
- Median aggregation across trials
- Normalization and downsampling
- SHA-256 fingerprint hash
- Local enrollment and identification
- Export and import of local gallery (JSON)
- Hardware and API information via GPUAdapter.info
- Multi-language interface:
  - English
  - Russian
  - Simplified Chinese
  - Vietnamese
- No network requests (offline by design)

---

## Requirements

- Browser with WebGPU support
- Secure context is required:
  - HTTPS
  - or localhost

WebGPU is not available when opened via file://.

---

## Limitations

- Results may vary depending on:
  - GPU driver version
  - Power and thermal state
  - Background GPU load
- Browsers may intentionally reduce fingerprinting surface
  - GPUAdapter.info fields may be empty or masked
- Similarity thresholds are heuristic and may require tuning

This is a probabilistic fingerprint, not a cryptographic identifier.

---

## Privacy and ethics

This project demonstrates a browser fingerprinting technique.

The demo performs no network communication and stores all data locally, but fingerprinting can have privacy implications. Use responsibly and only with appropriate consent and legal justification.

---

## Credits

© Vektor T13 Technologies  
detect.expert  
Developed by Dmytro Momot
