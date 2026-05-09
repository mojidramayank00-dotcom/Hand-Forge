# ◈ HAND FORGE — Gesture-Controlled 3D Interface

> A fullscreen, zero-dependency web app that lets you control a 3D model embedded via Sketchfab using nothing but hand gestures detected through your webcam.

![HAND FORGE](https://img.shields.io/badge/HAND-FORGE-orange?style=flat-square&labelColor=000&color=ff6b2b)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Hands-blue?style=flat-square&labelColor=000&color=00d4ff)
![CSS 3D](https://img.shields.io/badge/CSS-3D_Transforms-white?style=flat-square&labelColor=000&color=39ff14)
![No Build](https://img.shields.io/badge/No_Build-Required-white?style=flat-square&labelColor=000&color=fff)
![Single File](https://img.shields.io/badge/Single-File-white?style=flat-square&labelColor=000&color=fff)

---

## ✦ What It Does

Hold your fist up and drag a 3D model across the screen. Point your index finger and rotate it in any direction. Pinch your fingers to scale it up or down. Flash a peace sign to snap it back to default. Throw a Shaka and watch it spin. All in real time, no mouse, no touch — just your hands.

The 3D model (a Sketchfab embed) is controlled via CSS 3D perspective transforms applied to its wrapper container, giving a convincing 3D response without needing any API access to the iframe.

---

## ✦ Gesture Reference

| Gesture | Name | What It Does |
|---|---|---|
| ✊ | **FIST** | Grab — freeze & hold. Move wrist to nudge the model across the screen |
| 🖐 | **PALM** | Release — return to idle |
| 👆 | **POINT** | Index fingertip delta drives rotateX / rotateY in real time |
| 🤏 | **PINCH** | Distance between thumb + index tip controls scale |
| ✌️ | **PEACE** | Smooth spring reset to default transform |
| 🤙 | **SHAKA** | Triggers a 720° spin CSS keyframe animation |

---

## ✦ Features

| Feature | Description |
|---|---|
| **Real-time hand tracking** | MediaPipe Hands via CDN, ~30fps inference loop |
| **6 distinct gestures** | Classified per-frame from 21 hand landmarks |
| **CSS 3D transform control** | rotateX, rotateY, scale, translateX, translateY |
| **Adjustable sensitivity** | Live sliders for rotate, scale, and move speed |
| **Landmark skeleton overlay** | Color-coded skeleton drawn on webcam feed |
| **Miniature landmark map** | Normalized hand map in the right panel |
| **Live transform readouts** | Real-time degrees, scale factor, pixel offset |
| **Quick action buttons** | Reset, Spin, Flip X, Flip Y via buttons too |
| **Toast notifications** | Gesture event confirmations |
| **Futuristic HUD** | Industrial dark UI with ember + cyan palette |

---

## ✦ Quick Start

**No install. No build. One file.**

### Option A — Run locally

```bash
git clone https://github.com/YOUR_USERNAME/hand-forge.git
cd hand-forge

# HTTPS required for camera — serve with any of these:
npx serve .
# or
python3 -m http.server 8080
# then open https://localhost:8080 (not http://)
```

> ⚠️ Camera access requires either `localhost` or an HTTPS origin.
> A plain `file://` URL will be blocked by the browser.

### Option B — GitHub Pages (recommended)

1. Push to GitHub
2. **Settings → Pages → Source: main branch / root**
3. GitHub Pages serves over HTTPS automatically ✓

### Option C — Netlify / Vercel / Cloudflare Pages

Drop the folder — zero config required. All serve HTTPS by default.

---

## ✦ Swapping the 3D Model

Find this comment block inside `index.html` and change the `src`:

```html
<!-- ═══════════════════════════════════════════════════════════════
     SWAP THIS IFRAME SRC TO EMBED YOUR OWN SKETCHFAB MODEL
     Sketchfab embed URL format:
     https://sketchfab.com/models/YOUR_MODEL_ID/embed
═══════════════════════════════════════════════════════════════ -->
<iframe src="https://sketchfab.com/models/YOUR_MODEL_ID/embed?autostart=1">
</iframe>
```

You can also replace the iframe entirely with a `<model-viewer>` element for self-hosted GLB files:

```html
<script type="module" src="https://ajax.googleapis.com/ajax/libs/model-viewer/3.3.0/model-viewer.min.js"></script>
<model-viewer src="your-model.glb" auto-rotate camera-controls style="width:100%;height:100%;">
</model-viewer>
```

The CSS 3D transforms on the wrapper div will work identically with either embed method.

---

## ✦ Why CSS Transforms Instead of Sketchfab API

The Sketchfab iframe is served from a different domain (`sketchfab.com`), which means the browser's **same-origin policy** prevents JavaScript from reaching inside it to call any 3D engine functions. This is a fundamental browser security rule — not a Sketchfab limitation.

The solution: wrap the iframe in a container with `perspective: 1200px` and apply `rotateX`, `rotateY`, `scale`, and `translate` CSS transforms to the wrapper. This transforms the entire iframe as if it were a flat card in 3D space, giving a genuine and responsive 3D feel.

---

## ✦ Tech Stack

| Technology | Role |
|---|---|
| [MediaPipe Hands](https://developers.google.com/mediapipe) | ML hand landmark detection (CDN, no install) |
| HTML5 Canvas 2D | Webcam skeleton overlay + landmark minimap |
| CSS3 Transforms | `perspective`, `rotateX/Y`, `scale`, `translate` |
| CSS Keyframes | Spin animation (`@keyframes spinY`) |
| Vanilla JavaScript | All logic — gesture detection, state, UI |
| WebRTC `getUserMedia` | Camera stream |
| Google Fonts (CDN) | Bebas Neue, Share Tech Mono, Rajdhani |

---

## ✦ File Structure

```
hand-forge/
└── index.html    ← entire app, single self-contained file
└── README.md
```

---

## ✦ Customization

All key parameters are at the top of the `<script>` block and exposed as live sliders in the UI:

```js
let rotSens = 1.8;   // how much finger movement rotates the model
let scSens  = 2.0;   // how much pinch gap changes scale
let mvSens  = 1.0;   // how much wrist movement translates the model
```

MediaPipe settings:
```js
hands.setOptions({
  maxNumHands:            1,
  modelComplexity:        1,
  minDetectionConfidence: 0.72,
  minTrackingConfidence:  0.55
});
```

---

## ✦ Browser Support

| Browser | Status |
|---|---|
| Chrome 90+ | ✅ Full support |
| Edge 90+ | ✅ Full support |
| Firefox | ⚠️ MediaPipe WASM may have issues |
| Safari | ⚠️ Camera API restricted on some versions |
| Mobile Chrome | ✅ Works (front camera) |

---

## ✦ Credits

- 3D Model: [Corrupted Dark Forest Ent](https://sketchfab.com/3d-models/corrupted-dark-forest-ent-ac6f9e153cea4e20998cea31338ab8d6) by Pigcraft on Sketchfab
- Hand tracking: [MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker) by Google

---

## ✦ License

MIT — do whatever you want with it.

---

<p align="center">
  Built with ◈ and <code>CSS perspective</code>
</p>
