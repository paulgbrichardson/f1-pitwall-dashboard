# 🏎️ Formula 1 Pit Wall Dashboard for Home Assistant

> Vibe coded with Antigravity.  Code has been reviewed and could probably be refactored in places.  Tested on both desktop and mobile app, happy for feedback and issues

[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2024.1%2B-blue.svg)](https://www.home-assistant.io)
[![HACS](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/default)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

A complete, production-tested **Formula 1 Pit Wall Dashboard** for Home Assistant Lovelace dashboards, designed to match the official **Formula 1 Weekend Hub** carbon chassis aesthetic.

Built with real-time race engineering in mind, this layout unites continuous 60 FPS race control announcements, live driver timing sheets, real-time GPS track position telemetry, broadcast delay calibration, and session replay time travel into a cohesive, responsive dashboard.

---

## 📸 Dashboard Previews

### Desktop Pit Wall View
![F1 Pit Wall Desktop View](screenshots/desktop-pitwall.png)

### Mobile App View
![F1 Pit Wall Mobile View](screenshots/mobile-pitwall.png)

### Live vs. Replay Synchronisation Deck
![F1 Live vs Replay Mode](screenshots/replay-live-controls.png)

*(To add your own screenshots, place your images in the [`screenshots/`](screenshots/) directory).*

---

## 🏗️ Architecture & Sections Breakdown

The dashboard uses Home Assistant's modern **Sections** layout engine (`type: sections`) for responsive reflow across multi-monitor pitwalls and mobile phones:

### 1. Section 0: Full-Width Continuous Race Control Ticker
- **Card**: [`paulgbrichardson/f1-ticker-card`](https://github.com/paulgbrichardson/f1-ticker-card)
- **Features**: Ultra-smooth 60 FPS marquee ticker tracking race director messages, safety cars, track limits warnings, fastest laps, DRS battles, pit stops, and weather.
- **Integrated Spoiler Protection**: Direct `switch.f1_no_spoiler_mode` toggle button built into the ticker header, saving vertical screen real-estate.

### 2. Section 1: Full-Width Live Timings Sheet
- **Card**: `custom:f1-sensor-card` (from [`Nicxe/f1_sensor`](https://github.com/Nicxe/f1_sensor))
- **Features**: Live positions, driver badges, gap to leader, sector 1/2/3 colors (purple, green, yellow), and tyre compound rings.
- **Weekend Hub Carbon Chassis**: Customized via `styles: |` with a 4px racing red left indicator stripe, triple-gradient carbon chassis, tabular numeric typography, and dark-themed dropdown popups.

### 3. Section 2: Live Track Map & Mode Switching Deck
- **Column 1**: `custom:f1-track-map-card` displaying live driver GPS coordinates around the circuit.
- **Column 2**: Dynamic `vertical-stack` featuring:
  - **Mode Switcher**: [`paulgbrichardson/f1-mode-toggle-card`](https://github.com/paulgbrichardson/f1-mode-toggle-card) — 46px segmented capsule button with live telemetry dots (`● LIVE SESSION`, `● REPLAY RUNNING`, `● [SEEKING]`).
  - **Live Mode Active**: Displays [`paulgbrichardson/f1-live-control-card`](https://github.com/paulgbrichardson/f1-live-control-card) for real-time broadcast delay offset (`number.f1_live_delay`), quick delta nudges, and one-tap TV delay locking (`button.f1_match_live_delay`).
  - **Replay Mode Active**: Displays the modular replay deck (`custom:f1-sensor-card`) with seek controls, transport buttons, and progress sliders.

---

## 📦 Required Plugins & Integrations

Install the following components from HACS:

| Component | Type | Source |
| :--- | :--- | :--- |
| **Formula 1 Sensor** | Integration | [`Nicxe/f1_sensor`](https://github.com/Nicxe/f1_sensor) |
| **F1 Race Control Ticker Card** | HACS Plugin | [`paulgbrichardson/f1-ticker-card`](https://github.com/paulgbrichardson/f1-ticker-card) |
| **F1 Live Control Card** | HACS Plugin | [`paulgbrichardson/f1-live-control-card`](https://github.com/paulgbrichardson/f1-live-control-card) |
| **F1 Session Mode Toggle Card** | HACS Plugin | [`paulgbrichardson/f1-mode-toggle-card`](https://github.com/paulgbrichardson/f1-mode-toggle-card) |
| **card-mod** | HACS Plugin | [`thomasloven/lovelace-card-mod`](https://github.com/thomasloven/lovelace-card-mod) |

---

## 🚀 Quick Start Setup

### Method 1: Import Full Dashboard
1. Copy the contents of [`f1-pitwall.yaml`](f1-pitwall.yaml).
2. In Home Assistant, navigate to **Settings** → **Dashboards** → **Add Dashboard**.
3. Choose **New dashboard from scratch**, set the URL to `f1-pitwall`, and switch to the **Raw configuration editor** (three dots menu).
4. Paste the YAML and save.

### Method 2: Styling Existing F1 Sensor Cards
If you already have `custom:f1-sensor-card` installed and want to apply the Weekend Hub carbon chassis with dark dropdown styling:
1. Open [`modular-sensor-styles.yaml`](modular-sensor-styles.yaml).
2. Copy the `appearance:`, `styles:`, and `card_mod:` blocks into your card configuration.

---

## 🎨 Design Tokens & Custom CSS

The card theme uses the official **Weekend Hub** styling tokens:
- **Carbon Chassis**: Triple-gradient dark finish (`linear-gradient(110deg...)`, `radial-gradient(...)`, `linear-gradient(155deg...)`).
- **Indicator Stripe**: 4px racing red left edge (`#ff3b30`).
- **Typography**: Authentic `Formula1 Display` with negative tracking.
- **Form Controls Fix**: `color-scheme: dark !important;` and shadow DOM piercing via `card-mod` (`f1-module-view$ select option`) to eliminate white-on-white native dropdown popups.

---

## 📄 License

MIT © [Paul](LICENSE)
