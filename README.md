# buderus_ecomatic4000
 
> **ESPHome firmware for integrating the Buderus Ecomatic 4000 (HS4201) heating controller into Home Assistant via the KM2.0 Serial Module M404 and an RS232 interface.**

[![ESPHome](https://img.shields.io/badge/ESPHome-compatible-blue?logo=esphome)](https://esphome.io/)
[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-integration-41BDF5?logo=home-assistant)](https://www.home-assistant.io/)
[![ESPHome](https://img.shields.io/badge/ESPHome-2024.x%2B-blue)](https://esphome.io)
[![Platform](https://img.shields.io/badge/Platform-ESP32-red)](https://www.espressif.com/)
[![Framework](https://img.shields.io/badge/Framework-ESP--IDF-green)](https://docs.espressif.com/projects/esp-idf/)

---

![Home Assistant Buderus Panel](https://github.com/GernotAlthammer/buderus_ecomatic4000/raw/main/Pictures/HA-Buderus-Panel.png)
 
---
 
## Table of Contents
 
- [Overview](#overview)
- [Background & Credits](#background--credits)
- [Hardware Requirements](#hardware-requirements)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
- [Supported Sensor Values](#supported-sensor-values)
  - [Temperature Sensors](#temperature-sensors)
  - [Binary States – Control Bits (0x0803)](#binary-states--control-bits-0x0803)
  - [Binary States – Operating Bits (0x3108)](#binary-states--operating-bits-0x3108)
- [Home Assistant Integration](#home-assistant-integration)
- [Known Limitations](#known-limitations)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Disclaimer & License](#disclaimer--license)
 
---
 
## Overview
 
This project provides an **ESPHome-based firmware** that allows you to monitor your **Buderus Ecomatic 4000 (HS4201)** heating unit from within **Home Assistant**. Communication with the boiler controller is established via the **KM2.0 Serial Module M404**, which exposes a serial RS232 interface.
 
The firmware runs on an **ESP32 D1 Mini** microcontroller paired with an RS232-to-TTL converter module. Once flashed and configured, the ESP32 connects to your Wi-Fi network and exposes all available sensor readings and binary states directly to Home Assistant via the ESPHome native API.
 
---
 
## Background & Credits
 
This project is an **adaptation** of the excellent [KM271-WiFi-Module](https://github.com/the78mole/esphome_components/tree/main) project by **the78mole**, which was originally developed as a Wi-Fi replacement for the **KM271 serial module** used with the **Buderus Logamatic 2017** control unit.
 
The Buderus Ecomatic 4000 uses a similar but distinct serial protocol. This repository contains the necessary parameter mappings and configuration adjustments to make the firmware compatible with the **HS4201 / KM2.0 M404** hardware combination.
 
**Key difference from the original project:**  
You no longer need to manually copy the `km271_wifi` component directory into your Home Assistant ESPHome folder. The component is referenced directly and compiled as part of the ESPHome build process.
 
---
 
## Hardware Requirements
 
| Component | Description |
|---|---|
| **Buderus Ecomatic 4000** | Heating controller model HS4201 |
| **KM2.0 Serial Module M404** | Buderus serial communication module (RS232) |
| **ESP32 D1 Mini** | Microcontroller running the ESPHome firmware |
| **RS232-to-TTL Module** | Converts the RS232 signal levels to 3.3 V / 5 V TTL for the ESP32 |
| **RS232 Cable** | Connects the KM2.0 M404 to the RS232 module |
 
A reference photo of the complete hardware assembly (ESP32 D1 Mini + RS232 module) is available in the [`Example`](https://github.com/GernotAlthammer/buderus_ecomatic4000/tree/main/Example) folder of this repository.
 
![Hardware Example](https://github.com/GernotAlthammer/buderus_ecomatic4000/raw/main/Example/IMG_7350.jpg)
 
> ⚠️ **Safety note:** Working with heating systems involves mains voltages and can cause equipment damage or personal injury if done incorrectly. Only connect the serial interface as described. Do **not** modify the boiler's internal wiring. The RS232 serial connection is a low-voltage data-only interface.
 
---
 
## Repository Structure
 
```
buderus_ecomatic4000/
├── Documents/          # Background documentation (protocol specs, pin-outs, etc.)
├── Example/            # Photos of the reference hardware build
├── HA-Panel/           # Home Assistant dashboard panel configuration (Lovelace YAML)
├── Logs/               # Serial communication logs used for protocol analysis
├── Pictures/           # Screenshots (e.g., HA dashboard preview)
├── esphome/
│   └── components/
│       └── km271_wifi/ # ESPHome custom component (C++ source)
│           └── km271_params.h   # Parameter address map for Ecomatic 4000
├── LICENSE
└── README.md
```
 
---
 
## Getting Started
 
### Prerequisites
 
- A running **Home Assistant** installation with the **ESPHome add-on** installed.
- Basic familiarity with ESPHome YAML configuration files.
- The hardware components listed in the [Hardware Requirements](#hardware-requirements) section, assembled and connected.
 
### Installation
 
1. **Clone or download** this repository.
 
2. **Copy the ESPHome configuration** from the `esphome/` folder into your Home Assistant ESPHome configuration directory (usually accessible via the ESPHome add-on's editor, or directly at `/config/esphome/`).
 
   > **Note:** It is no longer necessary to manually copy the `km271_wifi` component subfolder. ESPHome will pull the component automatically during compilation.
 
3. Open the **ESPHome Dashboard** in Home Assistant.
 
4. Create a new device configuration (or edit an existing one) and paste / import the provided YAML configuration.
 
5. **Flash** the firmware to your ESP32 D1 Mini — either via USB for the initial flash, or OTA for subsequent updates.
 
### Configuration
 
The main ESPHome YAML file references the `km271_wifi` custom component and configures the UART interface for RS232 communication. At a minimum, you will need to adjust:
 
- **`wifi`** — your Wi-Fi SSID and password (or use `secrets.yaml`).
- **`api` / `ota`** — your ESPHome API encryption key and OTA password.
- **UART pin assignments** — match the GPIO pins on your specific ESP32 D1 Mini wiring to the RS232 module (TX/RX).
 
Example snippet:
 
```yaml
esphome:
  name: buderus-ecomatic4000
 
esp32:
  board: wemos_d1_mini32
 
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
 
api:
  encryption:
    key: !secret api_encryption_key
 
ota:
  password: !secret ota_password
 
uart:
  id: km271_uart
  tx_pin: GPIO17
  rx_pin: GPIO16
  baud_rate: 2400
 
external_components:
  - source: github://the78mole/esphome_components
    components: [km271_wifi]
 
km271_wifi:
  uart_id: km271_uart
```
 
> Refer to the actual YAML files in the `esphome/` folder for the complete, tested configuration specific to the Ecomatic 4000.
 
---
 
## Supported Sensor Values
 
All currently supported parameters are defined in [`esphome/components/km271_wifi/km271_params.h`](https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/esphome/components/km271_wifi/km271_params.h).
 
### Temperature Sensors
 
| Parameter | German Name | Address |
|---|---|---|
| Outside Temperature | Außentemperatur | `0x2109` |
| Boiler Flow Actual Temp | Kesselvorlaufisttemperatur | `0x1105` |
| Hot Water Actual Temp | Warmwasseristtemperatur | `0x721A` |
| HC1 Room Actual Temp | HK1 Raumisttemperatur | `0xA11F` |
| HC1 Flow Actual Temp | HK1 Vorlaufisttemperatur | `0x420F` |
| HC1 Mixer Position | HK1 Mischerstellung | `0x6116` |
| Boiler Flow Setpoint Temp | Kesselvorlaufsolltemperatur | `0x1206` |
| Hot Water Setpoint Temp | Warmwassersolltemperatur | `0x320C` |
| HC1 Flow Setpoint Temp | HK1 Vorlaufsolltemperatur | `0x410E` |
| Hot Water Target Temp | Warmwassertemperatur Ziel | `0x7119` |
 
### Binary States – Control Bits (0x0803)
 
These bits reflect the current switching state of the boiler's actuators:
 
| Bit | Description |
|---|---|
| ST-Bit 0 | Burner ON (Brenner EIN) |
| ST-Bit 1 | DHW Charging Pump (Ladepumpe Warmwasser) |
| ST-Bit 2 | Radiator Circulation Pump (Zirkulationspumpe Heizkörper) |
| ST-Bit 3 | Mixer OPEN (Mischer AUF) |
| ST-Bit 4 | Mixer CLOSE (Mischer ZU) |
| ST-Bit 5 | Burner OFF (Brenner AUS) |
| ST-Bit 7 | Underfloor Heating Circulation Pump (Zirkulationspumpe Fußboden) |
 
### Binary States – Operating Bits (0x3108)
 
These bits reflect the current operating mode of the heating circuit:
 
| Bit | Description |
|---|---|
| BTR-Bit 0 | Day Mode active (Betrieb Tag) |
| BTR-Bit 1 | Automatic Mode active (Betrieb Automatik) |
| BTR-Bit 2 | Summer Mode active (Betrieb Sommer) |
 
---
 
## Home Assistant Integration
 
Once the ESP32 is running and connected to your network, Home Assistant will automatically discover it via the ESPHome native API (if auto-discovery is enabled). All mapped sensors and binary sensors will appear as entities in Home Assistant.
 
A ready-to-use **Lovelace dashboard panel** configuration is available in the [`HA-Panel/`](https://github.com/GernotAlthammer/buderus_ecomatic4000/tree/main/HA-Panel) folder. It provides a clean overview card showing all relevant boiler data at a glance, similar to the preview screenshot at the top of this README.
 
To use it:
1. Open **Home Assistant → Settings → Dashboards**.
2. Edit your desired dashboard and add a new card using the **Manual** card editor.
3. Paste the YAML content from the `HA-Panel/` folder.
4. Adjust entity IDs as needed to match your ESPHome device name.
 
---
 
## Known Limitations
 
- **Read-only:** Writing / changing parameter values (e.g., target temperatures, operating modes) is **not yet supported**. The command codes required to send parameters back to the Ecomatic 4000 controller have not yet been fully reverse-engineered.
- **Partial parameter coverage:** Only a subset of the full Ecomatic 4000 data set is currently decoded. Additional parameters require further serial log analysis.
- **Single heating circuit:** The current implementation focuses on heating circuit 1 (HK1). Multi-circuit support has not been implemented yet.
- **Protocol differences:** The Ecomatic 4000 uses a protocol related to, but distinct from, the Logamatic 2017 protocol. Some parameters from the upstream `km271_wifi` project may not be applicable.
 
---
 
## Roadmap
 
The following improvements are being investigated:
 
- [ ] Reverse-engineer write command codes to enable setting target temperatures and operating modes from Home Assistant
- [ ] Add support for additional sensor parameters (fault codes, runtime counters, etc.)
- [ ] Improve documentation of the serial protocol specifics for the KM2.0 M404
- [ ] Add support for a second heating circuit (HK2) if applicable
 
Contributions and protocol analysis logs are very welcome — see [Contributing](#contributing).
 
---
 
## Contributing
 
This is a **hobby project** and contributions are warmly welcomed. If you:
 
- Own a Buderus Ecomatic 4000 and have captured serial logs with additional parameters,
- Have reverse-engineered write commands for the KM2.0 M404,
- Want to improve the ESPHome component or documentation,
 
…please feel free to open a **pull request** or file an **issue** on GitHub. Serial communication logs for analysis can be added to the [`Logs/`](https://github.com/GernotAlthammer/buderus_ecomatic4000/tree/main/Logs) folder.
 
> This project has no affiliation with Buderus, Bosch Thermotechnik, or any related company.
 
---
 
## Disclaimer & License
 
This project is licensed under the **MIT License** — see the [LICENSE](https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/LICENSE) file for details.
 
**Use at your own risk.** This software is provided as a hobby project with no warranty of any kind. The author(s) accept no liability for damage to your heating system, property, or any other harm arising from the use of this software. Always ensure that any hardware modifications comply with local regulations and do not void your equipment warranty.
 
> *"I'm working on this project as a hobby. My work on this software is in no way associated with a company. If you like to use it, or improve on it, feel free."*
> — GernotAlthammer
 
---
 
*Last updated: 2025-01-18*
