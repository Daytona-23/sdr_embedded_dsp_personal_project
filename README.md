# sdr_embedded_dsp_personal_project 
*last update 8/19/2026*

This repository is the main workspace for learning and experimenting with digital signal processing (DSP), embedded systems, and software-defined radio (SDR). The primary goal is to expand my current engineering skills by building a real-time communications receiver and progressively moving from high-level DSP experimentation toward lower-level embedded Linux, device-interface, and FPGA development.

The project is intentionally structured in phases so that each stage builds on the previous one. Initial development will focus on receiving and analyzing live RF signals on a Raspberry Pi 4. Later stages will add digital communications algorithms, optimized C/C++ processing, Linux hardware interfaces, and optional FPGA acceleration.

*key note: I will incorporate FPGA once my budget allows for extra funds to allocate to this side project*

## Project Goals

- Acquire live RF signals and process complex I/Q samples on embedded hardware
- Implement core DSP algorithms rather than relying entirely on prebuilt SDR processing blocks
- Build a real-time FM receiver before progressing to BPSK/QPSK digital communications
- Develop practical experience with filtering, multirate processing, synchronization, spectral analysis, and receiver performance metrics
- Explore real-time embedded topics including multithreading, buffering, latency, CPU utilization, and dropped-sample handling
- Interact with Linux hardware/device interfaces and develop basic driver-level functionality for SPI/GPIO-connected peripherals

## Project Details

### Current Hardware Utilized

- Raspberry Pi 4 — 4-core embedded Linux host and primary real-time DSP platform.
- USB SDR receiver & Antenna — Nooelec RTL-SDR v5 Bundle - NESDR Smart HF/VHF/UHF (100kHz-1.75GHz) Software Defined Radio. Premium RTLSDR w/ 0.5PPM TCXO, SMA Input, Aluminum Enclosure & 3 Antennas

#### Planned / Optional Hardware

- SPI / GPIO / UART connections — used for Raspberry Pi-to-peripheral or Raspberry Pi-to-FPGA communication.
- Logic analyzer / oscilloscope — optional tools for debugging digital interfaces, timing, and hardware behavior.
- FPGA development board — used for hardware implementation of DSP blocks such as an NCO, complex mixer, FIR filter, decimator, FIFO, and control registers.

### Software and Development Tools

#### Languages

- Python — DSP modeling, visualization, reference implementations, automated testing, and algorithm development.
- C/C++ — real-time SDR streaming, optimized DSP, multithreading, buffering, and embedded hardware interaction.
- Verilog / SystemVerilog — FPGA DSP implementation and hardware interface logic.

### Development Environment

- Linux / Raspberry Pi OS
- Git / GitHub
- GCC / G++
- CMake / Make
- Python scientific computing stack
- Linux shell and system profiling tools
- ModelSim / Questa or equivalent HDL simulator
- FPGA vendor synthesis/programming tools
- SDR / Linux Interfaces
- librtlsdr
- libusb
- GNU Radio for comparison, validation, and later rapid prototyping
- Linux utilities including lsusb, dmesg, lsmod, and udevadm

### Target System Architecture
```text
RF Signal
    |
    v
Antenna / RF Front End
    |
    v
SDR / ADC
    |
    v
Complex I/Q Samples
    |
    v
Raspberry Pi 4
    |
    +--> Acquisition / Buffering
    +--> FFT / Spectrum Analysis
    +--> Filtering / Decimation
    +--> FM or BPSK/QPSK Demodulation
    +--> Synchronization / Packet Processing
    |
    v
Decoded Audio / Data / Performance Metrics
```
### Later FPGA integration may move high-throughput processing closer to the sample source:
```text
SDR / Sample Source
        |
        v
+---------------------+
|        FPGA         |
| NCO / Mixer         |
| FIR Filter          |
| Decimator           |
| FIFO                |
+----------+----------+
           |
       SPI / GPIO
           |
           v
+---------------------+
|   Raspberry Pi 4    |
| Linux Interface     |
| Receiver DSP        |
| Demodulation        |
| Analysis / Logging  |
+---------------------+
```
## DSP and Communications Topics

The project will be used to develop practical experience with:

