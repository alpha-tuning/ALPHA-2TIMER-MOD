# ALPHA SST Replacement with 2Timer Live Mod

## Overview

The **ALPHA SST Replacement with 2Timer Live Mod** is a legacy Honda OBD1 ECU flash replacement project designed around PLCC32 flash memory devices adapted into the traditional 28-pin DIP footprint used by common 27SF512-style chipped ECU setups.

This board was created as an improved variation of the earlier ALPHA SST replacement hardware. It allowed larger flash devices to be installed in a Honda OBD1 ECU socket while also adding safer live bank switching through a flip-flop based 2Timer modification.

The purpose of this project was to make legacy chip-based tuning more flexible before low-cost real-time emulation became widely available.

## Supported Flash Devices

This design was intended for PLCC32 flash devices commonly used in legacy ECU memory replacement and expanded-map workflows.

Supported devices include:

```text
SST39SF040
SST39SF010
ST M29F040B
AMD AM29F010
```

These chips were mounted to a custom breakout board that allowed them to fit into a standard 28-pin DIP ECU socket originally intended for 27SF512-style hardware.

## Purpose of the Design

Traditional Honda OBD1 chip tuning commonly used 27SF512 EEPROMs in a 28-pin DIP socket. Larger PLCC32 flash chips provide more memory space, but they do not directly match the original ECU socket footprint.

This project solved that by adapting the larger PLCC32 flash devices into the original 28-pin DIP format while exposing the higher address lines needed for bank selection, live map switching, and programming configuration.

The result was a compact flash replacement board that could be installed like a normal chipped ECU ROM while supporting expanded memory layouts and multiple calibration banks.

## 2Timer Live Mod

The **2Timer Live Mod** adds synchronized bank switching logic to the flash replacement board.

Instead of allowing a manual switch to directly change the active flash bank at any random point during ECU operation, this design uses a flip-flop to catch the rising edge and apply the requested bank change in a more controlled way.

This was added because live map switching on address lines can occasionally trip the **MIL / Check Engine Light** if the bank changes while the ECU is actively reading from the ROM.

On some setups, if the active map was switched outside of the approximate **180 ns OE high window**, the ECU could briefly see an unstable address state or invalid ROM data. That small glitch could be enough to cause a fault condition.

The flip-flop based 2Timer circuit helps reduce this problem by latching the requested bank change instead of letting the address line switch asynchronously.

This does not guarantee that every live-switching condition is safe, but it greatly improves reliability compared to a direct mechanical or unsynchronized bank switch.

## Address Line Handling

Address lines above A14 were handled with **10K pull-up and pull-down resistors** to prevent floating inputs.

Floating high address lines can cause unwanted sector selection, random bank changes, unstable reads, or unexpected behavior during ECU operation and programming.

The design exposes **A15** and **A16** through a header configuration so the board can be set for fixed bank selection, live map switching, or programming mode.

This also allowed the chips to be programmed without needing a dedicated 32-pin adapter, especially when used with ALPHAburner.

## ALPHAburner Compatibility

These boards were designed to work with **ALPHAburner**.

The original ALPHAburner hardware was based on an STM32 microcontroller and is now deprecated.

The final variation moved to an RP2350B-based design before ALPHA development shifted toward real-time emulation hardware.

This repository remains focused on the legacy flash replacement and 2Timer hardware workflow.

## Legacy Status

This project is considered **legacy hardware**.

At the time these boards were developed, flash-based chip tuning was still a practical way to modify Honda OBD1 ECUs. Since then, open-source real-time emulation solutions such as **oneROM** have made live emulation extremely affordable and accessible.

With oneROM making real-time emulation possible at roughly a $10 hardware cost, traditional chip tuning has largely been put to bed for development, testing, calibration, and live map switching workflows.

This repository is preserved for educational use, historical reference, existing hardware maintenance, and documentation of the ALPHA hardware development timeline.

## Educational Use

The files, designs, documentation, notes, and related information in this repository are provided for **educational and reference use only**.

This project is shared to help others learn about Honda OBD1 ECU memory hardware, PLCC32 flash replacement, address line configuration, bank selection, synchronized bank switching, flip-flop based edge capture, and legacy chip tuning workflows.

You are responsible for verifying safety, compatibility, timing behavior, electrical behavior, and proper use before applying these designs to any ECU, vehicle, programmer, or hardware setup.

## Attribution Requirement

Any use, modification, redistribution, derivative hardware, documentation, video content, product listing, public project, or repository based on this design must provide clear attribution to:

```text
ALPHA / ALPHA Tuning
https://github.com/alpha-tuning
```

Attribution must remain visible in any public repository, documentation, schematic, PCB layout, board file, firmware reference, video description, product page, or derivative version that uses or references this work.

You may not remove ALPHA attribution, claim this design as your own original work, or present modified versions in a way that hides the original source.

If this project is used as the basis for another design, that design must clearly state that it is derived from the original ALPHA SST Replacement with 2Timer Live Mod hardware.

## Disclaimer

This repository is provided as-is, with no warranty, guarantee, or promise of fitness for any particular use.

Working with ECU memory hardware can affect vehicle operation. Incorrect wiring, incorrect programming, unstable bank switching, invalid calibration data, or improper installation can cause ECU faults, drivability issues, or hardware damage.

Use these files and designs at your own risk.

## Project Status

Development on this hardware has ended.

Future ALPHA work is focused on modern real-time emulation platforms, including ALPHAemu, AlphaLink, and open-source related workflows such as oneROM.
