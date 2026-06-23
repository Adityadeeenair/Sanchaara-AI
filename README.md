<div align="center">

# 🚦 Sanchaara AI (ಸಂಚಾರ AI)

**AI-Powered Parking Enforcement Intelligence for Bengaluru**

*Built for the Bengaluru Traffic Police · Flipkart Gridlock Hackathon 2.0 — Round 2*

<br />

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://sanchaara-ai.streamlit.app)
[![GitHub](https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/SohanVarkhedi/sanchaaraai)
[![License](https://img.shields.io/badge/License-MIT-F0A000?style=for-the-badge)](LICENSE)

<br />

> *Predict. Prioritise. Enforce.*

<br />

</div>

---

## ⚠️ The Problem

Illegal parking in dense urban corridors (commercial zones, metro junctions, transit hubs) blocks active lanes, chokes Bengaluru's roads, and delays emergency services. Traditional BTP enforcement is **reactive** — patrol-based, manual, and blind to where congestion pressure is actively building.

- 298,450 raw violation records exist, but none of it informs daily deployments.
- Officers patrol on intuition rather than spatial-temporal data.
- Low-impact zones get equal attention while high-impact critical areas are left underserved.

---

## ⚡ What Sanchaara AI Does

Sanchaara AI is a **Traffic Enforcement Decision Support System** that turns raw parking violation records into actionable deployment intelligence — without requiring new cameras, sensors, or hardware.

```
Raw Violation Data  →  Hotspot Detection  →  Impact Scoring  →  Ranked Zones  →  Officer Deployment Plan
```

| Component | Function |
| :--- | :--- |
| **Detect** | DBSCAN spatial clustering filters coordinate noise and isolates 120 true hotspots from 115,400 records. |
| **Score** | Three-component impact scoring model weighs violation density, rush-hour timing, and vehicle severity. |
| **Rank** | 120 congestion zones ordered by real-time pressure — fully explainable, auditable, and transparent. |
| **Simulate** | Greedily allocates available officers to the highest-priority zones. Automatically dispatches tow trucks only to zones where heavy vehicle infractions exceed 20%. |

---

## 📊 Live Results

From 115,400 approved Bengaluru parking violations (Nov 2023 – Mar 2024):

| Metric | Value |
| :--- | :--- |
| **Hotspots Identified** | 120 |
| **Top Zone Impact Score** | 0.739 |
| **Violations in Top Zone** | 29,618 |
| **Officers Needed (AM Shift, Top Zone)** | 19 |
| **BTP Police Stations Covered** | 46 |
| **Bad/Unparseable Coordinates** | 0 |

### Top 3 Congestion Hotspots:

| Rank | Zone | Impact Score | Total Violations | Officers Needed |
| :---: | :--- | :---: | :---: | :---: |
| **01** | BTP040 — Elite Junction, Upparpet | **0.739** | 29,618 | 19 |
| **02** | BTP051 — Safina Plaza, Shivajinagar | **0.712** | 9,946 | 7 |
| **03** | BTP020 — Hosahalli Metro, Vijayanagara | **0.673** | 17,747 | 7 |

---

## 📐 Impact Scoring Formula

Hotspots are ranked using a three-component weighted formula:

$$\text{Impact Score} = 0.5 \times \text{Volume}_{\text{norm}} + 0.3 \times \text{RushHour}_{\text{frac}} + 0.2 \times \text{Severity}_{\text{norm}}$$

- **Volume Norm**: Log-normalized violation count, preventing single massive clusters from completely skewing the results.
- **Rush-Hour Frac**: Share of violations occurring during active BTP enforcement hours (IST 07:00–12:00 or 17:00–20:00).
- **Severity Norm**: Average vehicle weight mapping from 0.2 (two-wheelers) to 1.0 (HGVs, buses, and container tankers).

---

## 🌐 Bilingual Support & Platform Features

### 1. English / Kannada Translation Engine
Sanchaara AI features a custom client-side localization module (`i18n.js`) with persistent `localStorage` synchronization. Changing languages on the landing page automatically syncs to the dashboard:
* **Dynamic Entities**: Translates maps, police station dropdown filters (preserving underlying lookup bindings), Chart.js tooltips, priority lists, and simulator tables on-the-fly.
* **Kannada Support**: Custom tailored for local Bengaluru Traffic Police preferences.

### 2. Strict Map Interaction & Zoom Locks
* Both the main dashboard map (`#leaflet-map`) and simulator map (`#sim-map`) are locked between `minZoom: 11` and `maxZoom: 16` and bounded strictly within Bengaluru city limits (`maxBounds`).
* Mouse dragging, keyboard panning, and scrolling are temporarily disabled during the landing intro flight to prevent manual zoom-outs before the map settles.

---

## 🛠️ Tech Stack & Directory Structure

```
Sanchaara-AI-main/
├── .gitignore
├── .streamlit/
│   └── config.toml
├── app.py
├── index.html
├── requirements.txt
├── serve.py
├── vercel.json
├── data/
│   ├── processed/
│   │   └── hotspots.json
│   └── raw/
│       └── .gitkeep
├── docs/
│   ├── decisions.md
│   └── tasks.md
├── frontend/
│   ├── index.html
│   ├── dashboard.html
│   ├── assets/
│   │   ├── bangalore_traffic_cctv_1_1781876572508.png
│   │   ├── bangalore_traffic_cctv_2_1781876595015.png
│   │   ├── bangalore_traffic_cctv_3_1781876616173.png
│   │   ├── bangalore_traffic_cctv_4_1781876647818.png
│   │   ├── bangalore_traffic_cctv_5_1781876681365.png
│   │   ├── bangalore_traffic_cctv_6_1781876731105.png
│   │   └── officer.png
│   ├── css/
│   │   ├── animations.css
│   │   ├── dashboard.css
│   │   ├── hero.css
│   │   ├── landing.css
│   │   └── theme.css
│   └── js/
│       ├── charts.js
│       ├── dashboard.js
│       ├── hero3d.js
│       ├── hotspots_data.js
│       ├── i18n.js
│       ├── landing.js
│       ├── map.js
│       └── simulator.js
└── src/
    ├── __init__.py
    ├── data_loader.py
    ├── hotspot_engine.py
    └── i18n.py

```

---

## 🚀 Setup & Local Execution

### Step 1: Install Python Requirements
```bash
pip install -r requirements.txt
```

### Step 2: Serve the Static Frontend Console
Serve the HTML/JS console dashboard locally (includes CORS setup for reading json):
```bash
python serve.py
```
* **Landing Page**: `http://localhost:8000/frontend/index.html`
* **Console Dashboard**: `http://localhost:8000/frontend/dashboard.html`

### Step 3: Run the Streamlit Operations Tool
In a separate terminal window, run the analytics application:
```bash
streamlit run app.py
```

---

## 🌐 Production Cloud Deployment

### 1. Static Frontend - Render (Full Platform)
The entire Sanchaara AI frontend (landing page + console dashboard + all assets) deploys as a static site on Render.
**To deploy:**
1. Push this repository to GitHub.
2. Go to [Render](https://render.com) → **New** → **Static Site**.
3. Connect your GitHub repository.
4. Set the following:
   - **Root Directory**: ` ` (leave blank — repo root)
   - **Build Command**: ` ` (leave blank — no build step)
   - **Publish Directory**: `frontend`
5. Click **Deploy**.

### 2. Streamlit Cloud
1. Push your master branch to GitHub.
2. Link your repository on [Streamlit Share](https://share.streamlit.io/).
3. Set the file entrypoint to `app.py` and deploy.

---

## 👥 SDG Alignment & Team

| Goal | How Sanchaara AI Contributes |
| :---: | :--- |
| **SDG 11** | Sustainable Cities — Reduces parking-induced congestion without new infrastructure. |
| **SDG 3** | Good Health & Well-being — Reduces engine-idling emissions in major corridors. |
| **SDG 16** | Strong Institutions — Data-driven routing replaces subjective patrol assignments. |

### Team: TEAM RDN
- **Sohan S Varkhedi** — Data / ML / Backend
- **Rahul C Araganji** — Full-Stack / Frontend
- **Aditya D Nair** — Backend / Scoring Logic

---

<div align="center">

*Built on 115,400 real Bengaluru traffic violations.*  
*No fabricated outcomes. No invented metrics.*

<br />

[![Live Demo](https://img.shields.io/badge/Open%20Sanchaara%20AI-Launch%20Platform-F0A000?style=for-the-badge)](https://sanchaara-ai.streamlit.app)

</div>
