---
layout: default
title: StompLink
---

# StompLink

A wireless BLE MIDI footswitch controller for the Mooer Prime M2, with wired fallback. Built with ESP32.

![StompLink](../images/stomplink.png)

## The Problem

The Mooer Prime M2 is a fantastic multi-effects unit, but it only has 2 footswitches. That's enough for switching presets, but not for toggling individual effects during a performance. The official Mooer F4 wireless controller exists, but where's the fun in buying something you can build?

## The Solution

**StompLink** is a DIY 4-button wireless footswitch that connects to the M2 via Bluetooth Low Energy (BLE) MIDI. Each button sends Control Change (CC) messages to toggle effects on/off within a preset — no more bending down mid-song to disable the delay.

For venues where Bluetooth is unreliable, StompLink also includes a wired TRS MIDI output (3.5mm Type A).

## Features

- 🎸 **4 Momentary Footswitches** for effect control
- 📶 **BLE MIDI** wireless connection to Mooer Prime M2
- 🔌 **Wired TRS MIDI** fallback (3.5mm Type A)
- 🔋 **18650 Battery** with USB-C charging
- 💡 **LED Indicators** for active effects
- 🏠 **Aluminum Enclosure** (1590BB) for stage durability

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     StompLink                           │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐                   │
│  │ SW 1 │ │ SW 2 │ │ SW 3 │ │ SW 4 │  ← Footswitches   │
│  └──┬───┘ └──┬───┘ └──┬───┘ └──┬───┘                   │
│     │        │        │        │                        │
│     └────────┴────────┴────────┘                        │
│                    │                                    │
│              ┌─────▼─────┐                              │
│              │   ESP32   │                              │
│              └─────┬─────┘                              │
│         ┌──────────┼──────────┐                         │
│         ▼          ▼          ▼                         │
│    ┌────────┐ ┌────────┐ ┌────────┐                    │
│    │BLE MIDI│ │TRS MIDI│ │  LEDs  │                    │
│    └────────┘ └────────┘ └────────┘                    │
└─────────────────────────────────────────────────────────┘
         │              │
         ▼              ▼
    ┌─────────────────────────┐
    │    Mooer Prime M2       │
    └─────────────────────────┘
```

## MIDI Implementation

### Control Change (CC) Messages

Each footswitch sends a CC message to toggle an effect block:

| Switch | Function | MIDI Message |
|--------|----------|--------------|
| SW 1 | Toggle Effect 1 | CC #[TBD] |
| SW 2 | Toggle Effect 2 | CC #[TBD] |
| SW 3 | Toggle Effect 3 | CC #[TBD] |
| SW 4 | Toggle Effect 4 | CC #[TBD] |

*Note: CC numbers will be determined based on Mooer Prime M2 MIDI implementation.*

### Connection Modes

| Mode | Protocol | Connector |
|------|----------|-----------|
| Wireless | BLE MIDI | Bluetooth 4.0+ |
| Wired | MIDI over TRS | 3.5mm Type A |

## Hardware

### Bill of Materials

| Component | Model/Spec | Purpose |
|-----------|------------|---------|
| Microcontroller | ESP32 | BLE MIDI + Logic |
| Battery | 18650 Li-Ion | Power |
| Charger | TP4056 USB-C | Battery charging with protection |
| Boost Converter | MT3608 | 3.7V → 5V regulation |
| Footswitches | Momentary Soft Touch × 4 | Effect toggles |
| MIDI Jack | 3.5mm TRS Female | Wired MIDI output |
| LEDs | 5mm with bezels × 4 | Status indicators |
| Enclosure | 1590BB Aluminum | Rugged stage housing |
| Power Switch | Mini Rocker | Battery cutoff |

### Wiring Diagram

*Coming soon...*

## Software Design

### Core Functionality

```cpp
// Pseudocode
void onFootswitchPress(int switch_id) {
    bool effect_state = toggleEffectState(switch_id);

    // Send via active connection
    if (ble_connected) {
        sendBLE_CC(switch_id, effect_state ? 127 : 0);
    } else {
        sendTRS_CC(switch_id, effect_state ? 127 : 0);
    }

    updateLED(switch_id, effect_state);
}
```

### Power Management

- **Deep Sleep**: ESP32 enters low-power mode after inactivity
- **Wake on Press**: Any footswitch press wakes the device
- **Battery Monitor**: Low battery LED warning

## Target Setup

This controller is designed for the following rig:

- **Guitar**: PRS SE 24-08 (with coil-split)
- **Multi-FX**: Mooer Prime M2
- **Monitoring**: JBL Live 500 headphones

### Example Use Case

Playing "Comfortably Numb" (Pink Floyd):
- **Verse**: SW1 disables Muff, clean tone
- **Solo**: SW1 enables Muff + SW2 enables Delay
- **Outro**: SW3 enables Flanger for modulation

## Project Status

🚧 **Work in Progress**

- [x] Concept and design
- [x] Bill of materials
- [ ] Hardware assembly
- [ ] ESP32 firmware
- [ ] BLE MIDI implementation
- [ ] Wired MIDI implementation
- [ ] 3D printed button caps
- [ ] Final enclosure drilling

## Tech Stack

- **Platform**: ESP32 (Arduino framework)
- **Wireless**: BLE MIDI (Bluetooth Low Energy)
- **Wired**: MIDI over TRS Type A
- **Power**: 18650 + TP4056 + MT3608
- **Build System**: PlatformIO

## Links

- GitHub Repository: *Coming soon*

## References

- [Mooer Prime M2 MIDI Implementation](https://www.mooeraudio.com/)
- [BLE MIDI Specification](https://www.midi.org/specifications/midi-transports-specifications/midi-over-bluetooth-low-energy-ble-midi)
- [MIDI TRS Type A Wiring](https://minimidi.world/)

---

[← Back to Projects](/)
