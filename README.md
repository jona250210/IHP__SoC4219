# SoC4219 (TinyWhisper): An Open-Source Fully-Integrated Multi-Mode Short-Wave Transmitter for Amateur Radio Applications in 130nm CMOS
(c) 2025-2026 Simon Dorrer (OE3SDE), Jonathan Hager (DK7JH), Matthias Jung (DL9MJ) and Harald Pretl

Single-technology IP library.

- doc/     : user documentation. The full documentation of the TinyWhisper transmitter is available [here](https://github.com/iic-jku/TinyWhisper) (currently not public).
- dependencies/ : sub-cells and blocks
- release/v.1.0.0 : immutable versioned deliveries

## Purpose

TinyWhisper shows what is possible with current open-source tools and open-source PDKs. From system design to final tapeout, from packaging to PCB design and 3D-printed enclosure. It can be further used for:

- Ham radio courses
- University courses
- Regression tests of various open-source tools

## Chip Specifications

| Parameter           | Value                                                                             |
| ------------------- | --------------------------------------------------------------------------------- |
| Technology          | IHP SG13G2 (130nm CMOS)                                                 |
| Die Area            | 2000 × 2000 µm (4 mm²)                                                            |
| Core Area           | 1270 × 1270 µm (1.613 mm²)                                                        |
| Clock Frequency     | 56 MHz                                                                            |
| Core Supply Voltage | 1.5 V                                                                             |
| I/O Supply Voltage  | 3.3 V                                                                             |
| Total Pads          | 56 (8 analog, 6 input, 9 output, 9 bidirectional, 7 VDD, 9 VSS, 3 IOVDD, 3 IOVSS) |
| Packaging           | QFN-48 package → 8 pads are connected to package VSS directly                     |
| Temperature Range   | -40°C to +125°C                                                                   |

### Supported Ham Radio Bands

| Band | Frequency   | LO Frequency | Pre-Divider |
| ---- | ----------- | ------------ | ----------- |
| 80m  | 3.5686 MHz  | 7 MHz        | ÷8          |
| 40m  | 7.0386 MHz  | 14 MHz       | ÷4          |
| 20m  | 14.0956 MHz | 28 MHz       | ÷2          |
| 10m  | 28.1246 MHz | 56 MHz       | ÷1          |

## Overview

- Analog Front-End — IQ Modulator
    - 3rd-Order Multiple Feedback (MFB) Lowpass Filter
        - Butterworth
        - fc = 400kHz
        - Barthelemy / Manfredini (B/M) Inverter-Based OTA
    - Passive CMOS Voltage-Mode Mixer
        - Single2Differential conversion circuit
        - Simple Transmission Gate Switch
- Digital Core
    - RISC-V CPU
        - SPI interface for external SRAM extension
        - I²C interface for external peripherals (I/O extender, display, keyboard, etc.)
        - UART interface for connection to PC / Raspberry Pi RP2040 + MicroPython
        - 8 × GPIOs (4 x inputs (one for external interrupt), 4 x outputs)
    - 30-Bit Iterative CORDIC for Sine / Cosine Generation
        - Adjustable amplitude
        - Adjustable fc, fm and phim enable different modulation schemes
        - Frequency resolution: df = fclk / 2^(n-1) = fclk / 2^(29) ~ 0.1Hz
        - Frequency maximum: fc = 2 x OSR x frequency / fclk
    - 1st- / 2nd-Order Delta-Sigma Modulator
        - OSR = 32 / 64 / 128 / 256
        - 1st / 2nd order
        - Inversion / exchange of the I/Q channel
    - 25% LO Generation for Mixer
        - DIV = 1 / 2 / 4 / 8
        - Debugging modes (mixer TG always on / off → fully-differential measurement of MFB filter is possible)
    - Debugging modes
        - Bidirectional pads: measure DSM / LO output or inject it externally
        - Analog pads: inject analog IQ modulator externally


## Cite This Work

```
@software{JKU_JMU_2026_TinyWhisper,
	author = {Dorrer, Simon and Hager, Jonathan and Jung, Matthias and Pretl, Harald},
	month = ToDo,
	title = {{GitHub Repository for TinyWhisper: An Open-Source Fully-Integrated Multi-Mode Short-Wave Transmitter for Amateur Radio Applications in 130nm CMOS}},
	url = {https://github.com/iic-jku/TinyWhisper},
	doi = {ToDo},
	year = {2026}
}
```

## Acknowledgements
Thanks to Chance Reimer for providing an open source [I2C module](https://github.com/chance189/I2C_Master) and to Ben Marshall for providing a [UART implementation](https://github.com/ben-marshall/uart).

This project is funded by the JKU/SAL [IWS Lab](https://research.jku.at/de/projects/jku-lit-sal-intelligent-wireless-systems-lab-iws-lab/), a collaboration of [Johannes Kepler University](https://jku.at) and [Silicon Austria Labs](https://silicon-austria-labs.com).

<p align="center">
  <a href="https://silicon-austria-labs.com" target="_blank">
    <img src="https://github.com/iic-jku/klayout-pex-website/raw/main/figures/funding/silicon-austria-labs-logo.svg" alt="Silicon Austria Labs" width="300"/>
  </a>
</p>
