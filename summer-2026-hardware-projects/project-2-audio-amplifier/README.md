# Project 2 — Audio Amplifier

**Status:** 🔧 In progress (due July 28, 2026)

## Overview

A two-stage audio amplifier: a TL072 op-amp non-inverting pre-amp stage feeding a 2N3904/2N3906 BJT push-pull (class B/AB) output stage, driving an 8Ω speaker. The design demonstrates small-signal voltage gain paired with a current-boosting output stage capable of driving a low-impedance load — the same basic architecture used in real audio amplifier front ends.

## Circuit Design — Pre-Amp Stage

- **Topology:** TL072 op-amp, non-inverting configuration, single +5V supply
- **Components:**
  - R1 = 4.7kΩ, R2 = 33kΩ → gain ≈ 8 (≈18dB)
  - R3 = R4 = 100kΩ — bias divider, sets the input at Vcc/2 (2.5V) so the single-supply op-amp has headroom to swing both ways
  - C1 = 1µF — AC coupling capacitor
  - C2 = 10µF — DC bypass capacitor, in series with R1
- **Why single-supply biasing works:** R3/R4 sit in parallel for AC signals (both Vcc and GND behave as AC ground) but in series for DC, which is what lets them set a stable DC bias point without attenuating the AC signal.
- **Why C2 matters:** at DC, C2 blocks the path through R1, so no current flows through R2 and Vout = Vin− = 2.5V (the bias point, no gain). At AC, C2's impedance is negligible, so current flows through R1 and is forced through R2 by KCL — this is what produces the actual voltage gain of 1 + R2/R1.

## Circuit Design — Output Stage

- **Topology:** 2N3904 (NPN) / 2N3906 (PNP) complementary push-pull output stage
- **Load:** CQRobot 3W 8Ω passive speaker

## Process

1. **Theory** — Covered op-amp virtual ground and negative feedback (inverting gain via KCL at the virtual ground node; non-inverting gain via voltage divider), then BJT structure and turn-on conditions (Ch. 4).
2. **Pre-amp simulation** — Built and verified the TL072 pre-amp in LTspice. Debugged 4 sequential bugs before reaching a working circuit; verified via both transient analysis (1.6Vpp output) and AC analysis (18.09dB gain, matching the ≈18dB hand calculation).
3. **Push-pull simulation** — LTspice sim of the BJT output stage, checked for crossover distortion.
4. **Breadboard build** — Pre-amp stage breadboarded first, then the push-pull output stage integrated with it; gain and crossover distortion measured on real hardware.

## Results

*(Fill in after breadboard integration — gain measurement, crossover distortion observations, any debugging notes.)*

## Lessons Learned

- **AC/DC split in single-supply biasing:** the same resistor network can look like two entirely different circuits depending on whether you're doing a DC or AC analysis — R3/R4 in series for DC bias-point calculations, but in parallel for AC gain calculations, because Vcc and GND are both AC ground.
- **Coupling vs. bypass capacitors serve different roles:** C1 blocks DC offset from reaching the next stage; C2 sets where the gain "turns on" as frequency increases, by controlling when the R1 leg becomes a real AC path.
- *(Add push-pull / crossover distortion lessons after breadboard integration.)*

## Skills Demonstrated

Op-amp non-inverting amplifier design · single-supply biasing · AC/DC circuit analysis · coupling and bypass capacitor design · BJT push-pull output stages · LTspice simulation (transient + AC analysis) · systematic multi-bug debugging · breadboard prototyping

## Resume Line

*(Draft after breadboard build — see Project 1's README for the target format.)*

## Files

- `preamp_sim.asc` — TL072 pre-amp LTspice schematic
- `pushpull_sim.asc` — BJT push-pull output stage LTspice schematic
- `photos/` — breadboard build photos, scope/multimeter readings
