# Project 3 — Custom PCB Environmental Sensor Node

**Status:** ⏳ Not started (due September 1, 2026)

## Overview

A custom-fabricated PCB built around an ATmega328P microcontroller, reading temperature and humidity from a DHT22 sensor and displaying live readings on an SSD1306 OLED display over I2C. This project moves beyond breadboard prototyping into full schematic capture, PCB layout, and fabrication via JLCPCB.

## Planned Components

- ATmega328P-PU (DIP-28, bootloaded)
- DHT22 / AM2302 temperature & humidity sensor
- SSD1306 0.96" OLED display (I2C)
- 16MHz crystal + 22pF load capacitors
- L7805CV 5V voltage regulator

## Planned Process

1. Ch. 11 (Voltage Regulators & Power Supplies) + Ch. 13 (Microcontrollers) reading
2. Ch. 10 §10.7 (crystal oscillator + load caps) standalone review
3. KiCad schematic capture
4. KiCad PCB layout
5. Gerber file generation and JLCPCB fabrication order (hard deadline: Aug 12–18)
6. Board population and soldering
7. Firmware: DHT22 read + I2C OLED display driver

## Files

*(To be added as the project progresses: KiCad project files, Gerber exports, firmware source, photos.)*
