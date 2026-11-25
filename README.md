# Real-Time Drowsiness Detection System (MediaPipe + Google Colab)

Browser-based real-time drowsiness detection built with **MediaPipe Face Landmarker** (`@mediapipe/tasks-vision@1.2.3`) and hosted via **Google Colab** + **Cloudflare Quick Tunnel**.

> **Disclaimer:** Educational drowsiness estimation prototype. Not a certified driver-safety or medical monitoring system.

## Architecture

- **Zero video uploads:** Webcam frames stay in the user's browser (`getUserMedia` → `MediaPipe FaceLandmarker` WASM/WebGL with automatic CPU fallback → pixel-scaled 3-pair EAR & yaw guard → live Canvas HUD + Web Audio alarm).
- **Lightweight Colab host:** Python `ThreadingHTTPServer` serves the self-contained frontend and exposes a public HTTPS link through `cloudflared` (`trycloudflare.com`), with a Colab `proxyPort` fallback.
- **Time-based 4-state classifier:**
  - **ACTIVE** (Green): Eyes open normally.
  - **DROWSY** (Orange): Both eyes partially closed below `0.24` EAR for `>= 1.2 s` (ignores brief natural blinks).
  - **SLEEPING** (Red): Both eyes closed below `0.19` EAR for `>= 2.5 s`, triggering a Web Audio dual-tone alarm with cooldown (`5.0 s`) and active-recovery reset.
  - **NO FACE** (Grey): Face absent past the `500 ms` grace window.

## Project Files

- `Drowsiness_Detector_MediaPipe_Colab.ipynb` — Complete 5-cell Google Colab notebook (Cloudflare setup, embedded HTML/CSS/JS app + Python HTTP server, HTTPS tunnel launcher).
- `active.PNG`, `drawsy.PNG`, `sleeping.PNG`, `project_screenshot1.PNG` — Captured live application screenshots across states.
- `work_report/Minor_Project_Report.docx` — Complete Minor Project Report.

## How to Run in Google Colab

1. Upload `Drowsiness_Detector_MediaPipe_Colab.ipynb` to Google Colab.
2. Run **Step 1** (setup `cloudflared`), **Step 2** (create web app & server helper), and **Step 3** (launch server & public HTTPS URL).
3. Open the printed `https://...trycloudflare.com` link in your browser, click **Start Camera**, and allow webcam permission.
