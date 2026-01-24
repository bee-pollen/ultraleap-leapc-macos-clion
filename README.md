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
- Ultraleap Hand Tracking App (SDK / Library is included here)
- CLion
- Xcode Command Line Tools

---

## Ultraleap SDK Path (macOS)

This project links against the SDK bundled inside:

Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK


---

## Expected Output
Connected.
Frame 160499 with 1 hands.
Hand id 234 is a right hand with position (x, y, z)

---

## License & Attribution
This project demonstrates how to build and run Ultraleap LeapC SDK samples on macOS
using CMake and CLion.



- Ultraleap LeapC SDK, headers, libraries, and sample code are © Ultraleap Ltd.
- Ultraleap SDK files are **not included** in this repository.
- You must install **Ultraleap Hand Tracking** separately.

This repository contains only:
- Build configuration (CMake)
- Integration and example application code
- Instructions for compiling Ultraleap samples on macOS

Use of Ultraleap technology is subject to the Ultraleap SDK Agreement:
https://central.leapmotion.com/agreements/SdkAgreement

---
<h2>Build &amp; Run</h2>

<p>You can build and run the project manually from the terminal:</p>

<pre><code>mkdir build
cd build
cmake ..
cmake --build .
./leap_clion
</code></pre>

<p>
  Or simply open the project folder in <strong>CLion</strong> and click
  <strong>Run</strong>.
</p>

<h1>Leap Motion in C on Mac (Apple Silicon) + CLion Setup (Ultraleap LeapC)</h1>

<h2>Requirements</h2>

<h3>Hardware</h3>
<ul>
  <li>Mac with Apple Silicon (M1/M2/M3) (I am using MacBook Air M2)</li>
  <li>Leap Motion Controller (I am using the old controller)</li>
  <li>USB cable / adapter if needed (I’m using the Leap C given cable + USB-A to USB-C)</li>
</ul>

<h3>Software</h3>
<ul>
  <li>Mac running updated software (I am using Tahoe v26.2)</li>
  <li>Ultraleap Hand Tracking app</li>
  <li>CLion</li>
  <li>Terminal (do not use Rosetta — turn it off in settings)</li>
  <li>Xcode Command Line Tools (needed for clang)</li>
</ul>

<h2>Step By Step Documentation</h2>

<h3>Step 1 — Make sure all hardware is ready and updated</h3>
<ul>
  <li>Check Xcode (for clang). In Terminal:</li>
</ul>
<pre><code>xcode-select --install
</code></pre>

<h3>Step 2 — Install Ultraleap Hand Tracking app</h3>
<ul>
  <li>Open Ultraleap Hand Tracking control panel and make sure tracking is working.</li>
  <li><strong>Very important:</strong> confirm hands are detected before continuing.</li>
</ul>

<h3>Step 3 — Find LeapSDK paths in the app bundle</h3>
<p>Paths:</p>
<ul>
  <li><code>/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/include/LeapC.h</code></li>
  <li><code>/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/lib/libLeapC.dylib</code></li>
</ul>

<p>Verify in Terminal:</p>
<pre><code>ls "/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/include/LeapC.h"
ls "/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/lib/libLeapC.dylib"
</code></pre>

<h3>Step 4 — Create a clean project folder (outside of /Applications)</h3>
<p>
  This is because macOS blocks writing inside app bundles and <code>/Applications</code>.
  (You can disable this, but it’s not recommended.)
</p>

<p>Create your project folder:</p>
<pre><code>mkdir -p ~/Dev/leap_clion
</code></pre>

<h3>Step 5 — Copy the official Ultraleap samples into your folder</h3>
<ul>
  <li><code>PollingSample.c</code> = prints frames/hands</li>
  <li><code>ExampleConnection.c/.h</code> = handles Leap connection + polling loop</li>
</ul>

<pre><code>cp "/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/samples/PollingSample.c" ~/Dev/leap_clion/main.c
cp "/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/samples/ExampleConnection.c" ~/Dev/leap_clion/
cp "/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/samples/ExampleConnection.h" ~/Dev/leap_clion/
</code></pre>

<h3>Step 6 — Create your CMakeLists.txt</h3>
<p>Edit:</p>
<pre><code>nano ~/Dev/leap_clion/CMakeLists.txt
</code></pre>

<p>Paste:</p>
<pre><code>cmake_minimum_required(VERSION 3.20)
project(leap_clion C)

set(CMAKE_C_STANDARD 11)

add_executable(leap_clion
        main.c
        ExampleConnection.c
)

# Ultraleap SDK paths (macOS)
set(ULTRALEAP_SDK "/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK")

target_include_directories(leap_clion PRIVATE
        "${ULTRALEAP_SDK}/include"
)

target_link_directories(leap_clion PRIVATE
        "${ULTRALEAP_SDK}/lib"
)

target_link_libraries(leap_clion PRIVATE
        LeapC
        "-framework CoreFoundation"
)

# Runtime library search path so dyld can find libLeapC.dylib
set_target_properties(leap_clion PROPERTIES
        BUILD_RPATH "${ULTRALEAP_SDK}/lib"
)
</code></pre>

<h3>Step 7 — Open the correct folder in CLion</h3>
<ol>
  <li>CLion → <strong>File</strong> → <strong>Open</strong></li>
  <li>Select <code>~/Dev/leap_clion</code> (the folder containing <code>CMakeLists.txt</code>)</li>
  <li>“Trust project / Load CMake” → <strong>Yes</strong></li>
</ol>

<p><strong>Important:</strong> Don’t open <code>~/Dev</code> (parent folder) or CLion may not find <code>main.c</code> correctly.</p>

<h3>Step 8 — Build and run</h3>
<p>Build and run from CLion. 🎉</p>

<hr />

<h2>Troubleshooting</h2>

<h3>Error: LeapC.h file not found</h3>
<p><strong>Fix:</strong></p>
<ul>
  <li>Check your <code>target_include_directories()</code> path.</li>
  <li>Confirm <code>LeapC.h</code> exists inside the app bundle.</li>
</ul>

<h3>Error: Library not loaded: @rpath/libLeapC</h3>
<p><strong>Fix:</strong></p>
<ul>
  <li>Keep <code>BUILD_RPATH</code> (preferred).</li>
  <li>Or set an env var in CLion Run config:</li>
</ul>
<pre><code>DYLD_LIBRARY_PATH=/Applications/Ultraleap Hand Tracking.app/Contents/LeapSDK/lib
</code></pre>

<h3>Error: “Permission denied” when compiling inside /Applications</h3>
<p><strong>Fix:</strong></p>
<ul>
  <li>Copy samples into your home folder (<code>~/Dev/...</code>) like we did.</li>
  <li>Or allow CLion to modify app locations (not recommended).</li>
</ul>

<h3>Error: x86_64 vs arm64</h3>
<p><strong>Fix:</strong></p>
<ul>
  <li>Make sure you’re not compiling under Rosetta.</li>
  <li>CLion should be Apple Silicon build.</li>
  <li>Don’t link an x86_64-only dylib.</li>
</ul>


