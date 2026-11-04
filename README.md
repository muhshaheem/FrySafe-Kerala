# FrySafe Kerala

**An open-source research platform for studying capacitive sensing of frying-oil degradation.**

FrySafe Kerala is an early-stage hardware and software research project. V1 investigates a low-cost capacitive sensing platform using a custom interdigitated electrode (IDE), Texas Instruments FDC1004 capacitance-to-digital converter, and Seeed Studio XIAO ESP32-S3.

The project is motivated by the widespread use of deep-frying in Kerala's food culture. Its intended contribution is openly documented experimental instrumentation, reproducible data collection, and a transparent validation process.

> [!WARNING]
> **Research prototype — not a food-safety instrument.** This design has not been calibrated against a laboratory reference method, independently validated, or approved by FSSAI or another regulator. Do not use it to certify food, declare an establishment unsafe, or make health claims.

## Project status

- **Stage:** V1 electronics and sensor-feasibility research
- **Maturity:** Experimental / not validated
- **Initial samples:** Edible oils for controlled bench characterization
- **Later research direction:** Food-derived lipid samples, only if a competent laboratory can establish a standardized extraction and reference-testing protocol.

## Why this project exists

Commercial instruments already measure total polar materials (TPM) or total polar compounds (TPC) in frying oil. FrySafe Kerala does not claim to invent that measurement. Its proposed contribution is an openly documented, lower-cost experimental platform with replaceable IDE geometries, accessible electronics and firmware, raw data, temperature logging, and a validation workflow.

The project's value will depend on measured performance and independent validation—not novelty of the concept alone.

## V1 architecture

```text
Controlled oil sample
       |
Replaceable IDE electrode
       |
TI FDC1004 capacitance converter
       | I2C
XIAO ESP32-S3 ------ SSD1306 OLED
       |
USB serial / CSV logging

10 kΩ NTC temperature sensor ---> ESP32-S3 ADC
```

The first milestone is to establish whether the custom electrode and measurement electronics produce stable, repeatable readings across controlled samples and temperatures. A capacitance change must not automatically be interpreted as oil degradation.

## Planned hardware

| Block | Component | Purpose |
|---|---|---|
| Controller | Seeed Studio XIAO ESP32-S3 | Acquisition and logging |
| Capacitance converter | TI FDC1004 | Capacitance measurement |
| Sensor | Custom PCB interdigitated electrode | Capacitive sensing interface |
| Temperature | 10 kΩ NTC + 10 kΩ fixed resistor | Temperature logging |
| Display | SSD1306 0.96-inch I2C OLED | Local readout |
| Sample holder | Glass chamber | Repeatable sample placement |

### Proposed connections

| Signal | XIAO ESP32-S3 | Peripheral |
|---|---|---|
| I2C SDA | D4 / GPIO5 | FDC1004 SDA, OLED SDA |
| I2C SCL | D5 / GPIO6 | FDC1004 SCL, OLED SCL |
| 3.3 V | 3V3 | FDC1004 VDD, OLED VCC |
| Ground | GND | Common GND |
| Thermistor divider | D0 / GPIO1 ADC | Divider midpoint |

This is a proposed allocation, not a substitute for checking the exact module pinout, PCB, and continuity before applying power. Follow the TI datasheet for FDC1004 CIN and shield connections. Do not connect the active shield output to ground.

## Electrode experiment

Initial custom PCB comparison:

| Variant | Finger width | Finger gap |
|---|---:|---:|
| A | 0.3 mm | 0.3 mm |
| B | 0.5 mm | 0.5 mm |
| C | 0.8 mm | 0.8 mm |

Initial mechanical target: approximately 12 mm × 12 mm active area, 10 mm finger length, and 8 fingers per comb. These are experimental starting dimensions, not optimized values.

Keep sample volume, electrode placement, chamber geometry, temperature, and measurement timing consistent. Avoid a grounded plane directly beneath active sensing fingers unless deliberately included as a controlled experiment.

The PCB electrode is not certified as food-contact material. Use only for controlled research samples; do not return measured samples to food.

## Firmware plan

PlatformIO with the Arduino framework. Initial firmware responsibilities:

1. Initialize I2C.
2. Verify FDC1004 manufacturer/device identification.
3. Configure a measurement channel and acquire readings.
4. Read the NTC ADC using the actual thermistor's manufacturer-specified beta value.
5. Display raw capacitance and temperature.
6. Stream timestamped CSV records over USB serial.

No TPC percentage, pass/fail threshold, or “safe/unsafe” classification belongs in V1.

## Experimental and validation plan

### Stage 1 — Electronics and repeatability

- Verify FDC1004 identity and stable I2C communication.
- Record an air baseline.
- Measure fresh edible oils as distinct sample types.
- Take repeated readings under controlled conditions.
- Compare IDE variants A, B, and C.
- Repeat at controlled temperatures and log temperature with every reading.
- Record sensor ID, electrode revision, sample ID, configuration, and conditions.

### Stage 2 — Reference-method comparison

Meaningful TPC calibration requires a competent food-testing laboratory and a suitable reference procedure. Seek a university food science/chemistry department or an appropriately accredited food-testing laboratory.

Lipid extracted from fried food is not automatically equivalent to the original fryer oil: selective absorption, water, food residues, and extraction conditions can affect the result. Any extraction method must be standardized and its bias studied. Do not attempt solvent extraction at home; it requires a suitable laboratory, risk controls, and trained supervision.

### Data principles

- Preserve raw readings.
- Log temperature and sensor configuration.
- Separate technical repeats from independent frying batches.
- Use independent samples for model evaluation.
- Publish failed experiments and limitations.
- Report uncertainty and the range of tested conditions.

## Safety and claims

FSSAI's TPC limit for used cooking oil is regulatory context for properly measured frying oil; it does not make this unvalidated prototype a regulatory test.

Until validated:
- Do not claim detection of cancer risk, kidney disease, toxins, or illness.
- Do not accuse a restaurant/vendor based on a prototype reading.
- Do not claim FSSAI approval, certification, or laboratory accuracy.
- Do not label samples “safe” or “unsafe.”
- Do not publish TPC percentages inferred from capacitance without a defensible calibration study.

## Roadmap

- [ ] Publish system block diagram and pin map.
- [ ] Add KiCad schematic and PCB source.
- [ ] Add PlatformIO firmware and build instructions.
- [ ] Verify FDC1004 communication on assembled hardware.
- [ ] Fabricate and document IDE variants A/B/C.
- [ ] Establish repeatability and temperature baselines.
- [ ] Publish raw CSV datasets with metadata.
- [ ] Find a laboratory partner for reference measurements.
- [ ] Evaluate whether food-derived lipid samples are meaningful.
- [ ] Publish validation results and limitations.

## Repository layout

See `hardware/`, `firmware/`, `docs/`, and `data/`. Files not yet implemented are intentionally marked as planned or placeholders.

## Contributing

Contributions are welcome in embedded firmware, KiCad design, sensor characterization, experimental design, documentation, and data analysis. Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening an issue or pull request.

## References

- Texas Instruments, [FDC1004 datasheet](https://www.ti.com/lit/ds/symlink/fdc1004.pdf).
- Seeed Studio, [XIAO ESP32-S3 documentation](https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/).
- Food Safety and Standards Authority of India, [official website](https://www.fssai.gov.in/).

Consult current official standards and reference methods before conducting a regulatory or food-safety study.

## License

MIT License. See [LICENSE](LICENSE).
