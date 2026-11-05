# MeshCore Custom Firmware for ESP32 Classic (WROOM) and ESP32-C3 SuperMini

Custom MeshCore builds for SX1276/SX1278 LoRa modules, with optional SSD1306 OLED and GPS support.

## Contents

1. [Prerequisites](#1-prerequisites)
2. [Download the firmware package](#2-download-the-firmware-package)
3. [Open the project in VS Code](#3-open-the-project-in-vs-code)
4. [Configure pin numbers](#4-configure-pin-numbers)
5. [Build and upload](#5-build-and-upload)
6. [Credits](#credits)

---

## 1. Prerequisites

1. Download and install [Visual Studio Code](https://code.visualstudio.com/) on Windows.
2. Install these VS Code extensions:
   - **PlatformIO IDE**
   - **C/C++** (Microsoft)

<img width="684" height="429" alt="VS Code extensions" src="https://github.com/user-attachments/assets/bb08f419-4f36-4a85-99cc-a205d50bad69" />

## 2. Download the firmware package

Download **one** of the following zip files:

- `MeshCore_SX1278_variants_clean.zip`
- `Meshcore-ajm.zip`

Unzip it to your C: or D: drive (whichever suits your PC), e.g. `C:\Meshcore` or `D:\Meshcore`.

Open the unzipped folder in Windows Explorer:

<img width="419" height="374" alt="Unzipped folder" src="https://github.com/user-attachments/assets/cbe5f376-f8f3-475b-87d0-c4d3ef978faf" />

## 3. Open the project in VS Code

Open the folder in VS Code, then open a new terminal from the PlatformIO extension (the alien-head icon on the left sidebar):

<img width="860" height="584" alt="PlatformIO terminal" src="https://github.com/user-attachments/assets/f5c86c0a-8c67-4701-b0a6-5803dc30b66c" />

Make sure the terminal prompt shows your project root folder, e.g. `C:\Meshcore>` or `D:\Meshcore>`, matching where you unzipped it:

<img width="892" height="877" alt="Terminal at project root" src="https://github.com/user-attachments/assets/fd039a54-ce69-4879-8093-569288445c93" />

## 4. Configure pin numbers

Connect your ESP32 to the PC, then adjust the pin numbers in the build flags as shown in the screenshot above. You can keep the defaults as in the screenshots for a minimal system or change them to suit your wiring.

> **Tip:** If you change any pins, verify your custom pin map (for example with ChatGPT or the ESP32 pinout documentation) so that you do **not** use bootstrapping/strapping pins.

The pins below are an alternate set for use with GPS and display modules (courtesy of AP2AJM):

```ini
; SX1276 / SX1278 LoRa radio
-D RADIO_CLASS=CustomSX1276
-D WRAPPER_CLASS=CustomSX1276Wrapper
-D P_LORA_DIO_0=26
-D P_LORA_DIO_1=32
-D P_LORA_NSS=18
-D P_LORA_RESET=23
-D P_LORA_SCLK=5
-D P_LORA_MISO=19
-D P_LORA_MOSI=27
-D GPS_NMEA_DEBUG=1

; SSD1306 OLED
-D DISPLAY_CLASS=SSD1306Display
-D DISPLAY_ADDRESS=0x3C
-D PIN_OLED_RESET=-1

; Shared I2C bus
-D PIN_BOARD_SDA=21
-D PIN_BOARD_SCL=22
```

## 5. Build and upload

Run the command that matches your board from the project root.

**ESP32 Classic (WROOM):**

```bash
pio run -e my_esp32_sx1278_companion_ble -t upload
```

**ESP32-C3 SuperMini:**

```bash
pio run -e my_esp32c3_sx1278_companion_ble -t upload
```

## Credits

- Alternate GPS/display pin mapping: AP2AJM
