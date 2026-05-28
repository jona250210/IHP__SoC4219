# SoC4219 (TinyWhisper): An Open-Source Fully-Integrated Multi-Mode Short-Wave Transmitter for Amateur Radio Applications in 130nm CMOS
(c) 2025-2026 Simon Dorrer (OE3SDE), Jonathan Hager (DK7JH), Matthias Jung (DL9MJ) and Harald Pretl

Single-technology IP library.

- doc/     : user documentation. The full documentation of the TinyWhisper transmitter is available [here](https://iic-jku.github.io/TinyWhisper/index.html).
- dependencies/ : sub-cells and blocks
- release/v.1.0.0 : immutable versioned deliveries
- design files: All design files of the TinyWhisper transmitter are available [here](https://github.com/iic-jku/TinyWhisper).

## Purpose

TinyWhisper demonstrates what is possible with current open-source tools and open-source PDKs, from system design to final tapeout, from packaging to PCB design, and even to a 3D-printed enclosure. It can also be used for:

- Ham radio courses
- University courses
- Regression tests of various open-source tools


## Chip Specifications

| Parameter           | Value                                                                             |
| ------------------- | --------------------------------------------------------------------------------- |
| Technology          | IHP SG13CMOS (130 nm CMOS)                                                        |
| Die Area            | 2000 × 2000 µm (4 mm²)                                                            |
| Core Area           | 1270 × 1270 µm (1.613 mm²)                                                        |
| Clock Frequency     | 56 MHz                                                                            |
| Core Supply Voltage | 1.5 V                                                                             |
| I/O Supply Voltage  | 3.3 V                                                                             |
| Total Pads          | 56 (8 analog, 6 input, 9 output, 9 bidirectional, 7 VDD, 9 VSS, 3 IOVDD, 3 IOVSS) |
| Packaging           | QFN-48 package → 8 pads are connected directly to package VSS                     |
| Temperature Range   | -40 °C to +125 °C                                                                 |


## Overview

- Analog Front-End — IQ Modulator
    - 3rd-order multiple-feedback (MFB) low-pass filter
        - Butterworth
        - fc = 400 kHz
        - Barthelemy / Manfredini (B/M) inverter-based OTA
    - Passive CMOS voltage-mode mixer
        - Single-to-differential conversion circuit
        - Simple transmission-gate switch
- Digital Core
    - RISC-V CPU
        - SPI interface for external SRAM extension
        - I²C interface for external peripherals (I/O extender, display, keyboard, etc.)
        - UART interface for connection to PC / Raspberry Pi RP2040 + MicroPython
        - 8 × GPIOs (4 × inputs, one with external interrupt capability, and 4 × outputs)
    - 30-bit iterative CORDIC for sine / cosine generation
        - Adjustable amplitude
        - Adjustable fc, fm, and phim enable different modulation schemes
        - Frequency resolution: df = fclk / 2^(n) = fclk / 2^(30) ~ 0.05 Hz
        - Maximum frequency: fc = 2 × OSR × frequency / fclk
    - Delta-Sigma modulator
        - OSR = 32 / 64 / 128 / 256
        - 1st / 2nd order
        - Inversion / exchange of the I/Q channel
    - 25% LO Generation for Mixer
        - DIV = 1 / 2 / 4 / 8
        - Debugging modes (mixer TG always on / off → fully differential measurement of the MFB filter is possible)
    - Debugging features
        - Bidirectional pads: measure DSM / LO output or inject signals externally
        - Analog pads: inject analog IQ modulator signals externally

## Acknowledgements

Thanks to Chance Reimer for providing an open source [I2C module](https://github.com/chance189/I2C_Master) and to Ben Marshall for providing a [UART implementation](https://github.com/ben-marshall/uart).

This project is funded by the JKU/SAL [IWS Lab](https://research.jku.at/de/projects/jku-lit-sal-intelligent-wireless-systems-lab-iws-lab/), a collaboration of [Johannes Kepler University](https://jku.at) and [Silicon Austria Labs](https://silicon-austria-labs.com).

<p align="center">
  <a href="https://silicon-austria-labs.com" target="_blank">
    <img src="https://github.com/iic-jku/klayout-pex-website/raw/main/figures/funding/silicon-austria-labs-logo.svg" alt="Silicon Austria Labs" width="300"/>
  </a>
</p>
