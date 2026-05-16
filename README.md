# Crackathon — Nagpur Road Inspector (Frontend)

Interactive road inspection dashboard for Nagpur, built on top of the [Crackathon](https://github.com/Mangesh46/crackathon) YOLOv8 model. Deployed on GitHub Pages.

**🔗 Live:** https://mangesh46.github.io/crackathon-frontend/nagpur_inspector.html

---

## What this is

A browser-based inspection tool that lets you step through 75 GPS-tagged road locations across Nagpur (8 zones), pull Mapillary street-level imagery for each point, and run YOLOv8 crack detection — all in one view.

The model runs on a Hugging Face Space backend. The frontend is a single HTML file served via GitHub Pages. Secrets are never stored in this repository — they are injected into the HTML at deploy time by GitHub Actions.

---

## Features

- **75 inspection points** across 8 Nagpur zones — Central, North, N-East, East, South, S-West, West, Ring Road
- **Interactive Leaflet map** — colour-coded markers update live as points are scanned
- **Mapillary integration** — fetches real street-level imagery for each GPS point
- **Auto Scan** — automatically cycles through all unvisited points, fetches imagery, and runs detection
- **Zone filter** — inspect one zone at a time or run a full city sweep
- **Coverage panel** — live progress bars per zone: clean / damaged / remaining counts
- **Bounding box overlay** — detected cracks drawn on the image with class labels and confidence scores
- **Condition score** — 0–100 severity score with repair recommendation per point

---

## Secrets & security

Credentials are **never committed** to this repo. They live in:

| Where | Secret | Used for |
|---|---|---|
| GitHub → Settings → Secrets → Actions | `MAPILLARY_TOKEN` | Fetching street-level imagery |
| GitHub → Settings → Secrets → Actions | `BACKEND_URL` | HF Space inference API endpoint |
| HF Space → Settings → Variables and secrets | `MAPILLARY_TOKEN` | API proxying (optional) |

The GitHub Actions deploy workflow replaces `__MAPILLARY_TOKEN__` and `__BACKEND_URL__` placeholders in the HTML before pushing to `gh-pages`. The source file in `main` always contains placeholders only.

---

## Repo structure

```
crackathon-frontend/
├── nagpur_inspector.html      # Single-file app (placeholders for secrets)
└── .github/
    └── workflows/
        └── deploy.yml         # Injects secrets → deploys to gh-pages
```

---

## How to deploy your own copy

1. Fork this repo
2. Add two GitHub Actions secrets under **Settings → Secrets → Actions**:
   - `MAPILLARY_TOKEN` — get a free token at [mapillary.com/app/settings/developers](https://www.mapillary.com/app/settings/developers)
   - `BACKEND_URL` — your Hugging Face Space URL, e.g. `https://your-username-crackathon-api.hf.space`
3. Push any change to `main` (or trigger manually via Actions tab)
4. GitHub Pages will serve the injected HTML from the `gh-pages` branch

---

## Backend

The inference API lives in the main [Crackathon repo](https://github.com/Mangesh46/crackathon) and is deployed as a Docker Space on Hugging Face. It exposes:

| Method | Path | Description |
|---|---|---|
| `GET` | `/health` | Liveness check |
| `POST` | `/detect-url` | Send Mapillary image URL → YOLOv8 detections |
| `POST` | `/detect` | Upload image file → detections |

---

## Author

**Mangesh Sarde & Atharva Chapekar** — Team sardemv  
Crackathon, IIT Bombay
