# 🏠 SUPERNOVA SMARTHOME — ESP8266 Wi-Fi Smart Switch Firmware

<p align="center">
  <img src="https://img.shields.io/badge/Platform-ESP8266-blue?style=for-the-badge&logo=espressif" alt="ESP8266">
  <img src="https://img.shields.io/badge/Board-NodeMCU-orange?style=for-the-badge" alt="NodeMCU">
  <img src="https://img.shields.io/badge/Flash-4MB-green?style=for-the-badge" alt="4MB Flash">
  <img src="https://img.shields.io/badge/Source-Closed-red?style=for-the-badge" alt="Closed Source">
</p>

<p align="center">
  <b>A complete, self-hosted Wi-Fi Smart Home switch controller for ESP8266 NodeMCU</b><br>
  Web Dashboard • Captive Portal • Amazon Alexa • Telegram Bot • OTA Updates • Timers & Scheduling
</p>

---

## 📖 Overview

**SUPERNOVA SMARTHOME** is a feature-rich, standalone home automation firmware built for the **ESP8266 NodeMCU (4MB Flash)**. It turns a single low-cost ESP8266 board into an 8-channel Wi-Fi relay/switch controller with a built-in web dashboard, captive portal setup, Amazon Alexa voice control, Telegram bot integration, and OTA (Over-The-Air) firmware updates — no cloud subscription, no third-party server required.

> 🔒 **Closed-source firmware.** This repository distributes a **precompiled, ready-to-flash binary (`.bin`)** only. The source code is not published. All configuration (Wi-Fi, switch names, Telegram, Alexa, timers, etc.) is done through the device's built-in Web Dashboard — no code editing required.

---

## ✨ Features

- 🔌 **8-Channel Relay/Switch Control** — individually named, individually configurable
- 🌐 **Built-in Web Dashboard** — control switches from any browser on your network (login-protected)
- 📡 **Captive Portal Wi-Fi Setup** — configure SSID/password without hardcoding credentials
- 🗣️ **Amazon Alexa Integration** (via fauxmoESP) — voice control ("Alexa, turn on Switch 1")
- 🤖 **Telegram Bot Control** — turn switches on/off and get instant notifications from anywhere in the world
- ⏰ **Timers & Scheduling** — duration-based timers and date/time-based (epoch) scheduled ON/OFF, with power-cut memory
- 🔁 **Per-Switch Power-On Behavior** — choose Last State / Always ON / Always OFF after a power cut
- 🎚️ **Physical Switch Support** — manual switch mode or touch switch mode via I2C (PCF8574) or direct GPIO
- 🔄 **OTA (Over-The-Air) Updates** — update firmware without a USB cable
- 🌍 **Static IP / DHCP Support** — flexible network configuration
- 🕒 **NTP Time Sync** — with configurable timezone offset
- 🔒 **Login-Protected Web UI** with session tokens
- 🔧 **Hardware Factory Reset Button** support
- 💾 **EEPROM-based Persistent Storage** — all settings survive power loss
- 📶 **Wi-Fi Status LED Indicator** (connected / searching / reset modes)

---

## 🧰 Hardware Requirements

| Component | Requirement |
|---|---|
| Microcontroller | ESP8266 NodeMCU (ESP-12E/ESP-12F) |
| **Flash Memory** | **4MB** (required — see note below) |
| Relay Module | Up to 8-channel relay module |
| Optional | PCF8574 I/O Expander (for I2C switch mode) |

> ⚠️ **Important:** This firmware is compiled specifically for **ESP8266 NodeMCU boards with 4MB flash memory**. Flashing this `.bin` file onto a module with smaller flash (e.g. 1MB) will fail to boot or brick the flash layout.

---

## ⚙️ Flashing the Precompiled `.bin` File

