---
layout: default
title: SolarPulse
---

# SolarPulse

A compact, WiFi-enabled off-grid monitoring display built with ESP32. Designed for real-time visibility into solar power systems without needing to check your phone or computer.

![SolarPulse Device](../images/solarpulse.png)

## The Problem

Living off-grid means constantly managing power. Traditional monitoring requires opening apps or dashboards — not practical when you just want a quick glance at your battery status while cooking dinner.

## The Solution

SolarPulse is a tiny, always-on display that sits in your kitchen (or anywhere) showing real-time power data:

- **Battery percentage** and charge/discharge state
- **Instant power consumption**
- **Available solar power**
- **LED alerts** for critical events (e.g., generator running)

The display cycles through different states based on system conditions, giving you contextual information when you need it.

## Architecture

```
┌─────────────────┐     MQTT      ┌─────────────────┐     HTTP      ┌─────────────────┐
│  Victron Energy │ ───────────▶  │  Raspberry Pi   │ ───────────▶  │    SolarPulse   │
│  Solar System   │               │  + Node-RED     │               │  ESP32 Display  │
└─────────────────┘               └─────────────────┘               └─────────────────┘
```

1. **Victron Energy** hardware publishes real-time data via MQTT
2. **Raspberry Pi** running Node-RED subscribes to MQTT, processes the data, and formats messages
3. **SolarPulse** receives HTTP requests and updates the display + LEDs

## Hardware

| Component | Model |
|-----------|-------|
| Microcontroller | ESP32-C3 Mini |
| Display | SSD1306 OLED 128x32 (I²C) |
| LEDs | 4x indicator LEDs (active low) |

The ESP32-C3 was chosen for its compact size, low power consumption, and built-in WiFi — perfect for a device that needs to run 24/7.

## Protocol Design

SolarPulse uses a simple, human-readable protocol that makes debugging easy:

```
<LED1><LED2><LED3><LED4>;<Line1>;<Line2>
```

**Example:**
```
1000;Bat 87%;+450W
```

- `1000` → First LED on (generator running), others off
- `Bat 87%` → First display line
- `+450W` → Second display line (charging at 450W)

This protocol is intentionally generic — SolarPulse can display **any** data you send it, making it reusable for other monitoring use cases.

## Software Design

### State Machine Architecture

The firmware uses a clean state machine pattern for managing device states:

```
BOOTING → WIFI_SETUP → MAIN
              ↑          │
              └──────────┘
            (on disconnect)
```

- **BOOTING**: Initialize hardware, configure WiFi manager
- **WIFI_SETUP**: Connect to known network or create AP for configuration
- **MAIN**: Listen for HTTP requests and update display

### Key Features

- **Captive Portal**: On first boot, creates a WiFi hotspot for easy network configuration
- **Auto-reconnect**: Automatically handles WiFi disconnections
- **Async Web Server**: Non-blocking HTTP handling for responsive updates
- **Modular Display Class**: Abstracts OLED and LED control into a reusable component

## API

Update the display with a simple HTTP GET request:

```bash
curl "http://<device-ip>/api?data=0000;Hello;World"
```

| Parameter | Description |
|-----------|-------------|
| `data` | Message in protocol format: `<LEDs>;<Line1>;<Line2>` |

## Getting Started

### Prerequisites

- PlatformIO
- ESP32-C3 development board
- SSD1306 OLED display

### Installation

```bash
git clone https://github.com/imarcelolz/solar-pulse.git
cd solar-pulse
pio run --target upload
```

### Configuration

1. Power on the device
2. Connect to the `SolarPulse` WiFi network
3. Open `http://192.168.4.1` and enter your WiFi credentials
4. The device will display its IP address once connected

## Future Improvements

- [ ] Add Node-RED flow examples to repository
- [ ] Support for multiple display pages
- [ ] Battery-powered version with deep sleep
- [ ] Web dashboard for configuration

## Tech Stack

- **Platform**: ESP32-C3 (Arduino framework)
- **Display**: Adafruit SSD1306 library
- **Networking**: WiFiManager, ESPAsyncWebServer
- **Build System**: PlatformIO

## Links

- [GitHub Repository](https://github.com/imarcelolz/solar-pulse)

---

[← Back to Projects](/)
