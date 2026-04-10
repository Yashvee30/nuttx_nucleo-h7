# NuttX RTOS on STM32

![RTOS](https://img.shields.io/badge/RTOS-Apache%20NuttX-blue)
![MCU](https://img.shields.io/badge/MCU-STM32-green)
![Language](https://img.shields.io/badge/Language-C-orange)
![Build](https://img.shields.io/badge/Build-Passing-brightgreen)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## Overview
This project demonstrates running Apache NuttX RTOS on an STM32 microcontroller. It showcases real-time multitasking, hardware abstraction, and peripheral interaction in an embedded system.

Designed as a hands-on learning project for understanding:
- Task scheduling  
- Driver interaction  
- Real-time system behavior  

---

## Architecture


+---------------------------+
| User Tasks |
| (GPIO / UART / Timer) |
+------------+--------------+
|
v
+---------------------------+
| NuttX Scheduler |
| (Priority-based RTOS) |
+------------+--------------+
|
v
+---------------------------+
| Device Drivers |
| (GPIO, UART, Timers) |
+------------+--------------+
|
v
+---------------------------+
| STM32 Hardware |
+---------------------------+


---

## Features
- Real-time multitasking using NuttX  
- GPIO control for LEDs and external devices  
- UART communication for debugging  
- Timer-based task execution  
- Multi-threading with priority scheduling  
- STM32 peripheral integration  

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

## Project Structure


.
├── nuttx/ # NuttX source
├── apps/ # Application layer
│ ├── gpio_task.c
│ ├── uart_task.c
│ ├── timer_task.c
│ └── main.c
├── configs/ # Board configuration
├── scripts/ # Flashing scripts
└── README.md


---

## Modules

### GPIO Task
- Controls LED blinking  
- Demonstrates digital output  

### UART Task
- Sends debug logs  
- Enables serial communication  

### Timer Task
- Executes periodic operations  
- Uses RTOS timers  

### Multi-threading
- Priority-based scheduling  
- Concurrent task execution  

---

## Build and Run

### 1. Configure NuttX
```bash
cd nuttx
make menuconfig

Enable:

UART
GPIO
Timers
2. Build Firmware
make -j$(nproc)
3. Flash Firmware
Using OpenOCD
openocd -f interface/stlink.cfg -f target/stm32f4x.cfg
Using ST-Link
st-flash write nuttx.bin 0x8000000
4. Serial Output
screen /dev/ttyUSB0 115200
Demo (Optional)
NuttX Booting...
GPIO Task Running
UART Initialized
Timer Tick: 1000ms
Future Work
Add motor control using PID
Integrate sensors like IMU or encoders
Build ROS2 communication interface
Expand into robotics applications
