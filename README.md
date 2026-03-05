# Yugaja Radio Update Tool

Windows desktop utility for updating firmware on the Yugaja WiFi Streamer + BT Speaker device (ESP32), based on the JC2432W328 CYD board (capacitive: ST7789 + CST820).

## What the app does

- Detects available COM ports.
- Lets you select which firmware parts to flash.
- Runs `esptool.exe` and writes selected `.bin` files to the correct ESP32 addresses.
- Shows log output and progress during flashing.
- Checks partition range overlap and blocks conflicting selections.

## Firmware flow (short)

1. Start the tool.
2. Select the device COM port.
3. Select the firmware parts to flash (or keep all checked).
4. Click `Start Flash`.
5. Wait for status: `Flashing completed successfully`.

Supported flash steps and addresses:

- `bootloader.bin` -> `0x1000`
- `partitions.bin` -> `0x8000`
- `ota_data_initial.bin` -> `0xE000`
- `app_wifi.bin` (fallback: `firmware_app.bin`) -> `0x10000`
- `app_bt.bin` (fallback: `firmware_boot.bin`) -> `0x1F0000`
- `spiffs.bin` -> `0x3D0000`

## File location

The app expects `esptool.exe` and firmware `.bin` files in the same folder as `yugaja_radio_update_tool.exe` (typically `bin/Release`).

## Download and prepare files

You must download all release files, not just a single `.bin`.

Before starting the updater, make sure all of these files are in one folder:

- `yugaja_radio_update_tool.exe`
- `yugaja_radio_update_tool.exe.config`
- `esptool.exe`
- `bootloader.bin`
- `partitions.bin`
- `ota_data_initial.bin`
- `app_wifi.bin` (or `firmware_app.bin`)
- `app_bt.bin` (or `firmware_boot.bin`)
- `spiffs.bin`

If files are split across different folders, flashing will fail because the updater cannot find required binaries.

## Hardware note (audio pins)

This device variant uses the following I2S/DAC pins:

```c
#define I2S_DOUT 17
#define I2S_BCLK 22
#define I2S_LRC 4
```

## Important

Do not disconnect USB or power during flashing.
