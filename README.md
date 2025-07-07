# STM32 Blue Pill Bootloader Setup Guide for Windows

## Professional Windows Configuration Manual  
**Complete guide for configuring bootloader on STM32F103C8T6 microcontroller**

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Hardware Configuration](#hardware-configuration)
- [Software Installation](#software-installation)
- [Bootloader Flash Procedure](#bootloader-flash-procedure)
- [Bootloader Verification](#bootloader-verification)
- [Troubleshooting Guide](#troubleshooting-guide)
- [Additional Resources](#additional-resources)

---

## <a name="overview"></a>Overview

The STM32F103C8T6 "Blue Pill" development board requires proper bootloader configuration to enable USB programming and debugging capabilities.  
This guide provides comprehensive instructions for Windows users to configure the bootloader using official STMicroelectronics tools and protocols.

> ⚠️ **Important Notice**  
> This procedure involves flashing firmware to the microcontroller's system memory. Incorrect implementation may render the device inoperable. Ensure all connections are verified before proceeding.

---

## <a name="prerequisites"></a>Prerequisites

### System Requirements
- Windows 10/11 (64-bit recommended)
- Administrator privileges for driver installation
- USB 2.0 or higher port
- Internet connection for software downloads

### Hardware List
- **STM32F103C8T6 Blue Pill**: ARM Cortex-M3 development board  
- **ST-Link V2 Programmer**: Official STMicroelectronics debugging interface  
- **Dupont Jumper Wires**: Female-to-female connections for SWD interface  
- **USB-A to Mini-B Cable**: For ST-Link programmer connectivity  

---

## <a name="hardware-configuration"></a>Hardware Configuration

### Step 1: SWD Interface Connection

Establish Serial Wire Debug (SWD) connection between ST-Link V2 and Blue Pill:

| ST-Link V2 Pin | Blue Pill Pin | Function              |
|----------------|---------------|-----------------------|
| SWDIO          | PA13          | Serial Wire Data I/O  |
| SWCLK          | PA14          | Serial Wire Clock     |
| GND            | GND           | Ground Reference      |
| 3.3V           | 3.3V          | Power Supply          |

---

### Step 2: Boot Configuration

Configure Blue Pill boot mode for **System Memory Boot**:

- **BOOT0** jumper: Set to `1` (3.3V)
- **BOOT1** jumper: Set to `0` (GND)

> ⚠️ **Critical**: Verify jumper positions before applying power. Incorrect boot configuration may prevent bootloader access.

---

## <a name="software-installation"></a>Software Installation

### Step 1: Install STM32CubeProgrammer

- Download from the [official ST website](https://www.st.com/en/development-tools/stm32cubeprog.html)
- Installation Path: `C:\Program Files\STMicroelectronics\STM32Cube\STM32CubeProgrammer`
- Recommended: Latest stable release

### Step 2: Install ST-Link USB Drivers

- Download from [here](https://www.st.com/en/development-tools/st-link-v2.html)
- Extract the package to a temporary folder
- Run the installer with administrator rights
- Follow wizard prompts
- Reboot the system if prompted

### Step 3: Acquire Bootloader Binary

- Get STM32duino bootloader from the [STM32duino GitHub](https://github.com/rogerclarkmelbourne/STM32duino-bootloader)
- Recommended binary: `generic_boot20_pc13.bin` (for Blue Pill with PC13 LED)

---

## <a name="bootloader-flash-procedure"></a>Bootloader Flash Procedure

### Step 1: Verify Device Connection

1. Connect ST-Link V2 to PC via USB  
2. Connect Blue Pill to ST-Link via SWD interface  
3. Launch **STM32CubeProgrammer**  
4. Select "ST-LINK" interface  
5. Click **Connect**

✅ *Success Indicator*: Status shows "Connected" and device info shows STM32F103C8

---

### Step 2: Prepare Flash Memory

1. Go to **Erasing & Programming** tab  
2. Select **Full chip erase**  
3. Click **Start Programming**  
4. Wait for confirmation

**Expected Output:**
'''Memory erased successfully
Flash memory cleared: 0x08000000 to 0x0800FFFF'''


---

### Step 3: Flash Bootloader Binary

1. Click **Browse** and select `generic_boot20_pc13.bin`  
2. Set start address: `0x08000000`  
3. Click **Start Programming**  
4. Monitor progress

✅ *Programming Complete*: “File download completed successfully”

---

### Step 4: Reset Boot Configuration

1. Power down Blue Pill  
2. Set **BOOT0** jumper to `0` (GND)  
3. Keep **BOOT1** at `0` (GND)  
4. Disconnect ST-Link  
5. Power Blue Pill via USB

---

## <a name="bootloader-verification"></a>Bootloader Verification

### Step 1: USB Device Recognition

- Connect Blue Pill to PC via USB  
- Open **Device Manager**  
- Check under **Universal Serial Bus Devices**

✅ *Success*: Device shows as `STM32 BOOTLOADER` or `Maple DFU` with no errors

---

### Step 2: DFU Mode Verification

Run the command:
```bash
dfu-util -l
Expected Output:
'''Found DFU: [1eaf:0003] ver=0201, devnum=XX, cfg=1, intf=0, path="X-X", alt=2,
name="STM32duino bootloader v1.0  Upload to Flash 0x8002000", serial="LLM 003"
'''
