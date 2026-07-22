# Summer 2026 Hardware Projects

Three hardware projects built over Summer 2026 to develop practical analog and embedded electronics skills ahead of EE co-op interviews. Each project moves through the same pipeline: theory → LTspice/KiCad simulation → breadboard (or PCB) build → measured verification.

Background: EE sophomore at Drexel University (transferred from CS), applying a Python/Flask software background and Physics 102 coursework to hands-on circuit design.

## Projects

| # | Project | Status | Summary |
|---|---------|--------|---------|
| 1 | [PWM LED Dimmer](./project-1-pwm-led-dimmer) | ✅ Complete | 555 timer astable oscillator + RC filter, adjustable duty cycle via potentiometer |
| 2 | [Audio Amplifier](./project-2-audio-amplifier) | 🔧 In progress | TL072 op-amp pre-amp + 2N3904/2N3906 BJT push-pull output stage, 8Ω speaker |
| 3 | [Custom PCB Environmental Sensor Node](./project-3-env-sensor-node) | ⏳ Planned | ATmega328P + DHT22 + SSD1306 OLED, custom fabricated PCB |

Each project folder contains a README with circuit design details, the design/debug process, results, and lessons learned, along with LTspice/KiCad source files and build photos.

## Tools Used

LTspice (simulation) · KiCad (PCB design) · Tinkercad (breadboard wiring verification) · JLCPCB (fabrication) · AstroAI TRMS multimeter (measurement/verification)
