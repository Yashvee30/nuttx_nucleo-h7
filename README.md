# NuttX RTOS on STM32 for Drone Drivers

![RTOS](https://img.shields.io/badge/RTOS-Apache%20NuttX-blue)
![MCU](https://img.shields.io/badge/MCU-STM32-green)
![Language](https://img.shields.io/badge/Language-C-orange)

---

## Overview
This project demonstrates the use of Apache NuttX RTOS on an STM32 microcontroller, with a focus on building low-level drivers and real-time task execution relevant to drone and robotics systems.

The implementation emphasizes deterministic scheduling, hardware-level control, and modular driver development, which are critical for flight control systems.

---

## Objectives
- Understand real-time scheduling in embedded flight systems  
- Develop low-level drivers for peripherals used in drones  
- Implement deterministic, time-critical task execution  
- Build a foundation for flight controller software  

---

## Features
- Real-time multitasking using NuttX  
- Deterministic task scheduling  
- GPIO driver for status LEDs and signals  
- UART driver for telemetry and debugging  
- Timer-based execution for periodic control loops  
- Multi-threading with priority control  
- STM32 peripheral interfacing  

---

## Tech Stack

| Component   | Technology |
|------------|-----------|
| RTOS       | Apache NuttX |
| MCU        | STM32 |
| Language   | C |
| Toolchain  | arm-none-eabi-gcc |
| Debugging  | OpenOCD, ST-Link |

---

## Driver Modules

### GPIO Driver
- Controls status LEDs and digital outputs  
- Can be extended for ESC signaling or interrupts  

### UART Driver
- Used for telemetry and debugging  
- Suitable for communication with flight controllers or ground stations  

### Timer Module
- Provides precise periodic execution  
- Foundation for control loops (PID, sensor polling)  

### Multi-threading
- Priority-based scheduling  
- Ensures time-critical tasks run reliably  

---

## Build and Run

### 1. Configure NuttX
```bash
cd nuttx_ws
make menuconfig

Enable:

UART
GPIO
Timers
(Optional) PWM for motor control

2. Build Firmware
make -j$(nproc)

3. Flash Firmware
Using OpenOCD
openocd -f interface/stlink.cfg -f target/stm32f4x.cfg
Using ST-Link
st-flash write nuttx.bin 0x8000000

4. Serial Output
screen /dev/ttyUSB0 115200

Future Work
PWM driver for motor (ESC) control
Sensor drivers (IMU, barometer, GPS)
PID control loop implementation
Integration with flight stack (PX4 / ROS2)
Real-time data logging and telemet


