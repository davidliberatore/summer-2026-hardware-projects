# Project 1 — PWM LED Dimmer

**Status:** ✅ Complete (June 17 – July 6, 2026)

## Overview

Designed, simulated, and built a pulse-width-modulation (PWM) LED dimmer using an NE555 timer IC in astable mode. The circuit generates a continuous square wave whose duty cycle sets the LED's perceived brightness — turning a potentiometer adjusts the brightness in real time, the same principle used in real-world dimmable LED drivers.

## Circuit Design

- **Topology:** NE555 astable multivibrator, RC timing network, LED with current-limiting resistor
- **Components:**
  - Ra = 1kΩ (fixed)
  - Rb = 10kΩ (potentiometer, wired as a 2-terminal variable resistor via wiper-to-outer-leg short)
  - C = 100nF (timing capacitor)
  - 220Ω LED current-limiting resistor
- **Governing equations:**
  - Frequency: `f = 1.44 / ((Ra + 2Rb) × C)`
  - Duty cycle: `D = (Ra + Rb) / (Ra + 2Rb)`
- **Design point:** at Rb = 10kΩ (pot maxed), f ≈ 686Hz, D ≈ 52%

## Process

1. **Theory** — Read the 555 timer datasheet and *Practical Electronics for Inventors* Ch. 10 (Oscillators & Timers) to understand astable mode and the duty cycle/frequency formulas.
2. **Simulation** — Built and verified the circuit in LTspice, confirming simulated frequency and duty cycle matched hand calculations.
3. **Breadboard verification (software)** — Rebuilt the same circuit in Tinkercad to check wiring logic before touching real hardware, catching and correcting several DIP-8 pin-mapping misunderstandings at this stage.
4. **Physical build** — Transferred the verified wiring 1:1 onto a real Elegoo UNO + breadboard, powered from the UNO's 5V/GND pins.
5. **Verification** — Used an AstroAI TRMS multimeter in DC voltage mode to measure the average output voltage at pin 3 (OUT) across the potentiometer's full range, confirming the dimmer's brightness control works as designed.

## Results

- LED glows at a steady, adjustable brightness (686Hz oscillation is far above the ~60–90Hz flicker-fusion threshold of human vision, so the eye perceives only the averaged brightness, not individual pulses).
- Turning the potentiometer produces a clear, monotonic change in LED brightness and in measured average voltage at pin 3.
- Measured Vout (pin 3–GND) ranged **2.1V–3.6V** (LED branch disconnected) and **1.65V–2.85V** (LED branch connected) across the pot's full sweep, against a **4.4V** measured rail (vs. 5V ideal).
- Measured values sit below the idealized prediction (~2.29V–4.4V) due to two compounding, well-understood real-world effects: (1) the LED branch's current draw loading down pin 3's output, and (2) the bipolar NE555's non-ideal output stage, which doesn't swing perfectly rail-to-rail (unlike CMOS variants such as the TLC555).

## Lessons Learned

- **DIP-8 pin mapping:** pins 1↔8, 2↔7, 3↔6, 4↔5 are mirrored pairs — same row, opposite sides of the breadboard's center gap — and are NOT automatically connected. Bridging any of these pairs (used for tying TRIG/pin 2 to THR/pin 6) requires an explicit jumper wire.
- **Breadboard row/column logic:** any two component legs sharing a row (within the same a–e or f–j group, same side of the gap) are automatically connected; crossing the gap or landing in a different column group always requires a jumper.
- **Potentiometers need one extra wiring step** versus fixed resistors: the wiper (middle leg) must be shorted to one outer leg to turn a 3-terminal pot into a simple 2-terminal variable resistor for this application.
- **A real debugging catch:** initially seated the pot with two legs in the same row, accidentally shorting them and breaking its variable behavior — caught by tracing leg positions carefully and confirmed with the multimeter's continuity mode before wiring further.
- **Simulation vs. reality gap:** real measured voltages never match idealized hand-calc numbers exactly — rail voltage sag, component tolerances, and non-ideal IC output behavior all contribute. Learning to explain *why* a real measurement differs from theory (rather than assuming something is wrong) is as important a skill as the circuit design itself.

## Skills Demonstrated

PWM generation and duty-cycle control · RC timing network design · 555 timer astable mode · LTspice simulation · breadboard prototyping · potentiometer wiring · multimeter-based verification (DC voltage, continuity) · systematic debugging · reconciling theoretical predictions with real-world measurements

## Resume Line

> Designed and built a PWM LED dimmer using a 555 timer and RC filter; verified adjustable duty cycle via TRMS multimeter, measuring output voltage across a 1.65V–2.85V range as the potentiometer varied Rb.

## Files

- `pwm_led_dimmer.asc` — LTspice schematic (add your file here)
- `photos/` — breadboard build photos, multimeter readings
