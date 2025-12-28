---
layout: default
title: Standup Desk Controller
---

# Standup Desk Controller

A DIY motor controller for standing desks, built with Arduino Nano and a BTS7960 motor driver.

![Standup Desk Controller](../images/standup-desk-controller.png)

## The Story

I bought a standing desk frame without a motor — just a manual crank. After a few weeks of manual adjustments, I decided to motorize it. While a simple PWM controller would have done the job, I wanted an excuse to write some C++ and have fun with Arduino.

## How It Works

```
┌──────────────┐      ┌──────────────┐      ┌──────────────┐
│   Toggle     │ ───▶ │   Arduino    │ ───▶ │   BTS7960    │ ───▶ DC Motor
│   Switch     │      │    Nano      │      │   Driver     │
└──────────────┘      └──────────────┘      └──────────────┘
                             │
                             ▼
                      ┌──────────────┐
                      │  Status LED  │
                      └──────────────┘
```

- **Toggle UP**: Desk raises while button is held
- **Toggle DOWN**: Desk lowers while button is held
- **Release**: Motor stops immediately
- **Error detection**: Monitors motor driver fault pins

## Hardware

| Component | Purpose |
|-----------|---------|
| Arduino Nano | Main controller |
| BTS7960 43A | High-current motor driver |
| DC Motor | Linear actuator for desk |
| Toggle Switch | Up/down control |
| LED | Status indicator |

### Why BTS7960?

The BTS7960 is a robust H-bridge driver rated for 43A, perfect for driving DC motors in both directions. It includes built-in error detection pins that signal overcurrent or thermal faults — useful for protecting the motor and desk.

## Software Design

### State Machine

```
    ┌─────────────────────────────────┐
    │                                 │
    ▼                                 │
  MAIN ──────▶ SLEEP ──────▶ (wake) ──┘
    │
    ▼
  ERROR ──────▶ SLEEP
```

- **MAIN**: Monitors buttons, controls motor
- **SLEEP**: Low-power mode after inactivity (WIP)
- **ERROR**: Triggered by motor driver faults

### Motor Control

The `Motor` class abstracts all motor operations:

```cpp
motor.forward();   // Raise desk
motor.backward();  // Lower desk
motor.stop();      // Stop immediately
motor.hasError();  // Check for faults
```

PWM speed control allows smooth starts and stops, reducing mechanical stress on the desk frame.

## Pin Configuration

| Pin | Function |
|-----|----------|
| 20 | Button UP |
| 21 | Button DOWN |
| 2 | Motor DOWN PWM |
| 3 | Motor DOWN Enable |
| 4 | Motor DOWN Error |
| 10 | Motor UP PWM |
| 9 | Motor UP Enable |
| 8 | Motor UP Error |

## Future Improvements

- [ ] Implement low-power sleep mode
- [ ] Add height presets with limit switches
- [ ] OLED display showing current height
- [ ] Memory for favorite positions

## Tech Stack

- **Platform**: Arduino Nano
- **Motor Driver**: BTS7960 43A H-Bridge
- **Build System**: PlatformIO
- **Language**: C++

## Links

- [GitHub Repository](https://github.com/imarcelolz/standup-desk-controller)

---

[← Back to Projects](/)
