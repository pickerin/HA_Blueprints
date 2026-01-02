# 🌬️ Home Assistant Blueprints by pickerin

This repository contains **Home Assistant Automation Blueprints** that I use in my own HAOS environment and am sharing with the community.

These blueprints focus on **real-world reliability**, optional helpers, and avoiding fragile “magic numbers” wherever possible.

Each blueprint consists of:
- a `.yaml` file containing the blueprint logic
- (optionally) a companion `.md` file with setup notes, rationale, and usage examples

If you want a smooth install and predictable behavior, **read the documentation** before importing.

---

## ⚙️ Feature Requests, Bugs, or Improvements?

Please use GitHub Issues — they help keep things organized and actionable:

👉 **[Open an issue](https://github.com/pickerin/HA_Blueprints/issues/new/choose)**

When reporting a bug, include:
- Home Assistant version
- Blueprint version
- Relevant entity IDs
- What you expected vs. what happened

---

## 🫴 Support & Discussion

Questions and discussion are welcome — especially if you’re adapting a blueprint to your own setup.

- Use the relevant **Home Assistant Community** thread (linked per blueprint when available)
- Or open a GitHub discussion / issue if it’s repo-specific

If something here helped you, a ⭐ on the repo is always appreciated.

---

## 🔃 Automation Blueprints

### 🌬️ Exhaust Fan – Auto On/Off (Humidity + Rate + Optional Controls)

This blueprint automatically controls an exhaust fan based on **humidity behavior**, not just static thresholds.

It prefers a **humidity rate (derivative) sensor** when available, and falls back to absolute humidity thresholds if not.

#### Features
- ✅ Auto-start fan when humidity is rising
- ✅ Auto-stop when humidity normalizes
- 💤 Optional *Sleep Hours* inhibit
- 🚿 Optional *Shower Mode* latch to avoid interfering with manual use
- ⏱ Optional max runtime timer failsafe
- 🔄 Works with or without derivative sensors

#### Requirements
- A humidity sensor for the room
- A switch entity controlling the exhaust fan

Optional helpers are fully supported but **not required**.

---

### 📥 Install Blueprint

Click the badge below to import directly into Home Assistant:

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](
https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/pickerin/HA_Blueprints/main/Automations/exhaust_fan_humidity_rise_auto_on_off.yaml
)

---

### 📄 Files

- **Blueprint YAML**  
  `blueprints/automation/pickerin/exhaust_fan_humidity_rise_auto_on_off.yaml`

- **Documentation (recommended)**  
  `blueprint_docs/exhaust_fan_humidity_rise_auto_on_off.md`  
  *(if present — contains setup guidance and design notes)*

---
