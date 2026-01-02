# 🌀 Exhaust Fan – Auto On/Off  
## Humidity + Rate + Optional Timer / Latch / Sleep-Hours  
### Helper & Tuning Guide

This document explains **how to tune and customize** the  
**Exhaust Fan – Auto On/Off (Humidity + Optional Rate/Timer/Latch)** blueprint.

This blueprint is designed to:

- Turn the fan **ON quickly** when humidity rises (shower, laundry, steam)
- Keep the fan **ON long enough** to actually clear the room
- Turn the fan **OFF safely** when humidity normalizes
- Avoid false triggers from weather, HVAC, or sensor noise
- Optionally **avoid running during sleep hours**
- Optionally **avoid turning off manually-started fans**
- Optionally enforce a **maximum runtime failsafe**

If your fan feels “too jumpy” or “too lazy,” this guide explains exactly which
knobs to turn.

---

## 🧠 How the Blueprint Thinks

The blueprint operates in **two modes**:

### ⭐ 1. Rate Mode (Preferred)
If a **humidity rate (derivative) sensor** is provided:

- Fan turns **ON** when humidity is rising fast enough
- Fan turns **OFF** when humidity rise slows and overall humidity is safe
- Much more reliable than fixed thresholds

### 🧱 2. Fallback Humidity Mode
If no rate sensor is provided:

- Fan turns **ON** when humidity exceeds a threshold
- Fan turns **OFF** when humidity drops below a threshold
- Simpler, but more prone to false triggers

---

## 🔌 Required Inputs

### fan_switch
**Type:** switch  
**Purpose:** The exhaust fan being controlled

No tuning required — just ensure it is the correct switch.

---

### humidity_sensor
**Type:** sensor (percentage)  
**Purpose:** Humidity level for the room

If your sensor updates slowly, consider longer hold times (`*_for` values).

---

## ⭐ Strongly Recommended (Rate Mode)

### humidity_rate_sensor
**Type:** sensor (percent per minute)  
**Purpose:** Measures how fast humidity is rising or falling

This sensor is what makes the automation *feel smart*.

Example derivative sensor configuration:

```yaml
sensor:
  - platform: derivative
    source: sensor.master_bathroom_humidity
    name: master_bathroom_humidity_rate
    unit_time: min

```
Alternatively, you can just create it with the Helper UI and these 
parameters:
- Name:  Whatever you want to call it
- Input sensor: Your humidity sensor
- Precision: 2
- Time window: LEAVE ALONE
- Time unit: Minutes

**Tuning advice:**
- Fan starts too late → lower `rate_on_threshold`
- Fan starts randomly → increase `rate_on_for`

---

## 💤 Optional Sleep-Hours Inhibit

### sleep_hours_boolean
**Type:** input_boolean  
**Purpose:** Prevents auto-start during sleep hours

- If ON → fan will **not auto-start**
- Fan is still allowed to auto-stop later

Recommended to avoid late-night surprises.

---

## 🪝 Optional Latch (Shower Mode)

### shower_mode_boolean
**Type:** input_boolean  
**Purpose:** Prevents automation from turning off a fan you turned on manually

How it works:
- Automation turns fan ON → latch turns ON
- Automation turns fan OFF → latch clears
- If latch is OFF, automation will not stop the fan

Use this if you often run the fan manually.

---

## ⏲️ Optional Max Runtime Timer

### max_run_timer
**Type:** timer  
**Purpose:** Hard safety cutoff for fan runtime

- Timer starts when fan turns ON
- When timer finishes → fan turns OFF

Useful if sensors get stuck or humidity never quite drops.

---

## 🎛️ Rate Mode Thresholds (Preferred Mode)

### rate_on_threshold
**Default:** 0.35  
Fan turns ON when humidity rate exceeds this value.

- Too sensitive → increase
- Too slow → decrease

---

### rate_on_for
**Default:** 00:02:00  
Humidity rate must stay above threshold this long.

- Noise triggers → increase
- Late activation → decrease

---

### humidity_min_to_start
**Default:** 45  
Minimum humidity required before rate-based start is allowed.

Prevents low-humidity false triggers.

---

### rate_off_threshold
**Default:** 0.05  
Fan turns OFF when humidity rate drops below this value.

- Fan runs too long → increase
- Fan stops too early → decrease

---

### rate_off_for
**Default:** 00:05:00  
Rate must stay low this long before stopping.

---

### humidity_max_to_stop
**Default:** 60  
Fan will not stop unless humidity is below this value.

Raise this in naturally humid climates.

---

## 🧱 Fallback Humidity Mode (Used Only Without Rate Sensor)

### humidity_on_threshold
**Default:** 55  
Turn fan ON above this value.

---

### humidity_on_for
**Default:** 00:02:00  
Humidity must stay high this long before starting.

---

### humidity_off_threshold
**Default:** 50  
Turn fan OFF below this value.

---

### humidity_off_for
**Default:** 00:05:00  
Humidity must stay low this long before stopping.

