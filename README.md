# SAT0RUEnhanced
# 呪術廻戦 — JJK: Cursed Technique

> A real-time hand gesture-driven cursed energy visualizer built with Three.js + MediaPipe Hands.

---

## Credits

| Role | Author |
|---|---|
| **Original Creator** | [reinesana](https://github.com/reinesana) |
| **Original Repository** | [github.com/reinesana/SAT0RU](https://github.com/reinesana/SAT0RU) |
| **Enhanced By** | shizuadev |

---

## Overview

This project uses your webcam and MediaPipe's real-time hand tracking to detect specific finger positions, then triggers particle-based visual effects inspired by the cursed techniques from *Jujutsu Kaisen*. Each technique has a unique particle formation, rotation behavior, bloom intensity, and color palette — all rendered in WebGL via Three.js with post-processing bloom.

---

## Features

- **13 unique cursed techniques** — each with distinct particle geometry, color, and motion
- **Real-time hand gesture detection** via MediaPipe Hands (no server needed, runs fully in-browser)
- **Adaptive performance mode** — auto-detects low-end hardware, cuts particle count and resolution accordingly
- **Gesture debouncing** — prevents flickering by requiring 5 consistent frames before switching techniques
- **Cursed Energy bar** — visual output meter per technique
- **Fully mirrored camera** — natural mirror-view for the user
- **No dependencies to install** — single HTML file, loads everything from CDN

---

## Hand Signs Reference

| Gesture | Technique | Color |
|---|---|---|
| Index finger only | Reverse CT: **Red** | 🔴 `#ff3333` |
| Index + Middle | Cursed Technique: **Blue** | 🔵 `#4488ff` |
| Pinch (index + thumb) | Secret Technique: **Hollow Purple** | 🟣 `#cc44ff` |
| All 4 fingers up | Domain Expansion: **Malevolent Shrine** | 🔴 `#ff2200` |
| Closed fist | Domain Expansion: **Infinite Void** | 🩵 `#aaddff` |
| Rock sign (index + pinky) | Cursed Tool: **Dragon Bone Katana** | 🟢 `#00ff88` |
| Thumb only | Ten Shadows: **Divergence Burst** | 🟠 `#ffaa00` |
| Index + Middle + Ring | Reverse CT: **Cursed Flower** | 🩷 `#ff69b4` |
| Spread all (wide) | Unlimited: **Star Rage** | ⚪ `#ffffff` |
| Middle + Ring only | Convergence: **Supernova Blood Pond** | 🟡 `#ffccaa` |
| Index + Ring only | Projection Sorcery: **Ratio Technique** | 🩵 `#00ffdd` |
| Index + Middle + Pinky | Maximum: **Meteor Furnace** | 🟠 `#ff6600` |

---

## Technical Stack

| Library | Version | Purpose |
|---|---|---|
| [Three.js](https://threejs.org) | r160 | WebGL rendering, particle system |
| [MediaPipe Hands](https://mediapipe.dev) | Latest | Real-time hand landmark detection |
| UnrealBloomPass | (Three addons) | Post-processing glow / bloom |
| MediaPipe Camera Utils | Latest | Webcam stream management |

---

## How It Works

### Particle System

Each technique precomputes `COUNT` particle target positions (`tp[]`), colors (`tc[]`), and sizes (`ts[]`) into typed arrays. Every animation frame, the live particle arrays lerp toward their targets:

```js
pos[i] += (tp[i] - pos[i]) * LERP;
```

This gives the smooth "formation" transition between techniques without any physics engine.

### Gesture Detection

MediaPipe returns 21 hand landmarks per hand. Finger state is determined by comparing tip Y position to the proximal knuckle:

```js
const isUp = (tip, base) => landmark[tip].y < landmark[base].y;
```

Pinch is measured as Euclidean distance between index tip and thumb tip.

### Adaptive Performance

On load, hardware concurrency and user agent are checked:

```js
const isLowEnd = navigator.hardwareConcurrency <= 4 || isMobile;
const COUNT = isLowEnd ? 5000 : 12000;
```

Pixel ratio is also capped at 1.5x and MediaPipe model complexity drops to 0 on low-end devices.

---

## Running Locally

No build step needed. Just open the HTML file in a browser:

```bash
# Option 1: Direct open (may have camera permission issues on some browsers)
open jjk_cursed_technique.html

# Option 2: Serve locally (recommended)
npx serve .
# or
python -m http.server 8080
```

Camera access requires HTTPS or localhost. Use a local server if opening directly doesn't trigger camera permission.

---

## Browser Compatibility

| Browser | Support |
|---|---|
| Chrome / Edge | ✅ Full support |
| Firefox | ✅ Works (slightly lower perf on bloom) |
| Safari | ⚠️ Limited (WebGL blend mode quirks) |
| Mobile Chrome | ✅ Works (auto eco mode) |

---

## Enhancements by shizuadev

The following changes were made on top of the original `SAT0RU` project by reinesana:

- Added 8 new techniques: Blue, Dragon, Ten Shadows, Reversal, Star Rage, Blood Pond, Projection Sorcery, Meteor Furnace
- Adaptive performance system for old / low-end machines
- Gesture debouncing (5-frame confirmation before switching)
- Mirrored canvas overlay for natural camera view
- Cursed Energy output bar UI
- Frame limiter (30/60 FPS target based on hardware)
- Reduced camera resolution on eco mode (320×240 vs 640×480)
- Responsive layout with `clamp()` sizing for all screen sizes
- Loading overlay with animated progress bar
- On-screen hand sign reference guide
- Credits panel
- Cleaner screen shake (amplitude tied to bloom strength)

---

## License

This project is based on open-source work by [reinesana](https://github.com/reinesana).  
All Jujutsu Kaisen intellectual property belongs to Gege Akutami / Shueisha.  
This is a fan project for educational and creative purposes only.
