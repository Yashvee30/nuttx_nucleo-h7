- Real-time multitasking with NuttX RTOS  
- GPIO control for LEDs, buttons, and external devices  
- UART serial communication  
- Timer-based periodic tasks  
- Multi-threading with priority scheduling  
- STM32 hardware driver integration  
- Embedded system debugging support  

---

## Tech Stack
- RTOS: Apache NuttX  
- MCU: STM32 (F1 or F4 series depending on board)  
- Language: C  
- Toolchain: arm-none-eabi-gcc  
- Debugger: OpenOCD and ST-Link  
- Build System: NuttX Kconfig and Make  

---

## System Architecture


+----------------------+
| Application Layer |
| User Threads |
+----------------------+
|
+----------------------+
| NuttX RTOS Kernel |
| Scheduler and APIs |
+----------------------+
|
+----------------------+
| Board Support Package|
| GPIO UART TIMERS |
+----------------------+
|
+----------------------+
| STM32 Hardware |
+----------------------+


---

## Modules

### GPIO Control Task
- Controls onboard LED
- Demonstrates digital output handling

### UART Task
- Serial communication with host PC
- Debug logging and data exchange

### Timer Task
- Executes periodic tasks using RTOS timers

### Multi-threading Demo
- Tests scheduling and task priorities
- Demonstrates concurrent execution

---

## Build Instructions

### Step 1: Configure NuttX
```bash
cd nuttx
make menuconfig

Select target STM32 board and enable required drivers such as UART, GPIO, and timers.

Step 2: Build Firmware
make -j$(nproc)
Step 3: Flash Firmware

Using OpenOCD

openocd -f interface/stlink.cfg -f target/stm32f4x.cfg

Or using st-flash

st-flash write nuttx.bin 0x8000000
Step 4: Serial Monitor
screen /dev/ttyUSB0 115200
