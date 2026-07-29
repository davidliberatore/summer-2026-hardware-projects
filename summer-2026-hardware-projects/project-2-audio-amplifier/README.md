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

Project 2: Two-Stage Audio Amplifier

A two-stage audio amplifier built from a TL072 op-amp non-inverting pre-amplifier and a discrete BJT class-AB push-pull output stage — takes a phone's headphone-level audio signal and drives an 8Ω speaker.

Timeline: July 12 – July 29, 2026

Overview

Designed, simulated, and built a two-stage audio amplifier:

Pre-amp stage — TL072 op-amp in a non-inverting configuration, providing ~8x voltage gain
Output stage — discrete 2N3904/2N3906 class-AB push-pull stage, providing the current drive needed to move a real speaker

Every stage was verified in LTspice before any hardware was built, then translated to a breadboard build with full DC and AC verification via a TRMS multimeter.

Circuit Design
Stage 1 — Pre-amp (TL072, non-inverting)
Single +5V supply (Elegoo Uno rail)
R3 = R4 = 100kΩ — bias divider setting the input at Vcc/2 (2.5V), required for single-supply operation
C1 = 1µF — input coupling cap (blocks DC, passes audio)
R1 = 4.7kΩ, R2 = 33kΩ — feedback network setting gain ≈ 1 + (R2/R1) ≈ 8x (18dB)
Built with kit-available resistors in series: R1 = 2k + 2k + 330 + 330 = 4.66kΩ; R2 = 10k + 10k + 10k + 2k + 1k = 33kΩ (exact)
C2 = 10µF — DC bypass across R1 (splits DC gain = 1 from AC gain = 8x)
Stage 2 — Push-pull output (2N3904 NPN / 2N3906 PNP, class AB)
Q1 collector → +V, Q2 collector → GND (current source/sink for each half-cycle)
Bias network: two 1kΩ resistors + two diodes (1N4007, substituted for 1N4148) in series between Q1 and Q2's bases — sets a permanent 1.2V gap that pre-biases both transistors to the edge of conduction, eliminating the crossover dead zone
AC input coupled directly to Q1's base (not the diode midpoint) via a 100µF cap
100µF bypass cap across the diode pair — gives the AC signal a low-impedance path to Q2's base, skipping the diodes' nonlinear resistance
Output coupled to the speaker via a 100µF cap (blocks the ~2.5V DC bias, passes audio only)
Load: CQRobot 8Ω/3W speaker
Process
Theory — op-amp virtual ground/negative feedback, BJT NPN/PNP turn-on conditions and current direction, class A/B/AB output stages
Pre-amp simulation (LTspice) — built and debugged the non-inverting stage; verified gain two independent ways (transient: 1.6Vpp; AC sweep: 18.09dB vs. calculated 18dB)
Push-pull simulation, bare class B first — deliberately unbiased, to directly observe crossover distortion (output flat-lined across the dead zone where neither transistor had enough Vbe to conduct)
Push-pull simulation, class AB — added the diode bias network and debugged three real issues in sequence (see Lessons Learned)
Breadboard build, Stage 1 — translated verified LTspice values to hardware, verified all 7 DC nodes before connecting audio input
Breadboard build, Stage 2 — built and integrated the push-pull stage, verified all DC nodes, connected live phone audio and confirmed working sound
Gain + distortion verification — measured stage-by-stage AC gain with a steady test tone, confirmed crossover distortion absent by listening test
Results
Node	Predicted	Measured
Pre-amp bias midpoint	~half of rail	2.176V
TL072 pin 3 (+)	match midpoint	2.176V
TL072 pin 2 (−)	match pin 3 (virtual short)	2.185V
TL072 pin 1 (output, quiescent)	match bias point	2.190V
Q1 base (push-pull)	~2.7–2.8V	2.869V
Q2 base (push-pull)	~1.4–1.5V	1.593V
Push-pull output node	~2.2–2.3V	2.225V
Live audio confirmed end-to-end: phone → pre-amp → push-pull → speaker, clean recognizable sound at moderate volume
Gain measured with a steady test tone: pre-amp ≈ 7.9x; push-pull stage ≈ 0.1–0.18x (well below ideal unity) — explained by the emitter-follower gain formula R_load/(R_load + re), where re ≈ 26mV/Ic is non-negligible at the deliberately small ~1.9mA class-AB bias current relative to the ~10Ω speaker load. A known, real tradeoff between idle efficiency and output impedance matching — not a fault
Crossover distortion: confirmed absent by listening test at low/moderate volume, consistent with LTspice-verified bias network and DC measurements
Volume-dependent clipping identified: at full phone volume, pre-amp output swings ~0.7–1.0V peak against ~±0.7V of available headroom on the single 4.4V rail — a real, characterized supply-headroom limitation, confirmed via AC voltage comparison at two volume levels
Lessons Learned
Single-supply circuits can't swing through 0V. A transistor's collector fixed at GND creates a hard floor — the whole signal has to live above it, which is why both stages bias around Vcc/2.
AC injection point matters as much as DC bias point. Injecting a signal at a node between two diodes pushed those diodes into reverse-bias cutoff at the swing extremes, even with a correct DC bias — traced via literal anode/cathode voltage inequalities, not just "it looks wrong."
A fix for one problem can be incomplete. Relocating the AC injection point solved the primary cutoff bug but left a subtler secondary issue (signal degradation through two series diode junctions) that only a bypass capacitor fully resolved.
Emitter follower gain isn't automatically ~1. It depends on load impedance vs. the transistor's internal re, which depends on bias current — a class-AB stage optimized for idle efficiency will show reduced voltage gain into a low-impedance load.
AC measurements need a steady signal source. Music's constantly-changing loudness makes point-to-point voltage comparisons meaningless unless both readings are taken at the same instant.
Component substitution is a normal part of prototyping. Resistor series combinations and a 1N4007-for-1N4148 substitution both worked, with documented, calculated tolerance impact.
Skills Demonstrated

Op-amp non-inverting amplifier design · negative feedback and virtual short · single-supply biasing (Vcc/2 technique) · BJT class-B/class-AB push-pull output stage design · crossover distortion diagnosis and correction · diode biasing networks · LTspice simulation and iterative circuit debugging · breadboard prototyping · systematic DC/AC verification with a TRMS multimeter · component substitution and tolerance analysis · root-cause debugging across simulation and hardware
