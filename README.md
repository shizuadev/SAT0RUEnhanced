
# 呪術廻戦 — JJK: Cursed Technique Visualizer

<div align="center">

![JJK Banner](https://media.tenor.com/PcTVZK3aROQAAAAi/albanie.gif)

**A real-time hand gesture particle visualizer inspired by Jujutsu Kaisen**  
*Point your hand at the webcam. Activate cursed techniques.*

[![Original](https://img.shields.io/badge/Original-reinesana-ff3333?style=flat-square)](https://github.com/reinesana/SAT0RU)
[![Enhanced](https://img.shields.io/badge/Enhanced-shizuadev-00ffff?style=flat-square)](https://github.com/reinesana)
[![License](https://img.shields.io/badge/License-Fan%20Project-bb00ff?style=flat-square)](#license)

</div>

---

## Credits

| | |
|---|---|
| **Original Creator** | [reinesana](https://github.com/reinesana) |
| **Original Repository** | [github.com/reinesana/SAT0RU](https://github.com/reinesana/SAT0RU) |
| **Enhanced By** | **[shizuadev](https://github.com/shizuadev)** |

---

## Overview

Uses your webcam + MediaPipe Hands to detect finger gestures in real-time and trigger Three.js particle effects inspired by cursed techniques from *Jujutsu Kaisen*. Each technique has unique particle geometry, rotation behavior, bloom intensity, and color. Everything runs fully in-browser — no install, no server.

---

## Quick Start

No build step needed. Just open and run:

```bash
# Recommended: serve locally (camera needs localhost or HTTPS)
npx serve .
# or
python -m http.server 8080
```

Then open `http://localhost:8080/jjk_cursed_technique.html` in Chrome or Edge.

> **Note:** Direct file:// open may block camera access. Use a local server.

---

## All 19 Techniques

| Technique | Character | Gesture | Power |
|---|---|---|---|
| Reverse CT: Red | Gojo Satoru | Index finger only | 55% |
| Cursed Technique: Blue | Gojo Satoru | Index + Middle | 55% |
| Hollow Purple | Gojo Satoru | Pinch (index + thumb) | 95% |
| Domain: Infinite Void | Gojo Satoru | Relaxed closed fist | 80% |
| Infinity: Limitless | Gojo Satoru | Thumb only extended | 75% |
| Unlimited: Star Rage | Gojo Satoru | All 5 fingers spread | **100%** |
| Domain: Malevolent Shrine | Ryomen Sukuna | 4 fingers up (no thumb) | 90% |
| Sukuna CT: Dismantle | Ryomen Sukuna | Index + Middle + Pinky | 70% |
| Sukuna CT: Cleave | Ryomen Sukuna | Tight closed fist | 75% |
| Projection Sorcery | Nanami Kento | Index + Ring fingers | 45% |
| Straw Doll: Hairpin | Nobara Kugisaki | Ring finger only | 65% |
| Blood Pond Convergence | Choso | Middle + Ring fingers | 72% |
| Piercing Blood | Choso | Thumb + Index + Middle | 78% |
| Cursed Speech: Fallen | Toge Inumaki | Middle finger only | 68% |
| Rika: Spirit Manifestation | Yuta Okkotsu | Thumb + Index + Middle + Pinky | 92% |
| Maximum: Meteor Furnace | Jogo | Index + Middle + Pinky | 85% |
| Ten Shadows: Divergence | Megumi Fushiguro | Thumb half-extended | 65% |
| Cursed Tool: Dragon Bone | Tool User | Index + Pinky (rock sign) | 50% |
| Reverse CT: Cursed Flower | Shoko Ieiri | Index + Middle + Ring | 60% |

> **See `JJK_HandSign_Guide.pdf` for illustrated hand diagrams of every gesture.**

---

## How Gesture Detection Works

The detection system was rebuilt from the ground up for stability:

**Landmark smoothing** — raw MediaPipe landmarks are lerped between frames (α = 0.55) to eliminate hand jitter from causing false positives.

**Confidence scoring** — each finger gets a 0–1 confidence value based on PIP joint vs fingertip Y-delta, rather than binary up/down comparisons. The thumb uses X-axis lateral extension instead of Y.

**Voting buffer** — the last 8 frames are stored and weighted by confidence. A majority vote over the buffer determines the current gesture rather than reacting to single frames.

**Hysteresis** — switching *to* a new technique requires 5 consistent votes. Switching *back* to neutral requires 10 consecutive neutral frames. This prevents flicker when transitioning between gestures.

```
Raw landmarks → Lerp smoother → Per-finger confidence (0..1) → Classifier 
  → 8-frame history → Weighted vote → Hysteresis → activateTechnique()
```

---

## Performance Tiers

Auto-detected on load based on `navigator.hardwareConcurrency`:

| Tier | Cores | Particles | FPS | Camera | Antialiasing |
|---|---|---|---|---|---|
| LOW | ≤ 2 or mobile | 5,000 | 30 | 320×240 | Off |
| MID | 3–4 | 11,000 | 60 | 480×360 | Off |
| HIGH | 5+ | 20,000 | 60 | 640×480 | On |

---

## Stack

| Library | Version | Purpose |
|---|---|---|
| [Three.js](https://threejs.org) | r160 | WebGL particle rendering |
| [MediaPipe Hands](https://mediapipe.dev) | Latest | Real-time 21-point hand tracking |
| UnrealBloomPass | Three addons | Post-processing glow |
| MediaPipe Camera Utils | Latest | Webcam stream |

---

## What shizuadev Added

Built on top of the original 4-technique SAT0RU demo:

- **15 new techniques** (19 total) with unique particle shapes and rotations
- **Rebuilt gesture detection** — smoothing, confidence scoring, voting, hysteresis
- **3-tier performance system** — auto-detects hardware, adjusts particle count + resolution
- **Per-technique rotation speeds** — each technique rotates differently
- **Cursed energy output bar** — visual power meter per technique
- **Flash activation pulse** — brief screen flash on technique switch
- **Frame limiter** — targets 30 or 60 FPS based on tier, no wasted GPU cycles
- **`low-power` GPU hint** on tier 0
- **Camera mirror fix** — both video and canvas overlay correctly mirrored
- **Loading overlay** with animated progress bar
- **Removed on-screen guide panel** — see the PDF instead

---

## Browser Compatibility

| Browser | Support |
|---|---|
| Chrome / Edge | ✅ Full |
| Firefox | ✅ Works |
| Safari | ⚠️ Limited WebGL blend mode |
| Mobile Chrome | ✅ Auto eco mode |

---

## License

Fan project for educational and creative purposes.  
Jujutsu Kaisen © Gege Akutami / Shueisha.  
Original code © [reinesana](https://github.com/reinesana/SAT0RU).
