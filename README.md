# RC-IOT
# AI Surveillance RC  – Technical README

## Overview

This project converts a **basic toy RC car** into an **AI-enabled indoor surveillance rover** by using a **smartphone as the brain** (camera, AI, voice, networking) and minimally modifying the existing RC system.

The core principle is **non-invasive control**: we do not change the car’s motors or RF system. Instead, we automate the **existing remote control**.

---

## System Goals

* Autonomous indoor patrolling
* Human/motion detection using phone camera
* Voice alerts and recording
* Low cost (₹5,000 budget)
* Minimal electronics modification
* Safe and reversible build

---

## Hardware Inventory (Current)

### RC Car

* Type: Toy RC car (Wembley/Popsugar class)
* Control: Handheld remote
* Radio Frequency: 2.4 GHz
* Protocol: Proprietary RC protocol (not Wi-Fi / Bluetooth)
* Onboard compute: None
* Sensors: None
* Charging: USB

**Key constraint:** Car only responds to its paired remote.

---

### RC Remote

* Buttons: Forward / Backward / Left / Right
* Electronics:

  * Button matrix (copper pads)
  * Encoder MCU (COB / epoxy blob)
  * 2.4 GHz RF transmitter
* Protocol: Proprietary, undocumented
* Programmable: No
* Hackable at button level: Yes

**Key insight:** Button presses are purely electrical and can be simulated.

---

### Smartphone (Poco X2)

* Camera: Yes
* Microphone & Speaker: Yes
* AI capability: Yes (on-device ML / apps)
* Connectivity:

  * Wi-Fi
  * Bluetooth
  * IR blaster (TV/AC only)
* Raw RF transmission: No

**Key constraint:** Phone cannot transmit proprietary RC RF signals.

---

## How the RC System Works (Baseline)

### Manual Control Flow

```
Human finger
   ↓
Remote button
   ↓
2.4 GHz proprietary RF
   ↓
Car receiver
   ↓
Motor driver
   ↓
Car moves
```

* No autonomy
* No feedback
* No sensing

---

## Why the Phone Cannot Control the Car Directly

Although both Wi-Fi/Bluetooth and RC remotes use the **2.4 GHz band**, they use **different protocols**.

| Technology | Frequency | Protocol     |
| ---------- | --------- | ------------ |
| Wi-Fi      | 2.4 GHz   | IEEE 802.11  |
| Bluetooth  | 2.4 GHz   | BLE / BR-EDR |
| RC Remote  | 2.4 GHz   | Proprietary  |

Smartphones cannot:

* Generate raw RF waveforms
* Access low-level radio modulation
* Transmit proprietary RC packets

Therefore, **software-only control is impossible**.

---

## Identified Integration Point

### Remote Button Matrix (Critical)

Each button on the remote consists of **two copper pads**.

* Pads shorted → button pressed
* Pads open → button released

This is the **only viable injection point** for automation.

---

## Proposed Architecture (Locked)

```
Phone (AI + Camera + Voice)
        ↓  Wi-Fi
      ESP32
        ↓  GPIO
   Relay / Transistor
        ↓
Remote button pads
        ↓
Remote RF transmitter
        ↓
RC Car
```

---

## Role of Each Component

### Smartphone

* AI inference (person/motion detection)
* Patrol logic
* Voice alerts
* Video recording
* Sends high-level commands

### ESP32 (To Be Added)

* Acts as electronic button presser
* Receives Wi-Fi commands from phone
* Controls relays / GPIO
* No AI, no RF

### Remote

* Continues to handle RF transmission
* Encodes valid control packets

### RC Car

* Executes movement only

---

## What Is Explicitly Out of Scope

* RF protocol emulation
* Direct motor rewiring (for v1)
* ROS / SLAM
* Bluetooth-based control
* Phone-powered motor driving

---

## Known Unknowns (Next Inspection Required)

* Remote PCB pad layout
* Shared ground confirmation
* Steering mechanism type
* Physical mounting space

These will be resolved after opening the remote.

---

## Current Status

**State:** Documentation complete, no automation hardware added yet.

**Next Milestone:** Remote automation layer (ESP32 + relays).

---

## Versioning

* v0.1 – System understanding & constraints (this document)
* v0.2 – Remote automation wiring & firmware (next)

---

## Summary (One Line)

> This project transforms a radio-controlled toy into an AI surveillance rover by letting a smartphone think and an ESP32 act.
