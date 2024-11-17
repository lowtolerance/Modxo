![Modchip + Axolotl = Modxo](images/logo.png) <img src="images/Shalx-TR.png" height="176">

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://github.com/Team-Resurgent/Repackinator/blob/main/LICENSE.md)
[![.NET](https://github.com/Team-Resurgent/Modxo/actions/workflows/BundleModxo.yml/badge.svg)](https://github.com/Team-Resurgent/Modxo/actions/workflows/BundleModxo.yml)
[![Discord](https://img.shields.io/badge/chat-on%20discord-7289da.svg?logo=discord)](https://discord.gg/VcdSfajQGK)

#Modxo

Modxo (pronnounced "Modsho") is an RP2040 firmware that converts a Raspberry Pi Pico (or similar RP2040-based device) into an Original Xbox-compatible LPC peripheral device. 

Modxo can be used for loading an Xbox BIOS image from the LPC port, as well as for interfacing comaptible Xbox software with peripheral devices such as HD44480 displays or addressable RGB LEDs.

Modxo is *not* a modchip. Whereas legacy modchips rely on largely obsolete hardware like LPC flash storage chips or expensive programmable logic ICs, Modxo is the first fully software-based implementation of the LPC protocol. It is open source software, mostly written in C, developed using the official Raspberry Pi Pico SDK and designed to run on RP2040-based hardware. 

No specialized hardware or complicated tooling is needed to load Modxo on a compatible device -- in most cased just a USB cable is all that is necessary. And installation works much like legacy devices -- all that is needed for installation is a compatible RP2040-based device, a few resistors, wire and basic soldering equipment. Custom PCBs exist to simplify the installation process even further.

# How to Install
### 1. Requirements
- An Xbox (any revision) with a working LPC Port. 1.6 Xboxes will need an LPC rebuild.
- A RP2040 development board. There may be some clone boards that are not compatible. The following boards are known to work with Modxo:
- - Official Raspberry Pi Pico or RP2040 Zero (There are some clone boards that are not compatible)
  - YD-RP2040
  - RP2040 Zero
  - RP-Tiny
- 4 100 Ohm resistors (tested with 1/4 W resistors)

### 1. Requirements
* An Xbox (any revision) with a working LPC Port. 1.6 Xboxes will need an LPC rebuild.  
* A RP2040 development board. There may be some clone boards that are not compatible. The following boards are known to work with modxo:
- * Official Raspberry Pi Pico
- * YD-RP2040 
- * RP2040 Zero
- * RP-Tiny 
- 
* 4 100 Ohm resistors (tested with 1/4 W resistors) 
 
### 2. Wiring diagrams

#### LPC Header
![LPC Header wiring diagram](images/lpc_header_wiring.png)

* Note: D0 is required for versions 1.0 - 1.5 unless it is grounded.
* Note: LFrame and LPC 3.3V connections are required by version 1.6 or when connecting the Pico to USB port.
* Note: LFrame is not required for USB debugging.
* Note: An LPC Rebuild is required for version 1.6

#### Official Pico

![LPC Header wiring diagram](images/official_pinout.png)

* Note: Please add the diode if connecting the Pico to USB. This avoids powering the LPC 5V Pin from the USB cable.

#### YD2040

![LPC Header wiring diagram](images/YDRP2040_pinout.png)

* Note: Don’t forget to add solder to jumper R68 if using the onboard RGB LED.

#### RP2040 Zero

![LPC Header wiring diagram](images/RP2040_Zero_pinout.png)

* Note: USB Port doesn't have a diode, so it is not recommended to connect usb cable while connected to the XBOX

### 3. Flashing firmware

#### Packing Bios
1. Go to [https://team-resurgent.github.io/modxo/](https://team-resurgent.github.io/Modxo/)
2. Drag and Drop your bios file
3. UF2 File with bios image will be downloaded

#### Flashing steps
1. Connect Raspberry Pi Pico with BOOTSEL button pressed to a PC and one new drive will appear.
2. Copy Modxo.uf2 into the Raspberry Pi Pico Drive.
3. Reconnect Raspberry Pi Pico with BOOTSEL button pressed, so the previous drive will showup again.
4. Copy your bios UF2 file into the drive

# Firmware Build Instructions

1.- Download and Install [Visual Studio Code](https://code.visualstudio.com/download)

2.- Install extension: "Raspberry Pi Pico"

![Pico Extension](images/extension.png)

3.- Ensure SDK 1.5.1 selected as below...

![SDK 1.5.1](images/sdk.png)

4.- After SDK is installed, git submodules must be updated from command line by running:


**Windows:**

```
cd %HOMEPATH%\.pico-sdk\sdk\2.0.0
git submodule update --init --recursive
```


**Linux:** 
TODO
**MacOS:** 
TODO

5.- Go to Raspberry Pi Pico Tab in VS Code by clicking the Pico icon in the sidebar and from there click "Configure CMake"

6.- Go to Run and Debug Tab and select Build for your board

7.- Click "Start Debugging" (Green arrow)

8.- UF2 File will in the `build` folder generated during the compile process
# Docker
#### Setup
Build your base docker image with
```
docker build -t modxo-builder .
```

#
#### Firmware Build
```
docker compose run --rm builder
```

Output will be `out/modxo_[pinout].uf2`

There are also some extra parameters that can be passed to the build script:

- MODXO_PINOUT=`official_pico` | `yd_rp2040` | `rp2040_zero` - Default is `official_pico`.

- CLEAN=`y`: triggers a clean build. Default is disabled.

- BUILD_TYPE=`Release` | `Debug` - Default is `Debug`.


_Examples:_
```
MODXO_PINOUT=rp2040_zero BUILD_TYPE=Release docker compose run --rm builder
```
```
CLEAN MODXO_PINOUT=yd_rp2040 docker compose run --rm builder
```

#### Packing Bios locally
Place your bios file named `bios.bin` in this directory or place any bios files (regardless of their name) in the bios directory
```
docker compose run --rm bios2uf2
```

# Known bugs
 * Windbg get stuck sometimes when connected to Modxo SuperIO's serial port

# Notes
 * Currently, Modxo uses the ID 0xAF. Any derivative hardware with significant changes should ideally use a different ID. This is so that software like PrometheOS can target features appropriately.
 
# Attribution Requirement

     a) **Attribution:**  
       If you distribute or modify this work, you must provide proper 
       attribution to the original authors. This includes:
       - Mentioning the original project name: `Modxo`.
       - Listing the original authors: `Shalx / Team Resurgent`.
       - Including a link to the original project repository: 
           `https://github.com/Team-Resurgent/Modxo`.
       - Clearly stating any modifications made.

    b) **Logo and Branding:**  
       Any derivative work or distribution must include the logos provided by
       the original authors in accordance with the branding guidelines. The 
       logos must be displayed prominently in any interface or documentation 
       where the original project is referenced or attributed.

    c) **Branding Guidelines:**  
       You can find the logos and detailed branding guidelines in the 
       `BRANDING.md` file provided with this project. The logos must not be 
       altered in any way that would distort or misrepresent the original 
       branding.
