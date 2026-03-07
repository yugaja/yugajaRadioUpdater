# Yugaja Radio Update Tool

Windows desktop utility for updating firmware on Yugaja WiFi Streamer + BT Speaker devices (ESP32). Supports both JC2432W328 and esp32-2432S028R boards with automatic firmware variant selection.

## What the app does

- Detects available COM ports.
- **Selects board type** (JC2432W328 or esp32-2432S028R) to use correct firmware variants.
- Lets you select which firmware parts to flash.
- Runs `esptool.exe` and writes selected `.bin` files to the correct ESP32 addresses.
- Shows log output and progress during flashing.
- Checks partition range overlap and blocks conflicting selections.

## Firmware flow (short)

1. Start the tool.
2. **Select the board type:** JC2432W328 or esp32-2432S028R.
3. Select the device COM port.
4. Select the firmware parts to flash (or keep all checked).
5. Click `Start Flash`.
6. Wait for status: `Flashing completed successfully`.

### Supported flash steps and addresses:

All boards use the same memory addresses:

- `bootloader.bin` -> `0x1000`
- `partitions.bin` -> `0x8000`
- `ota_data_initial.bin` -> `0xE000`
- WiFi firmware -> `0x10000`
- BT firmware -> `0x1F0000`
- `spiffs.bin` -> `0x3D0000`

### Board-specific firmware files:

**JC2432W328 board:**
- `app_wifi.bin` (fallback: `firmware_app.bin`) - WiFi firmware
- `app_bt.bin` (fallback: `firmware_boot.bin`) - BT firmware

**esp32-2432S028R board:**
- `app_wifi_res.bin` (fallback: `app_wifi.bin`) - WiFi firmware (optimized)
- `app_bt_res.bin` (fallback: `app_bt.bin`) - BT firmware (optimized)

## File location

The app expects `esptool.exe` and firmware `.bin` files in the same folder as `yugaja_radio_update_tool.exe` (typically `bin/Release`).

## Download and prepare files

You must download all release files, not just a single `.bin`.

### For JC2432W328 board:

Before starting the updater, make sure all of these files are in one folder:

- `yugaja_radio_update_tool.exe`
- `esptool.exe`
- `bootloader.bin`
- `partitions.bin`
- `ota_data_initial.bin`
- `app_wifi.bin` (or fallback: `firmware_app.bin`)
- `app_bt.bin` (or fallback: `firmware_boot.bin`)
- `spiffs.bin`

### For esp32-2432S028R board:

Before starting the updater, make sure all of these files are in one folder:

- `yugaja_radio_update_tool.exe`
- `esptool.exe`
- `bootloader.bin`
- `partitions.bin`
- `ota_data_initial.bin`
- `app_wifi_res.bin` (or fallback: `app_wifi.bin`)
- `app_bt_res.bin` (or fallback: `app_bt.bin`)
- `spiffs.bin`

**Note:** If files are split across different folders, flashing will fail because the updater cannot find required binaries.

## Hardware notes

### JC2432W328 board (audio pins):

This device variant uses the following I2S/DAC pins:

```c
#define I2S_DOUT 17
#define I2S_BCLK 22
#define I2S_LRC 4
```

### esp32-2432S028R board:

Refer to the device documentation for pin configuration.

## Important

- **Do not disconnect USB or power during flashing.**
- Select the correct board type before flashing to ensure correct firmware files are used.
- The application supports fallback filenames for compatibility with legacy firmware releases.
