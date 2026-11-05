# MeshCore Custom Firmware for ESP32 Classic (WROOM) and ESP32-C3 SuperMini

Custom MeshCore builds for SX1276/SX1278 LoRa modules, with optional SSD1306 OLED, BME280 and GPS support.

## Contents

1. [Prerequisites](#1-prerequisites)
2. [Download the firmware package](#2-download-the-firmware-package)
3. [Open the project in VS Code](#3-open-the-project-in-vs-code)
4. [Configure pin numbers](#4-configure-pin-numbers)
5. [Physical wiring](#5-physical-wiring)
6. [Build and upload](#6-build-and-upload)
7. [Credits](#credits)

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

Open the unzipped folder in VS Code Explorer:

<img width="419" height="374" alt="Unzipped folder" src="https://github.com/user-attachments/assets/cbe5f376-f8f3-475b-87d0-c4d3ef978faf" />

## 3. Open the project in VS Code

Open the folder in VS Code, then open a new terminal from the PlatformIO extension (the alien-head icon on the left sidebar):

<img width="860" height="584" alt="PlatformIO terminal" src="https://github.com/user-attachments/assets/f5c86c0a-8c67-4701-b0a6-5803dc30b66c" />

Make sure the terminal prompt shows your project root folder, e.g. `C:\Meshcore>` or `D:\Meshcore>`, matching where you unzipped it. In the left hand excplorer menu click on variants and then select the folder my_esp32_sx1278_companion then click on platformio.ini

<img width="892" height="877" alt="Terminal at project root" src="https://github.com/user-attachments/assets/fd039a54-ce69-4879-8093-569288445c93" />

## 4. Configure pin numbers

Connect your ESP32 to the PC, then adjust the pin numbers in the build flags as shown in the screenshot above. There are two ready-made pin profiles. Use **one** of them and wire your hardware to match. **Press **Ctrl+S** to save if you make any changes**

> **Tip:** If you change any pins, verify your custom pin map (for example with ChatGPT or the ESP32 pinout documentation) so that you do **not** use bootstrapping/strapping pins.

> **Warning:** The two profiles use different pins (for example GPIO5 is `SCLK` in the AP2AJM profile but `NSS` in the simple profile). Never mix wiring from one profile with flags from the other.

### Profile A: Simple companion (no screen, no GPS)

```ini
-D RADIO_CLASS=CustomSX1276
-D WRAPPER_CLASS=CustomSX1276Wrapper
-D P_LORA_SCLK=18
-D P_LORA_MISO=19
-D P_LORA_MOSI=23
-D P_LORA_NSS=5
-D P_LORA_RESET=14
-D P_LORA_DIO_0=26
-D P_LORA_DIO_1=32
-D SX127X_CURRENT_LIMIT=120
-D LORA_TX_POWER=17
-D LORA_FREQ=433.175
```

### Profile B: AP2AJM (OLED display + BME280 + NEO-6M GPS)

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

; BME280 environmental sensor
-D ENV_INCLUDE_BME280=1
-D TELEM_BME280_ADDRESS=0x76

; NEO-6M GPS
-D ENV_INCLUDE_GPS=1
-D PIN_GPS_TX=16
-D PIN_GPS_RX=17
-D GPS_BAUD_RATE=9600
-D ENV_SKIP_GPS_DETECT=1
```

## 5. Physical wiring

The wiring tables below are for the **ESP32 Classic (WROOM)** and match the pin profiles above exactly.

**General notes**

- Fit the antenna to the SX1278 module **before** powering it on. Transmitting without an antenna can damage the radio.
- The SX1278 (Ra-02) is a **3.3V-only** device. Never connect it to 5V.
- Connect all grounds together (ESP32, LoRa module, OLED, BME280, GPS, battery).

### 5.1 Profile A: Simple companion wiring

SX1278 LoRa module to ESP32:

| SX1278 pin | ESP32 pin |
|------------|-----------|
| 3.3V       | 3V3       |
| GND        | GND       |
| SCK        | GPIO18    |
| MISO       | GPIO19    |
| MOSI       | GPIO23    |
| NSS (CS)   | GPIO5     |
| RESET      | GPIO14    |
| DIO0       | GPIO26    |
| DIO1       | GPIO32    |

### 5.2 Profile B: AP2AJM wiring

**SX1278 LoRa module to ESP32:**

| SX1278 pin | ESP32 pin |
|------------|-----------|
| 3.3V       | 3V3       |
| GND        | GND       |
| SCK        | GPIO5     |
| MISO       | GPIO19    |
| MOSI       | GPIO27    |
| NSS (CS)   | GPIO18    |
| RESET      | GPIO23    |
| DIO0       | GPIO26    |
| DIO1       | GPIO32    |

**Shared I2C bus (SSD1306 OLED and BME280 both connect to the same two pins):**

| Device pin (OLED and BME280) | ESP32 pin |
|------------------------------|-----------|
| VCC                          | 3V3       |
| GND                          | GND       |
| SDA                          | GPIO21    |
| SCL                          | GPIO22    |

- OLED I2C address: `0x3C`
- BME280 I2C address: `0x76`

**NEO-6M GPS module to ESP32:**

| ESP32 pin (build flag)  | GPS module pin |
|-------------------------|----------------|
| GPIO16 (`PIN_GPS_TX`)   | RX             |
| GPIO17 (`PIN_GPS_RX`)   | TX             |
| 3V3                     | VCC            |
| GND                     | GND            |

### 5.3 Battery voltage monitor (both profiles)

Both builds read battery voltage on **GPIO34** through a 100k / 100k voltage divider across the battery.

```text
Battery (+) ---[ 100k ]---+---[ 100k ]--- Battery (-) / GND
                          |
                       GPIO34
```

| Connection                    | To                |
|-------------------------------|-------------------|
| Battery (+) via 100k resistor | GPIO34            |
| GPIO34 via 100k resistor      | Battery (-) / GND |

## 6. Build and upload

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
