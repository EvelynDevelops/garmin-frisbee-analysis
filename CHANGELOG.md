# Changelog

## v1.1.2 (2026-03-16)

### Security & Analysis Improvements

**Security:**
- Removed `config.example.json` — credentials must now be set via environment variables only (`GARMIN_EMAIL`, `GARMIN_PASSWORD`)
- Updated `install.sh` to guide users toward env var setup instead of config file
- Updated `SKILL.md` metadata to include `garmin_auth.py login` as an explicit install step

**New: Ground Contact Time Analysis (`frisbee_activity.py` + `frisbee_chart.py`):**
- Extracts `stance_time` from FIT files — average GCT displayed as a stat card
- GCT Trend Ratio: 2nd half vs 1st half average — rising GCT indicates late-game fatigue
- New GCT timeline chart in the activity dashboard (shown when data is available)

**New: Intensity Ceiling Chart (`frisbee_compare.py`):**
- New chart showing Avg HR + Max HR as grouped bars per activity
- Zone 4 (148 bpm) and Zone 5 (167 bpm) reference lines overlaid
- Answers "did I actually hit high-intensity zones?" at a glance
- Added `chartjs-plugin-annotation` to compare dashboard

**References:**
- Rewrote `references/extended_capabilities.md` to reflect actual script capabilities, new metrics, and GCT availability notes

---

## v1.0.0 (2026-03-11)

### Initial Release

Ultimate Frisbee performance analytics skill for Clawdbot, built on Garmin Connect data.

**Frisbee Scripts:**
- `frisbee_activity.py` — Post-game dashboard: sprint count, top speed, Sprint Fatigue Index, HR zone distribution, speed timeline with sprint highlight bands
- `frisbee_tournament.py` — Tournament review dashboard: Body Battery fatigue curve, per-game HR comparison, Heart Rate Recovery curves, overnight sleep/HRV
- `frisbee_compare.py` — Comparison & season analysis: training vs game intensity, top speed trends, volume over time (modes: training / tournament / cross / season)
- `frisbee_chart.py` — Frisbee-specific chart generation

**General Garmin Scripts:**
- `garmin_data.py` — Fetch health metrics (sleep, Body Battery, HRV, heart rate, activities, stress)
- `garmin_chart.py` — Generate interactive HTML health dashboards
- `garmin_data_extended.py` — Extended metrics (training readiness, VO2 max, body composition, SPO2, intensity minutes)
- `garmin_activity_files.py` — Download and parse FIT/GPX activity files
- `garmin_query.py` — Time-based queries ("what was my HR at 3pm?")
- `garmin_auth.py` — Authentication and token management

**Key Metrics:**
- Sprint detection: speed > 14.4 km/h sustained ≥ 2 seconds
- Sprint Fatigue Index: last 3 sprint peaks ÷ first 3 sprint peaks (< 0.85 = significant fatigue)
- Heart Rate Recovery curves post-game (30 min window)
- Body Battery pre-game readiness (≥ 75 = ready, ≥ 50 = manageable, < 50 = compromised)

**Configuration:**
- Clawdbot config (`skills.entries.garmin-frisbee-analysis.env`)
- Local `config.json` support
- Environment variables (`GARMIN_EMAIL`, `GARMIN_PASSWORD`)

**Requirements:**
- Python 3.7+
- garminconnect, fitparse, gpxpy
- Garmin Connect account with GPS-enabled wearable (tested on Garmin 265S)
