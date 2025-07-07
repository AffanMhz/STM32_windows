
# STM32 Blue Pill Bootloader Setup Guide for Windows
**Professional Windows Configuration Manual**  
Complete guide for configuring bootloader on STM32F103C8T6 microcontroller

Visit Site: https://affanmhz.github.io/STM32_windows/

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Hardware Setup](#hardware-setup)
- [Software Installation](#software-installation)
- [Bootloader Flash Procedure](#bootloader-flash-procedure)
- [Bootloader Verification](#bootloader-verification)
- [Troubleshooting Guide](#troubleshooting-guide)
- [Additional Resources](#additional-resources)

---

<a id="overview"></a>
## 1. Overview
The STM32F103C8T6 "Blue Pill" development board requires proper bootloader configuration to enable USB programming and debugging capabilities. This guide provides comprehensive instructions for Windows users to configure the bootloader using official STMicroelectronics tools and protocols.

> **Important Notice**  
> This procedure involves flashing firmware to the microcontroller's system memory. Incorrect implementation may render the device inoperable. Ensure all connections are verified before proceeding.

---

<a id="prerequisites"></a>
## 2. Prerequisites

### System Requirements
- Windows 10/11 (64-bit recommended)
- Administrator privileges for driver installation
- USB 2.0 or higher port
- Internet connection for software downloads

### Hardware List
- STM32F103C8T6 Blue Pill: Primary development board
- ST-Link V2 Programmer: Official debugging interface
- Dupont Jumper Wires (Female-to-female)
- USB-A to Mini-B Cable: For ST-Link connectivity

---

<a id="hardware-setup"></a>
## 3. Hardware Setup

### Step 1: SWD Interface Connection
Establish Serial Wire Debug (SWD) connection between ST-Link V2 and Blue Pill:

| ST-Link V2 Pin | Blue Pill Pin | Function            |
|----------------|---------------|---------------------|
| SWDIO          | PA13          | Serial Wire Data I/O|
| SWCLK          | PA14          | Serial Wire Clock   |
| GND            | GND           | Ground Reference    |
| 3.3V           | 3.3V          | Power Supply        |

### Step 2: Boot Configuration
Configure Blue Pill boot mode:
- BOOT0 jumper: Position to "1" (3.3V)
- BOOT1 jumper: Position to "0" (GND)

> **Critical**  
> Verify jumper positions before applying power. Incorrect boot configuration may prevent bootloader access.

---

<a id="software-installation"></a>
## 4. Software Installation

### Step 1: STM32CubeProgrammer
1. Download and install the official programming utility:
   ```
   Download: https://www.st.com/en/development-tools/stm32cubeprog.html
   ```
2. Default installation path:
   ```
   C:\Program Files\STMicroelectronics\STM32Cube\STM32CubeProgrammer
   ```
3. Use latest stable release

### Step 2: ST-Link Driver Installation
1. Download official drivers:
   ```
   https://www.st.com/en/development-tools/stsw-link009.html
   ```
2. Extract package to temporary directory
3. Run installer as administrator
4. Restart system if prompted

### Step 3: Bootloader Binary Acquisition
- Recommended firmware:
  ```
  generic_boot20_pc13.bin (For Blue Pill with PC13 LED)
  ```
- Download from STM32duino project repository

---

<a id="bootloader-flash-procedure"></a>
## 5. Bootloader Flash Procedure

### Step 1: Connection Verification
1. Connect hardware as described in [Hardware Setup](#hardware-setup)
2. Launch STM32CubeProgrammer
3. Select "ST-LINK" interface
4. Click "Connect"
   - **Success Indicator**: "Connected" status with device information

### Step 2: Memory Preparation
1. Navigate to "Erasing & Programming" tab
2. Select "Full chip erase" option
3. Click "Start Programming"
   - **Expected Output**:
     ```
     Memory erased successfully
     Flash memory cleared: 0x08000000 to 0x0800FFFF
     ```

### Step 3: Flash Programming
1. Browse and select bootloader binary file
2. Set start address:
   ```
   0x08000000
   ```
3. Click "Start Programming"
   - **Success Message**:
     ```
     File download completed successfully
     ```

### Step 4: Boot Configuration Reset
1. Power down Blue Pill
2. Configure jumpers:
   - BOOT0 → "0" (GND)
   - BOOT1 → "0" (GND)
3. Disconnect ST-Link
4. Power via USB

---

<a id="bootloader-verification"></a>
## 6. Bootloader Verification

### Step 1: USB Recognition
1. Connect Blue Pill to PC via USB
2. Open Device Manager
3. Verify under "Universal Serial Bus devices":
   - `STM32 BOOTLOADER` or `Maple DFU` present

### Step 2: DFU Mode Test
1. Open command prompt
2. Execute:
   ```bash
   dfu-util -l
   ```
3. Verify output contains:
   ```bash
   Found DFU: [1eaf:0003] ver=0201... name="STM32duino bootloader..."
   ```

### Step 3: LED Indicator Test
- PC13 LED behavior:
  - Rapid blinking during boot
  - Stabilizes after 8 seconds
  - Constant on during programming

---

<a id="troubleshooting-guide"></a>
## 7. Troubleshooting Guide

| Problem                          | Solution                                  |
|----------------------------------|-------------------------------------------|
| ST-Link can't detect device      | Verify SWD connections and 3.3V power     |
| Flash programming failures       | Perform full chip erase before retrying   |
| Windows doesn't recognize device | Install USB drivers, try different port   |
| Unresponsive device              | Re-flash bootloader via SWD recovery      |

> **Recovery Tip**  
> Always recover through ST-Link SWD interface using the original procedure.

---

<a id="additional-resources"></a>
## 8. Additional Resources
- [STM32F103C8T6 Reference Manual](https://www.st.com/resource/en/reference_manual/cd00171190.pdf)
- [STM32CubeProgrammer User Manual](https://www.st.com/resource/en/user_manual/dm00347944.pdf)
- [ST-Link V2 Documentation](https://www.st.com/resource/en/user_manual/dm00026748.pdf)
- [STM32duino Project Documentation](https://github.com/rogerclarkmelbourne/STM32duino-bootloader)

> **Technical Support**  
> For assistance, consult STMicroelectronics official forums with detailed error reports.