- Complex I/Q signal representation
- FFT and power spectral density analysis
- Windowing and spectral leakage
- RF channel identification
- Signal power and noise-floor estimation
- Digital frequency translation
- Numerically controlled oscillators (NCOs)
- FIR filtering
- Decimation and resampling
- FM demodulation
- Automatic gain control concepts
- BPSK and QPSK
- Pulse shaping and matched filtering
- Carrier and frequency recovery
- Symbol timing recovery
- Constellation analysis
- Packet / bitstream decoding

### Embedded Linux / Driver Topics

As the project progresses, the Raspberry Pi will also be used to explore lower-level embedded concepts:

- USB device enumeration and data flow
- Asynchronous device I/O
- Producer/consumer processing pipelines
- Ring buffers
- Multithreading and CPU affinity
- Real-time throughput and latency measurements
- SPI and GPIO interfaces
- Linux character/device interfaces
- read(), write(), and ioctl() concepts
- Hardware interrupts and FIFO thresholds
- Memory mapping
- DMA and zero-copy concepts

### FPGA Extension (if budget allows)

The optional FPGA stage will focus on implementing a digital downconversion and preprocessing chain in RTL:
```text
Input Samples
     |
     v
Phase Accumulator / NCO
     |
     v
Complex Mixer
     |
     v
FIR Low-Pass Filter
     |
     v
Decimator
     |
     v
FIFO
     |
     v
Raspberry Pi
```
The FPGA interface may expose configurable registers for items such as:

- Center / tuning frequency
- DSP enable and reset controls
- Decimation factor
- Filter configuration
- Status flags
- FIFO level

The FPGA and Raspberry Pi implementations can then be compared in terms of throughput, latency, CPU utilization, numerical accuracy, and FPGA resource usage.

## Project Milestones

### *Phase 1 — SDR Fundamentals*

- Configure the Raspberry Pi and SDR hardware
- Capture complex I/Q samples
- Build a basic spectrum analyzer
- Implement FFT/PSD visualization and signal measurements
- Characterize sample rate, gain, noise floor, and dropped samples

### *Phase 2 — Software DSP Receiver*

- Implement digital frequency translation
- Design and implement FIR channel filters
- Add decimation/resampling
- Build an FM receiver and recover audio
- Extend the receiver toward BPSK/QPSK signals
- Implement matched filtering and receiver synchronization

### *Phase 3 — Embedded Linux Development*

- Move performance-critical processing into C/C++ where appropriate
- Implement multithreaded acquisition and DSP pipelines
- Profile CPU load, latency, and sample loss
- Experiment with CPU affinity and buffering strategies
- Develop a simple SPI/GPIO hardware interface or Linux device-driver component

### *Phase 4 — FPGA Integration*

- Implement and simulate an FPGA DDC chain
- Validate RTL against a Python software reference model
- Implement FIFO and Raspberry Pi control/data interfaces
- Integrate FPGA preprocessing with the Raspberry Pi receiver
- Benchmark software-only versus FPGA-accelerated implementations

## Performance / Validation Metrics

Results will be evaluated using measurable engineering metrics where applicable:

- SNR
- Noise floor
- Occupied bandwidth
- Carrier-frequency offset
- FFT / PSD results
- Constellation quality
- BER vs. $E_b/N_0$
- CPU utilization
- Processing latency
- Dropped samples / buffer overruns
- FPGA resource utilization
- Hardware vs. software numerical accuracy

### Success Criteria

A successful implementation should demonstrate:

- Stable real-time SDR acquisition on the Raspberry Pi without sustained sample loss
- DSP blocks validated against Python/reference implementations
- Successful FM demodulation and/or correct recovery of digital communication symbols
- Quantitative receiver-performance measurements rather than only visual demonstrations
- A documented embedded hardware/software interface
- If FPGA integration is completed, a measurable comparison between software and FPGA implementations
  

## Planned Repository Structure
```text
sdr_embedded_dsp_personal_project/
|
+-- README.md
+-- docs/
|   +-- architecture.md
|   +-- project_notes.md
|   +-- results.md
|
+-- src/
|   +-- python/
|   +-- cpp/
|   +-- receiver/
|
+-- drivers/
|
+-- rtl/
|   +-- src/
|   +-- tb/
|
+-- simulations/
+-- tests/
+-- scripts/
+-- data/
+-- results/
|   +-- figures/
|   +-- logs/
|
+-- CMakeLists.txt
+-- .gitignore
```
*Project Status: Initial planning and SDR/Antenna acquisition (already have Raspberry Pi 4) 8/19/2026*
