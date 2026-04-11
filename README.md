# Yugaja Radio Update Tool

Windows desktop utility for updating firmware on Yugaja WiFi Streamer + BT Speaker devices (ESP32 / ESP32-S3).

## Supported boards

- `jc2432w328_noPSRAM` (default)
- `jc4827w543` (ESP32-S3)
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

ESP32 boards use:

- `bootloader.bin`
- `partitions.bin`
- `ota_data_initial.bin`
- `app_wifi.bin`
- `app_bt.bin`
- `spiffs.bin`

`jc4827w543` (ESP32-S3) uses:

- `bootloader.bin`
- `partitions.bin`
- `firmware.bin`
- `spiffs.bin`

Legacy/fallback file names are no longer used.

## Release package layout

`bin/release` contains the updater app, `esptool.exe`, and board firmware folders:

- `yugaja_radio_update_tool.exe`
- `esptool.exe`

Firmware files are organized by board folders under `bin/release`:

- `jc2432w328_noPSRAM`
- `jc4827w543`
- `esp32-2432S028R_PSRAM`
- `esp32-2432S028R_noPSRAM`
- `esp32-2432S028R-V3`
- `esp32-2432S028R-V3_noPSRAM`

Each board folder contains the required file names for that board (see **Firmware files** above).

## Flash addresses

ESP32 boards:

- `bootloader.bin` -> `0x1000`
- `partitions.bin` -> `0x8000`
- `ota_data_initial.bin` -> `0xE000`
- `app_wifi.bin` -> `0x10000`
- `app_bt.bin` -> `0x1F0000`
- `spiffs.bin` -> `0x3D0000`

`jc4827w543` (ESP32-S3):

- `bootloader.bin` -> `0x0`
- `partitions.bin` -> `0x8000`
- `firmware.bin` -> `0x10000`
- `spiffs.bin` -> `0x2B0000`

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

`jc4827w543` uses the built-in I2S class-D amplifier. Default I2S pins:

```c
// CYD I2S wiring (GPIO42 LRCK, GPIO41 DOUT, GPIO42 BCLK depending on module).
// Kept aligned with the original bring-up branch.
#define I2S_DOUT                  41
#define I2S_BCLK                  42
#define I2S_LRC                   2
```

## Important

- Do not disconnect USB or power during flashing.
- Select the correct board type before flashing.
- Keep firmware files in their board-specific folders.
