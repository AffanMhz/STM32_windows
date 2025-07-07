
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
