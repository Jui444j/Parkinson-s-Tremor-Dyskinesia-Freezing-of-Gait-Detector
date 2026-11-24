# Parkinson's Tremor, Dyskinesia and Freezing of Gait Detector

## Project Overview

A real-time embedded movement detection system developed using the STM32 ARM Cortex-M4 microcontroller. The system monitors motion patterns associated with Parkinson's disease using an accelerometer and gyroscope, digital signal processing, frequency-domain analysis, and Bluetooth Low Energy (BLE).

This project was developed as a final project for Real-Time Embedded Systems at NYU Tandon School of Engineering.

## Key Features

- Real-time accelerometer and gyroscope data acquisition.
- Interrupt-driven sampling at 52 Hz with polling fallback.
- FFT-based tremor and dyskinesia detection.
- Freezing of Gait (FOG) detection using a finite-state machine.
- Adaptive noise thresholds and frequency-band analysis.
- Exponential Moving Average (EMA) for detection stability.
- Bluetooth Low Energy communication through GATT services.
- LED-based movement status and intensity indication.
- Modular embedded firmware architecture.

## Hardware and Software

| Component | Specification |
|---|---|
| Development Board | STM32 DISCO-L475VG-IOT01A |
| Processor | ARM Cortex-M4 |
| Motion Sensor | LSM6DSL Accelerometer and Gyroscope |
| Programming Language | Embedded C/C++ |
| Embedded Framework | Mbed OS |
| Development Environment | PlatformIO |
| DSP Library | ARM CMSIS-DSP |
| Sensor Communication | I2C — 400 kHz |
| Wireless Communication | Bluetooth Low Energy |
| Debugging Interface | ST-LINK |
| Serial Baud Rate | 115200 |

## System Architecture

The system collects motion data from the LSM6DSL sensor, processes the signals on the STM32 microcontroller, identifies movement patterns, and communicates detection results through BLE, LED indications, and the serial console.

### Digital Signal Processing

| Parameter | Configuration |
|---|---|
| Sampling Frequency | 52 Hz |
| Window Duration | 3 seconds |
| Samples per Window | 156 |
| FFT Size | 256 points |
| Window Function | Hann Window |
| Accelerometer Weight | 70% |
| Gyroscope Weight | 30% |
| EMA Alpha | 0.3 |
| Detection Confirmation | 3 consecutive windows |

The signal-processing pipeline performs DC removal, normalization, weighted sensor-data combination, Hann windowing, zero-padding, FFT analysis, and adaptive threshold-based detection.

### Movement Detection

| Movement Pattern | Detection Method |
|---|---|
| Tremor | Frequency analysis within 3–5 Hz |
| Dyskinesia | Frequency analysis within 5–7 Hz |
| Freezing of Gait | Step detection, cadence, motion variance, and state transitions |

The FOG algorithm uses a finite-state machine to distinguish between non-walking, walking, potential freezing, and confirmed freezing states.

These detection parameters are experimental thresholds implemented for the prototype.

## Bluetooth Low Energy

The system advertises under the device name `PD_Detector` and exposes three GATT characteristics for real-time monitoring.

| Characteristic | Output |
|---|---|
| Tremor Intensity | Nominal 0–1000 scale |
| Dyskinesia Intensity | Nominal 0–1000 scale |
| FOG Status | 0 — Normal, 1 — Detected |

The characteristics support BLE read and notification operations.

## LED Indication

| Condition | LED Pattern |
|---|---|
| Normal | 0.5 Hz heartbeat |
| Tremor | 2 Hz blinking |
| Dyskinesia | 4 Hz blinking |
| Freezing of Gait | Triple-blink alarm |

## Firmware Architecture

| Source File | Function |
|---|---|
| `main.cpp` | System initialization and main execution loop |
| `sensor.cpp` | Sensor configuration, interrupts, and data acquisition |
| `signal_processing.cpp` | FFT, frequency analysis, and movement classification |
| `fog_detection.cpp` | Step tracking and FOG state machine |
| `ble_comm.cpp` | BLE advertising, GATT services, and data transmission |
| `led_control.cpp` | Visual status indication |
| `config.cpp` / `config.h` | System configuration and detection parameters |

## Build and Deployment

1. Install Visual Studio Code and PlatformIO.
2. Open the `pd-detect` directory as a PlatformIO project.
3. Connect the STM32 development board through ST-LINK.
4. Build the firmware using `pio run`.
5. Upload the firmware using `pio run -t upload`.
6. Open the serial monitor using `pio device monitor`.
7. Connect to `PD_Detector` using a compatible BLE application.

## Project Demonstration

[Watch the project demonstration](./rtes_demonstration.mp4)

The demonstration covers the embedded movement detection system, sensor-based signal processing, and real-time output.

## Technical Skills Demonstrated

- Embedded C/C++ Programming
- ARM Cortex-M4 and STM32 Development
- Real-Time Embedded Systems
- Digital Signal Processing and FFT
- Accelerometer and Gyroscope Integration
- Interrupt Service Routines
- Finite-State Machine Design
- I2C Communication
- Bluetooth Low Energy and GATT
- Mbed OS and PlatformIO
- Embedded Firmware Debugging
- IoT and Biomedical Embedded Systems

## Disclaimer

This project is an educational engineering prototype. It is not a clinically validated medical device and is not intended for medical diagnosis or treatment.
