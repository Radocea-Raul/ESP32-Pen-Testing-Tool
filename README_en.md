# ESP32_Jammer - 2.4GHz ESP32 Jammer Tool

🇷🇴 *[Citește documentația în Română](README.md)*

![Assembled Jammer](poze/proiect_asamblat.jpg)
![PCB](poze/PCB.JPG)

⚠️ **DISCLAIMER:** This project was created STRICTLY for educational purposes and network security testing (penetration testing) in controlled, authorized environments. Using jamming devices in public spaces or against equipment you do not own is illegal in most countries. The author assumes no responsibility for any misuse of this code.

## Description
This project turns an ESP32 into a 2.4GHz radio testing tool. The device controls **three NRF24L01 radio modules simultaneously** to flood channels with a constant signal, potentially jamming Wi-Fi, Bluetooth, or drone signals. 

The device features an OLED screen and a 3-button navigation system that allows you to select the attack type directly from the device. To concentrate all power on the radio modules, the ESP32's native Wi-Fi and Bluetooth interfaces are automatically disabled in the code.

## Main Features (Menu)
* **BT Jammer:** Jams Bluetooth channels (random frequencies/channels up to 80).
* **Drone Jammer:** Launches interference on drone-specific channels (up to 125).
* **WiFi Jammer:** Targets the main Wi-Fi channels (1, 6, and 14).
* **Multi Ch Jam:** Combined/multi-channel attack on 2.4GHz.

## Required Hardware
* Microcontroller **ESP32**
* **3 x NRF24L01 radio modules** (connected via VSPI)
* **3 x 10µF Capacitors** (soldered directly to the power pins of each NRF24L01 module for stability)
* **AMS1117 Voltage Regulator** (to provide the necessary current for the radio modules)
* **47µF Capacitors** (for filtering on the AMS1117 regulator - *note: works perfectly fine with 20µF as well*)
* **128x64 OLED Display** (I2C, `0x3C` address, SSD1306 driver)
* **3 x Push Buttons** (connected to pins 14 [UP], 12 [DOWN], 13 [SELECT])

## Technologies and Libraries
To compile this code, you will need the following libraries installed in the Arduino IDE:
* `RF24` (for NRF24 modules) 
* `Adafruit_GFX` and `Adafruit_SSD1306` (for the OLED screen)
* `U8g2_for_Adafruit_GFX` (for fonts and UI) 

## Installation and Usage
1. Clone this repository.
2. Open the `.ino` file in Arduino IDE.
3. Make sure you have installed all the libraries mentioned above from the *Library Manager*.
4. Select your ESP32 board from the *Tools* menu and the correct port.
5. Click *Upload*.
6. Once powered on, you will see the boot logo ("demonSHIT" / Cypher Box), followed by a welcome message, and then you will enter the main menu. Use the buttons to navigate.

## Wiring Diagram (Pinout)

**Power Supply (Very Important):**
The NRF24L01 modules consume a lot of current. The ESP32's 3.3V pin is not enough for 3 modules simultaneously. 
* Use the `VIN` (5V) pin of the ESP32 to power the **AMS1117** regulator.
* The 3.3V output of the AMS1117 regulator will power the 3 radio modules.
* Place the **47µF** (or 20µF) capacitors on the input/output of the regulator.
* Solder a **10µF** capacitor directly to the VCC/GND pins of each NRF24 module.

**SPI Connections (Common for all 3 NRF24 modules):**
All 3 modules are wired in parallel to the ESP32's VSPI pins:
* **MOSI** -> Pin 23
* **MISO** -> Pin 19
* **SCK** -> Pin 18

**Specific CE / CSN Connections (To control them separately):**
| Module | CE Pin | CSN Pin |
|-------|--------|---------|
| Radio 1 | Pin 27 | Pin 15 |
| Radio 2 | Pin 26 | Pin 25 |
| Radio 3 | Pin 17 | Pin 5 |

**OLED Display & Buttons:**
| Component | ESP32 Pin |
|------------|-----------|
| OLED SDA | Pin 21 (Default I2C) |
| OLED SCL | Pin 22 (Default I2C) |
| UP Button | Pin 14 (Connected to GND) |
| DOWN Button | Pin 12 (Connected to GND) |
| SELECT Button| Pin 13 (Connected to GND) |

⚠️ **Important compatibility note (Compilation Errors):** If you encounter errors when trying to compile the code, the issue is most likely caused by new versions of the ESP32 board package or libraries. 
* It is recommended to **downgrade** the **esp32 by Espressif Systems** board package in the *Boards Manager* (preferably to a stable version from the `2.0.x` series, such as `2.0.14` or `2.0.17`, because `3.x` versions introduce major changes that can break compatibility).
* Additionally, if you are using the latest versions of the libraries (especially `RF24` or `Adafruit_GFX`), they might also require a downgrade to a previous version if errors persist.

## Project Evolution (Behind the Scenes)

Before reaching the final, clean PCB version, the "Cypher Box" started as a breadboard prototype. It was a "spaghetti wiring" mess at first to test the simultaneous SPI communication with the 3 NRF24L01 modules and the AMS1117 power supply, but it was worth the effort!

![Breadboard Prototype 1](poze/poza_breadboard1.jpg)
*(Breadboard testing phase)*

![Breadboard Prototype 2](poze/poza_breadboard2.jpg)
*(First tests with the OLED screen)*

## Original Source
This project was developed starting from the original code created by [Divine Zeal](https://github.com/dkyazzentwatwa). Thanks for the inspiration and the logic behind it!

## Contact
raulradocea@gmail.com
