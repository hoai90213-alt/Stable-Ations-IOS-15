<p align="center">
  <img src="icon.png" width="120" alt="Stable Action app icon" />
</p>

<h1 align="center">Stable Action</h1>

<p align="center">
  <strong>Samsung Super Steady Horizon Lock - rebuilt for iPhone.</strong><br/>
  Samsung's Galaxy S26 Ultra has it. Now iOS does too.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-iOS%2015%2B-black?style=flat-square&logo=apple" />
  <img src="https://img.shields.io/badge/Language-Swift-orange?style=flat-square&logo=swift" />
  <img src="https://img.shields.io/badge/Mode-Action%20%7C%20Normal-blue?style=flat-square" />
  <img src="https://img.shields.io/badge/Stabilisation-Horizon%20Lock-green?style=flat-square" />
</p>

<p align="center">
  <img src="demo.gif" width="340" alt="Stable Action demo" />
</p>

---

## Overview

Samsung's Galaxy S26 Ultra ships a feature called **Super Steady with Horizon Lock** that keeps your footage perfectly level no matter how much you tilt or shake the phone. It's one of the most impressive stabilisation tricks in mobile cameras right now - and it was exclusive to Android.

**Stable Action brings that exact same experience to iOS.** Point it at anything, tilt your wrist, run, jump, ride - the horizon stays locked flat, just like on the S26 Ultra. Built entirely in native Swift, runs 100% on-device, no subscriptions, no cloud.

---

## Inspired by Samsung Super Steady Horizon Lock

The Galaxy S26 Ultra's Horizon Lock works by cropping into the sensor and counter-rotating that crop window against the phone's tilt in real time. The result is a video where the horizon never moves, even when the phone is spinning in your hand.

Stable Action replicates this using the exact same principle - gyroscope-driven counter-rotation, a floating crop window inside a larger sensor buffer, and hardware ISP stabilisation layered on top. It also adds **translation correction** (up/down/left/right drift compensation) that Samsung's version doesn't publicly advertise.

---

## Features

| | Feature | Details |
|---|---|---|
| 🎯 | **Horizon Lock** | Gyroscope-driven crop counter-rotation at 120 Hz |
| 📐 | **Translation Correction** | Accelerometer-based X/Y drift stabilisation |
| 🔧 | **Hardware Stabilisation** | ISP `.cinematicExtendedEnhanced` in Action Mode |
| 👁 | **Preview = Recording** | Same pipeline for both - no surprises |
| 📷 | **Normal Mode** | Full-frame, no crop, standard camera feel |
| 🎬 | **Action Mode** | Full Horizon Lock + translation + aggressive ISP |
| 🎯 | **Tap to Focus** | Lock focus and exposure to any point on screen |
| 💾 | **Auto Save** | Saves to Photos library automatically on stop |

---

## How it works

### The horizon rectangle

In Action Mode you're not seeing the full sensor frame. You're looking through a cropped **3:4 portrait window** floating in the middle of the sensor output. That window is intentionally smaller than the full frame - the surrounding space is the buffer the stabilisation uses to rotate and shift without ever hitting the edge.

Picture a picture frame floating inside a larger canvas. When the canvas tilts, the frame stays upright.

### Two corrections, every frame

**Roll correction** - the phone tilts, the crop window rotates the opposite direction by the exact same amount. They cancel out. The horizon stays flat. This is the core of what Samsung calls Horizon Lock.

**Translation correction** - the phone jerks sideways or up/down, the crop window slides the opposite way to compensate. The accelerometer detects the movement, integrates it into a velocity, and shifts the window against it. When movement stops, the window drifts back to centre automatically.

### iOS 15 compatibility mechanism

To keep the same behavior on iOS 15, Stable Action uses:

- **Deployment target 15.0** in project and target settings.
- **Continuous roll unwrapping** before smoothing, so crossing the `-π/π` angle boundary does not cause sudden rotation flips.
- **iOS 15-safe APIs** in UI/video code (for example, tap handling and thumbnail generation paths).
- **Availability gates** for newer stabilization modes (newest mode on iOS 18+, fallback mode on lower versions).

### Hardware on top

Stable Action also engages the iPhone's built-in ISP stabilisation on every frame. Hardware cleans up micro-jitter - hand tremor, footstep impact, engine vibration. Software handles the macro roll. The two layers stack.

### Normal vs Action Mode

| | Normal Mode | Action Mode |
|---|---|---|
| **Preview** | Full sensor frame | Stabilised 3:4 crop |
| **Roll correction** | ✗ | ✓ |
| **Translation correction** | ✗ | ✓ |
| **Hardware ISP** | `.auto` | `.cinematicExtendedEnhanced` |
| **Recorded area** | Full frame | Stabilised crop only |

---

## Usage

```
1. Open the app                    → camera starts in Normal mode
2. Toggle Action Mode              → Horizon Lock pipeline activates
3. Tilt the phone                  → horizon stays locked flat
4. Tap anywhere                    → locks focus + exposure to that spot
5. Press the red button            → starts recording (REC pulses at top)
6. Press again                     → stops, saves to Photos automatically
7. Tap the thumbnail (bottom-left) → plays back the last clip in-app
```

---

## Requirements

- iPhone with iOS 15 or later
- iOS 18 or later for best Action Mode stabilisation
- Physical device required - camera and gyroscope unavailable in Simulator

---

## CI And Release

- GitHub Actions builds on `macos-latest`.
- The workflow creates an **unsigned** IPA and uploads it as artifact.
- The same IPA is published to a rolling pre-release tag: **`ci-latest`**.
- Old workflow runs are auto-cleaned (latest 5 runs are kept).

To download the newest IPA, open the repository Releases page and pick the latest asset from `ci-latest`.

---

## Permissions

| Permission | Why |
|---|---|
| **Camera** | To capture video |
| **Microphone** | To record audio alongside video |
| **Photos** | To save finished clips to your library |

---

## Support

If you find this project useful, consider buying me a coffee ☕

<p align="center">
  <a href="https://buymeacoffee.com/rudrashah">
    <img src="https://img.shields.io/badge/Buy%20Me%20a%20Coffee-rudrashah-FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black" alt="Buy Me A Coffee" />
  </a>
</p>

---

## Credits

Designed and built by **[Rudra Shah](https://rudrahsha.in)**

---

<p align="center">
  <sub>Built for iPhone · Inspired by Samsung Galaxy S26 Ultra Super Steady Horizon Lock</sub>
</p>
