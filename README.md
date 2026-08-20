# 🤟 ASL Sign Language Recognizer

A complete web app for real-time American Sign Language (ASL) alphabet recognition
using your webcam. It detects 21 hand landmarks with MediaPipe (via
`@tensorflow-models/hand-pose-detection`) and classifies them into letters A–Z
with a lightweight K-Nearest-Neighbors classifier.

## Tech stack

- **React 18 + Vite** — frontend
- **TensorFlow.js + `@tensorflow-models/hand-pose-detection`** (MediaPipe Hands runtime) — hand landmark detection
- **Tailwind CSS** — dark, responsive styling
- **Custom KNN classifier** — `src/utils/knnClassifier.js` (ml5.js and `@tensorflow-models/knn-classifier` are deprecated and incompatible with current TF.js releases, so this app ships a ~40-line Euclidean-distance KNN instead)

## Getting started

```bash
npm install
npm run dev
```

Open the printed URL (default `http://localhost:5173`). A production build is
available via `npm run build` (then `npm run preview`).

## How to use

1. **Start Camera** — grants webcam access; the feed appears with the hand
   skeleton drawn on top in real time.
2. **Training Mode** — pick a letter (A–Z), hold up the sign, click
   **Capture Sample**. Repeat 8–12 times per letter with slight hand movement
   between captures. Training data is saved to `localStorage` automatically.
3. **Recognize Mode** — the predicted letter and confidence percentage update
   live. A letter above the confidence threshold held steady for ~1 second is
   appended to the **Sentence Builder** text.
4. Use **Space / Backspace / Clear** to edit the sentence.
5. Adjust the **confidence threshold** slider in Settings (lower = more
   permissive, higher = stricter).

## Notes

- The hand-landmark model (~7 MB) and MediaPipe WASM are fetched from the
  internet on first run, so an internet connection is required.
- MediaPipe Hands works best in Chrome or Edge on desktop and mobile (iOS Safari
  is supported via `playsinline`).
- Models persist per-browser via `localStorage` — no server required.

## Project structure

```
├── index.html
├── vite.config.js
├── tailwind.config.js
└── src/
    ├── App.jsx                      # layout, mode switching, prediction pipeline
    ├── main.jsx                     # React entry point
    ├── index.css                    # Tailwind directives
    ├── components/
    │   ├── CameraFeed.jsx           # webcam + landmark overlay canvas
    │   ├── TrainingPanel.jsx        # letter selector + capture sample
    │   ├── PredictionPanel.jsx      # live prediction + confidence bar
    │   ├── SentenceBuilder.jsx      # output text + Space/Backspace/Clear
    │   └── SettingsPanel.jsx        # confidence threshold slider
    ├── hooks/
    │   └── useHandDetection.js      # model loading, camera, rAF detection loop
    └── utils/
        └── knnClassifier.js         # KNN classifier + feature extraction + save/load
```