Flash the provided `.bin` directly using [esptool](https://github.com/espressif/esptool), the Arduino IDE's built-in "Upload using programmer" option, or the NodeMCU PyFlasher / ESP8266 Flash Download Tool.

### Board / Flash Settings (for reference)

| Setting | Value |
|---|---|
| Board | Generic ESP8266 Module / NodeMCU 1.0 (ESP-12E Module) |
| Flash Size | **4MB (FS:1MB, OTA:~1019KB)** or 4MB (FS:2MB, OTA:1019KB) |
| CPU Frequency | 160 MHz |
| Upload Speed | 115200 |
| Flash Address | `0x00000000` |

### Using esptool (command line)

```bash
esptool.py --port COM_PORT --baud 115200 write_flash 0x00000000 SUPERNOVA_SMARTHOME.bin
```

Replace `COM_PORT` with your board's serial port (e.g. `COM5` on Windows, `/dev/ttyUSB0` on Linux).

---

## ⚠️ Known Limitation: Telegram Bot vs Web Dashboard/Captive Portal (RAM Usage)

The ESP8266 has only **~80 KB of total RAM**. Running the **Telegram Bot** (which uses an HTTPS/SSL connection) **at the same time** as the **Web Dashboard / Captive Portal** can cause those services to slow down or become temporarily unresponsive, since SSL connections are memory-intensive on this chip.

**Recommended usage:**
1. Keep the Telegram Bot **OFF** by default.
2. Turn it **ON** via a Telegram message only when you need remote control/notifications.
3. Before using the Web Dashboard or Captive Portal for configuration, turn the Telegram Bot back **OFF** using a Telegram command.
4. Turn it back **ON** again whenever remote access is needed.

This is a hardware RAM limitation of the ESP8266 chip itself, not a bug — following this usage pattern gives the most stable experience.

---

## 🚀 Getting Started

1. Flash the firmware (see [Flashing](#️-flashing-the-precompiled-bin-file) above).
2. Power on the ESP8266 — it will start in **Access Point (AP) mode** with a captive portal.
3. Connect to the Wi-Fi network `SUPERNOVA SMARTHOME` (default password: `12345678`).
4. A captive portal / login page will open automatically (or visit `192.168.4.1` in your browser).
5. Log in (default: `admin` / `admin` — **change this immediately**).
6. Configure your home Wi-Fi (STA mode), switch names, Alexa, Telegram bot token, and timers from the dashboard.

> 🔐 **Security tip:** Always change the default AP password and web login credentials after first setup.

---

## 🤖 Telegram Bot Setup

1. Create a bot via [@BotFather](https://t.me/BotFather) on Telegram and get your **Bot Token**.
2. Get your **Chat ID** (via [@userinfobot](https://t.me/userinfobot) or similar).
3. Enter the Token and Chat ID(s) in the device's Web Dashboard settings.
4. Enable the bot, and control your switches or receive notifications directly from Telegram.

---

## 🗣️ Alexa Voice Control

Powered by [fauxmoESP](https://github.com/vintlabs/fauxmoESP), each switch is exposed as a separate Alexa-compatible device. Simply say:
> "Alexa, discover devices"

Then control each switch by name (e.g., "Alexa, turn on Switch 1").

---

## 📌 Version

**Current release:** `v1.0.0` — 4MB Flash build

---

## 🤝 Feedback & Support

Found a bug, or have a feature request? Please open an [Issue](../../issues) — feedback helps improve future releases.

---

## 📄 License

This is **closed-source freeware**. The compiled binary (`.bin`) is free to download and use for personal/home projects. The source code is not published, and reverse engineering or redistribution of modified binaries is not permitted without permission.

---

## ⭐ Support the Project

If this firmware helped you build your own smart home, consider giving the repo a ⭐ — it helps others discover the project too!

---

### 🏷️ Keywords
`ESP8266` `NodeMCU` `smart home` `home automation` `IoT` `relay module` `wifi switch` `Alexa` `fauxmoESP` `Telegram bot` `captive portal` `OTA firmware update` `Arduino` `smart switch DIY`
