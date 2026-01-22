# Ultraleap LeapC on macOS (Apple Silicon) – CLion Example

This repository demonstrates how to build and run **Ultraleap LeapC (C API)** on **macOS Apple Silicon** using **CLion and CMake**.
 
✅ Native C  
✅ Works with Ultraleap Hand Tracking app

---

## Requirements

### Hardware
- Apple Silicon Mac
- Leap Motion Controller

### Software
- Ultraleap Hand Tracking App
- CLion
- Xcode Command Line Tools

---

## Ultraleap SDK Path (macOS)

This project links against the SDK bundled inside:

Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK


---

## Build & Run

```bash
mkdir build
cd build
cmake ..
cmake --build .
./leap_clion

Or simply open the folder in CLion and click Run.

---
Expected Output:
Connected.
Frame 160499 with 1 hands.
Hand id 234 is a right hand with position (x, y, z)

---
**License**
Ultraleap SDK files are not included in this repository.
You must install Ultraleap Hand Tracking separately.

