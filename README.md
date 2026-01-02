# Home Assistant Blueprints

This repository contains **production-grade Home Assistant Blueprints** built and used in a real, long-running Home Assistant installation.

These are not demo automations.

They exist to solve recurring automation problems **correctly**, **predictably**, and **defensively**, while remaining reusable across rooms, devices, and households.

If you’ve ever rewritten the same automation five times with slightly different entity IDs—this repo is for you.

---

## Design Philosophy

These blueprints follow a few strict principles:

### 1. Real-World First
Every blueprint here is extracted from an automation that:
- Ran for months (or years)
- Survived reboots, sensor flaps, and entity renames
- Handled edge cases like sleep hours, manual overrides, and device failures

No theoretical YAML. No blog-post toys.

---

### 2. Optional Means Optional
Most blueprints accept **optional helpers**:
- Input booleans (latches, inhibit modes)
- Timers (failsafes)
- Derivative sensors (rate-of-change)
- Schedule helpers

If you don’t provide them, the blueprint still works.

If you *do* provide them, behavior improves—never breaks.

---

### 3. Rate Beats Threshold
Absolute thresholds lie.

Wherever possible, these blueprints prefer **rate-of-change sensors** (via the `derivative` platform) over raw values:
- Humidity rise instead of humidity %
- Power change instead of absolute wattage
- State transitions instead of polling

Fallback thresholds exist—but only as a last resort.

---

### 4. Sleep, Quiet, and Human Context Matter
Automations should not:
- Wake people up
- Fight manual actions
- Run just because “a sensor changed”

Most blueprints support:
- Sleep-hours inhibit
- Latch booleans (to respect manual starts)
- Time-based or presence-based suppression

Automation should feel *intentional*, not robotic.

---

### 5. Modern Home Assistant YAML Only
This repository intentionally uses:
- **Plural containers**: `triggers`, `conditions`, `actions`
- **Singular discriminators** inside lists: `trigger`, `condition`, `action`
- `notify.send_message` (not legacy notify service calls)
- `choose:` instead of nested if-else templates

Blueprints here target **Home Assistant 2025.x+**.

---

## Blueprint Index

### 🚿 Exhaust Fan Automation

**Exhaust Fan – Auto On/Off (Humidity + Rate + Optional Helpers)**

Automatically controls bathroom / laundry exhaust fans using:
- Humidity rise *rate* (preferred)
- Fallback humidity thresholds
- Optional shower latch
- Optional sleep-hours inhibit
- Optional max runtime timer

**Why this exists:**  
Humidity alone is noisy. This blueprint reacts to *behavior*, not numbers.

**Supports:**
- Multiple rooms
- Manual fan usage
- Nighttime suppression
- Failsafe shutdowns

---

*(More blueprints will be added as patterns stabilize and repeat.)*

---

## Helper Patterns Used

Many blueprints rely on helpers you may not already use. These are intentional.

### Derivative Sensors (Strongly Recommended)
Used to detect **rate of change**, not absolute values.

Example:
```yaml
sensor:
  - platform: derivative
    source: sensor.master_bathroom_humidity
    name: master_bathroom_humidity_rate
    unit_time: min