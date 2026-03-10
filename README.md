# Yugaja Radio Update Tool

Windows desktop utility for updating firmware on Yugaja WiFi Streamer + BT Speaker devices (ESP32). Supports JC2432W328, esp32-2432S028R, and esp32-2432S028R v3 boards with automatic firmware variant selection.

## What the app does

- Detects available COM ports.
- Selects board type to use correct firmware variants.
- Lets you select which firmware parts to flash.
- Runs `esptool.exe` and writes selected `.bin` files to the correct ESP32 addresses.
- Shows log output and progress during flashing.
- Checks partition range overlap and blocks conflicting selections.

## Firmware flow (short)

1. Start the tool.
2. Select the board type.
3. Select the device COM port.
4. Select the firmware parts to flash (or keep all checked).
5. Click `Start Flash`.
6. Wait for status: `Flashing completed successfully`.

### Supported flash steps and addresses

All boards use the same memory addresses:

- `bootloader.bin` -> `0x1000`
- `partitions.bin` -> `0x8000`
- `ota_data_initial.bin` -> `0xE000`
- WiFi firmware -> `0x10000`
- BT firmware -> `0x1F0000`
- `spiffs.bin` -> `0x3D0000`

### Board-specific firmware files

- `JC2432W328`: `app_wifi.bin` / `app_bt.bin` (fallbacks: `firmware_app.bin` / `firmware_boot.bin`)
- `esp32-2432S028R`: `app_wifi_res.bin` / `app_bt_res.bin` (fallbacks: `app_wifi.bin` / `app_bt.bin`)
- `esp32-2432S028R v3`: `app_wifi_v3.bin` / `app_bt_v3.bin` (fallbacks: `app_wifi_res_v3.bin` / `app_bt_res_v3.bin` / `app_wifi.bin` / `app_bt.bin`)

## File location

The app expects `esptool.exe` and firmware `.bin` files in the same folder as `yugaja_radio_update_tool.exe` (typically `bin/release` in this package).

## Download and prepare files

You must download all release files, not just a single `.bin`.

Before starting the updater, make sure all of these files are in one folder:

- `yugaja_radio_update_tool.exe`
- `esptool.exe`
- `bootloader.bin`
- `partitions.bin`
- `ota_data_initial.bin`
- board app files (according to selected board type above)
- `spiffs.bin`

If files are split across different folders, flashing will fail because the updater cannot find required binaries.

## Important

- Do not disconnect USB or power during flashing.
- Select the correct board type before flashing to ensure correct firmware files are used.
- The application supports fallback filenames for compatibility with legacy firmware releases.
