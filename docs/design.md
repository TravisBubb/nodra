# Nodra v0.1: Official Design Document

**Project**: Nodra  
**Version**: 0.1 (Draft)  

**Purpose**: Secure, standalone peer-to-peer text communication device with a modular mesh networking protocol.  
**Audience**: Technical reviewers, potential employers, internal team.  

> NOTE: This document is a draft and subject to updates as hardware and firmware choices evolve.  

---

## Table of Contents
1. [Introduction](#1-introduction)  
   1.1 [Purpose](#11-purpose)  
   1.2 [Scope](#12-scope)  
   1.3 [Audience](#13-audience)  
2. [System Overview](#2-system-overview)  
   2.1 [System Goals](#21-system-goals)  
   2.2 [Use Cases](#22-use-cases)  
3. [Hardware Design](#3-hardware-design)  
   3.1 [Bill of Materials (BOM)](#31-bill-of-materials-bom)  
   3.2 [Microcontroller Selection](#32-microcontroller-selection)  
   3.3 [Peripheral Components](#33-peripheral-components)  
   3.4 [Power Management](#34-power-management)  
4. [Software Design](#4-software-design)  
   4.1 [Bare-Metal Approach](#41-bare-metal-approach)  
   4.2 [Firmware Architecture](#42-firmware-architecture)  
   4.3 [Communication Stack](#43-communication-stack)  
   4.4 [Security Features](#44-security-features)  
   4.5 [CLI Management Tool (“meshctl”)](#45-cli-management-tool-meshctl)  
5. [System Diagrams](#5-system-diagrams)  
6. [Implementation Plan](#6-implementation-plan)  
   6.1 [Prototyping Approach](#61-prototyping-approach)  
   6.2 [Development Milestones](#62-development-milestones)  
   6.3 [Testing Strategy](#63-testing-strategy)  
7. [Future Extensions](#7-future-extensions)  
   7.1 [Sensor Integration](#71-sensor-integration)  
   7.2 [Alternate Input Devices](#72-alternate-input-devices)  
   7.3 [Long-Term Vision](#73-long-term-vision)  

---

## 1. Introduction

### 1.1 Purpose
The purpose of this document is to define the design of Nodra, a secure, lightweight, and portable mesh communication device. Nodra enables users to exchange encrypted messages in environments without reliable cellular or Wi-Fi coverage, leveraging LoRa mesh networking and a simple user interface.

### 1.2 Scope
This document specifies the hardware and software design, including the microcontroller platform, communication stack, power system, and CLI management tool. The focus is on an initial prototype suitable for demonstration, testing, and eventual field use.

### 1.3 Audience
This document is intended for:
- Hardware and software developers  
- Security engineers  
- Potential defense/aerospace partners  
- Technical reviewers and evaluators  

> NOTE: Intended audience may require different levels of detail. This doc focuses on technical clarity for engineers and evaluators.  

---

## 2. System Overview

### 2.1 System Goals
- Enable secure peer-to-peer and mesh communication without infrastructure. 
- Provide hardware-backed encryption and randomness (TRNG + AES)
- Operate on low power for extended field use
- Support firmware upgrades and management via an external CLI tool

### 2.2 Use Cases
- Field operators exchanging encrypted messages
- Decentralized communications in denied or degraded environments
- Secure firmware updates via CLI

> TODO: Add real-world scenario diagrams for clarity.  

---

## 3. Hardware Design

### 3.1 Bill of Materials (BOM)
- **MCU**: STM32WB55RG Development Board  
- **LoRa Radio**: HopeRF RFM95 or Semtech SX1262 module  
- **Input**: 4×4 matrix keypad with I²C GPIO expander (MCP23017)  
- **Display**: 1.8”–2.4” SPI TFT or OLED display (e.g., ST7789)  
- **Power**: LiPo battery with onboard regulator (via dev board)  
- **Connectors**: USB (for debugging, power, CLI flashing)  
- **Misc**: Wires, breadboard for prototyping  

### 3.2 Microcontroller Selection
The **STM32WB55RG** was selected due to its:
- Hardware TRNG  
- AES-128/256 acceleration  
- Dual-core architecture for protocol separation  
- Integrated BLE (potential future use case for app integration)  

### 3.3 Peripheral Components
- 4×4 matrix keypad with MCP23017 I²C GPIO expander  
- TFT Display (ST7789) for UI menus and messaging  

### 3.4 Power Management
- Power management is handled via the dev board in initial prototype
- Future custom PCB may integrate voltage regulators and decoupling capacitors

---

## 4. Software Design

### 4.1 Bare-Metal Approach
- No HAL or RTOS frameworks used
- Only standard CMSIS headers for register-level control

### 4.2 Firmware Architecture
- **Core 1 (Cortex-M4)**: Application logic, mesh routing, encryption, UI  
- **Core 2 (Cortex-M0)**: Radio stack, low-level timing  

### 4.3 Communication Stack
- LoRa PHY (modulation, frequencies)  
- Custom mesh layer (node discovery, routing, store-and-forward)  
- Encryption (AES-GCM per-packet)  

### 4.4 Security Features
- Hardware TRNG for key generation  
- AES-128/256 accelerated encryption  
- Secure bootloader for firmware authenticity  

### 4.5 CLI Management Tool (“meshctl”)
A companion **Rust-based CLI** will:  
- Perform firmware upgrades via USB  
- Handle node registration into the mesh  
- Query device status and logs  
- Support key provisioning, clean wiping, and allow-listing other nodes  

> TODO: Add CLI flow diagram and example commands.  

---

## 5. System Diagrams
> TODO: Insert diagrams when ready, e.g.:  
- [Block Diagram](#)  
- [Pin Map](#)  
- [Communication Flow](#)

---

## 6. Implementation Plan

### 6.1 Prototyping Approach
- Use STM32WB55 dev board & breadboard wiring  
- Start with keypad & display integration  
- Add LoRa PHY communication  
- Integrate mesh routing layer  

### 6.2 Development Milestones
1. Hardware bring-up (keypad, display, LoRa)  
2. Bare-metal drivers for all peripherals  
3. Mesh protocol implementation  
4. CLI prototype for firmware flashing  
5. Secure boot and firmware validation  
6. End-to-end encrypted message exchange demo  

### 6.3 Testing Strategy
- Unit testing of drivers (on-device validation)  
- Loopback tests for radio stack  
- Integration tests with multiple devices  
- CLI → Device firmware upgrade test  

> NOTE: Consider automated test scripts for repeated validation.  

---

## 7. Future Extensions

### 7.1 Sensor Integration
- Environmental sensors (GPS, accelerometer, etc.) for context data.  

### 7.2 Alternate Input Devices
- Compact alphanumeric keyboards or capacitive input devices.  

### 7.3 Long-Term Vision
- Transition from dev board prototype to a **custom PCB** with integrated power and ruggedized enclosure.  
