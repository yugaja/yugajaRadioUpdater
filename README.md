# Yugaja Radio Update Tool

Windows desktop utility for updating firmware on Yugaja WiFi Streamer + BT Speaker devices (ESP32).

## Supported boards

- `jc2432w328_noPSRAM` (default)
- `esp32-2432S028R_PSRAM`
- `esp32-2432S028R_noPSRAM`
- `esp32-2432S028R-V3`
- `esp32-2432S028R-V3_noPSRAM`

## Firmware update flow

1. Start the updater.
2. Select board type.
3. Select COM port.
4. Keep `Full chip erase first` enabled (recommended).
5. Select firmware parts to flash.
6. Click `Start Flash`.

## Firmware files

Only these file names are supported in this release package:

- `bootloader.bin`
- `partitions.bin`
- `ota_data_initial.bin`
- `app_wifi.bin`
- `app_bt.bin`
- `spiffs.bin`

Legacy/fallback file names are no longer used.

## Release package layout

`esptool.exe` is in `bin/release`.

Firmware files are organized by board folders under `bin/release`:

- `jc2432w328_noPSRAM`
- `esp32-2432S028R_PSRAM`
- `esp32-2432S028R_noPSRAM`
- `esp32-2432S028R-V3`
- `esp32-2432S028R-V3_noPSRAM`

Each board folder contains the same required file names listed above.

## Flash addresses

All board variants use the same flash addresses:

- `bootloader.bin` -> `0x1000`
- `partitions.bin` -> `0x8000`
- `ota_data_initial.bin` -> `0xE000`
- `app_wifi.bin` -> `0x10000`
- `app_bt.bin` -> `0x1F0000`
- `spiffs.bin` -> `0x3D0000`

## Hardware note (important)

`jc2432w328_noPSRAM` keeps the old I2S pinout (unchanged):

```c
#define I2S_DOUT 17
#define I2S_BCLK 22
#define I2S_LRC  4
```

For ESP32 variants, due to PSRAM support changes, I2S `BCLK` was changed:

- `esp32-2432S028R_PSRAM`
- `esp32-2432S028R_noPSRAM`
- `esp32-2432S028R-V3`
- `esp32-2432S028R-V3_noPSRAM`

Pin change for ESP32 variants:

- old: `I2S_BCLK = GPIO17`
- new: `I2S_BCLK = GPIO26`

Current ESP32 audio pinout:

```c
#define I2S_DOUT 22
#define I2S_BCLK 26
#define I2S_LRC  27
```

## Important

- Do not disconnect USB or power during flashing.
- Select the correct board type before flashing.
- Keep firmware files in their board-specific folders.
