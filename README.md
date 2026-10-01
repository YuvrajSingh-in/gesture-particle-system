# Gesture-Controlled 3D Particle System

Real-time hand tracking driving a Three.js particle field in the browser. No build step, no install — open the file and allow the camera.

Your webcam feed goes to **MediaPipe Hands**, which returns 21 hand landmarks per frame. Those landmarks are mapped to parameters of a **Three.js** particle system, so the geometry responds to your hand as you move it.

---

## Controls

| Gesture | Effect |
|---|---|
| Pinch fingers together | Expand or contract the particle field |
| Make a fist | Morph the particles into a different shape |
| Move your hand | Shift the colour of the field |

---

## Running it

```bash
# Any static server will do — the camera API requires http/https, not file://
python -m http.server 8000
```

Then open <http://localhost:8000> and allow camera access when prompted.

---

## How it works

```
webcam  ──►  MediaPipe Hands  ──►  21 landmarks/frame  ──►  Three.js particle system
                                        │
                                        ├─ thumb–index distance  →  field radius
                                        ├─ fist detection        →  target geometry
                                        └─ palm position         →  hue
```

Everything lives in a single `index.html`. Libraries load from CDN:

- [`@mediapipe/hands`](https://google.github.io/mediapipe/solutions/hands) — hand landmark detection
- [`@mediapipe/camera_utils`](https://google.github.io/mediapipe/) — webcam frame pump
- [`three.js` r128](https://threejs.org/) — rendering

---

## Notes

The video preview is mirrored (`transform: scaleX(-1)`) so moving your hand right moves the effect right, which is what people expect from a mirror rather than a camera.

Tracking quality depends on lighting and on your hand staying inside the frame. MediaPipe runs entirely client-side — no video leaves the browser.
