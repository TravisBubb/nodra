# Nodra


![Status](https://img.shields.io/badge/status-in--progress-orange)
![License](https://img.shields.io/badge/license-MIT-blue)
![Last Commit](https://img.shields.io/github/last-commit/TravisBubb/nodra)
![Repo Size](https://img.shields.io/github/repo-size/TravisBubb/nodra)

**Nodra**: A secure, standalone peer-to-peer text communication device leveraging modular LoRa mesh networking.
> NOTE: This project is currently in active development (v0.1).

---

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Hardware](#hardware)
- [Software](#software)
- [Design Documentation](#design-documentation)
- [Contributing](#contributing)

---

## Project Overview
Nodra is designed for secure text communication in environments without reliable cellular or Wi-Fi coverage. It combines:
- **Standalone hardware device** with keypad and display  
- **LoRa mesh networking** for peer-to-peer and multi-hop communication  
- **Hardware-backed security** with TRNG and AES acceleration  
- **Rust-based CLI (`meshctl`)** for firmware management and device provisioning  

> NOTE: Initial prototype uses STM32WB55 development boards for rapid testing.

---

## Features
- Encrypted messaging via AES-GCM  
- Mesh networking with node discovery and store-and-forward routing  
- Bare-metal firmware (no RTOS/HAL) using CMSIS headers  
- Firmware upgrade and node management via `meshctl` CLI  

---

## Hardware
- **MCU**: STM32WB55RG  
- **LoRa Module**: Semtech SX1262  
- **Input**: 4×4 matrix keypad with MCP23017 I²C GPIO expander  
- **Display**: 1.8–2.4” SPI TFT/OLED (ST7789)  
- **Power**: LiPo battery (via dev board)  
- **Connectors**: USB for debugging, flashing  

> TODO: Update BOM for final v1 hardware.

---

## Software
- Bare-metal firmware controlling keypad, display, and LoRa stack  
- Mesh routing protocol implemented in Rust (via FFI for MCU integration)  
- CLI tool (`meshctl`) handles:  
  - Firmware upgrades  
  - Node registration and provisioning  
  - Secure wiping and allow-list management  

---

## Design Documentation
Full design documentation including architecture diagrams, pin mapping, and communication flow is available in the [docs directory](./docs/design.md).

> TODO: Add final diagrams in `docs/diagrams/` folder.

---

## Contributing
> NOTE: This is primarily a personal portfolio project, but PRs and suggestions are welcome.

- Fork the repo
- Create a feature branch
- Submit a pull request with a detailed description into the main branch
