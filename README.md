# MP3-Gotchi v2

A pocket-sized offline MP3 player with a Tamagotchi-style monkey companion, being developed around a custom ESP32-S3 PCB.


## Original version (v1)
<img
  src="https://raw.githubusercontent.com/Artisfera/MP3-Gotchi/main/hardware/3dmodels/0.3.0/MP3-Gotchi-0.3.0.png"
  alt="Original MP3-Gotchi modular prototype"
  width="720"
/>


## Why?

Have you ever lost signal during a trip and realised that streaming had quietly become your entire music library?

You can store MP3 files on a phone, of course, but carrying a dedicated music player still feels different.

My girlfriend runs into this problem often, so for our anniversary I decided to build her a one-of-a-kind Tamagotchi-style MP3 player. While listening to music, she will also be able to feed and take care of a little monkey companion.

She loves monkeys, so it fits perfectly ✨


## What makes v2 different?

The first MP3-Gotchi proved that the idea could work, but it was built mostly from ready-made development modules. That made the device larger, limited the available hardware choices and left many parts of the design hidden inside separate boards.

Version 2 is being rebuilt around a custom ESP32-S3 PCB. Instead of connecting complete modules together, the main parts of the player are being designed directly into one compact device.

The new version is planned to include:

- offline music playback from a microSD card
- a dedicated stereo DAC and headphone amplifier
- support for wired TRRS headphones and their microphone
- a small internal speaker
- a 96×64 color OLED
- motion sensing
- dual vibration feedback
- a rotary encoder and physical buttons
- USB-C charging and programming
- a rechargeable Li-Ion battery

These are the goals for the complete v2 design. The current HomeFab prototype does not include every planned feature.

## Current status

> [!IMPORTANT]
> MP3-Gotchi v2 is under active hardware development. The version currently available in this repository is **HomeFab v0.1.0**, a reduced PCB design intended for experimental home fabrication. The full version for professional PCB manufacturing is a future development step. The board has not been documented here as manufactured and hardware-tested.

### HomeFab v0.1.0

The HomeFab revision omits microphone support, the dedicated battery over-discharge protection circuit, and the second vibration motor. These are still part of the intended full v2 design.

| Block | Component | HomeFab v0.1.0 status |
| --- | --- | --- |
| USB-C | USB-C receptacle | Included in schematic |
| Battery charger | TP4056 | Included in schematic |
| Battery connection | Li-Ion cell connector | Included in schematic |
| Battery protection | DW01A + FS8205A | Not included in HomeFab |
| 3.3 V buck converter | SC189ZSKTRT | Included in schematic |
| Microcontroller | ESP32-S3-WROOM | Included in schematic |
| Stereo DAC | PCM5102A | Included in schematic |
| Headphone amplifier | TS482IDT | Included in schematic |
| Microphone preamp | TLV9062 | Not included in HomeFab |
| TRRS headphone jack | FC68129 | Included; microphone circuitry omitted |
| microSD socket | ATTEND 112I-TA01 | Included in schematic |
| Rotary encoder | Encoder with push switch | Included in schematic |
| Vibration motors | Mini vibration motor | One included; second omitted |
| 6-axis IMU | MPU6050 | Included in schematic |
| Color OLED | SSD1331, 96×64 | Separate module, connected to main PCB |
| Internal speaker amplifier | PAM8302AAD | Included in schematic |
| Buttons | Two user buttons | Included in schematic |

**Project files:** [HomeFab KiCad project](hardware/MP3-Gotchi-v2_HomeFab/) | [HomeFab BOM (CSV)](hardware/MP3-Gotchi-v2_HomeFab/MP3-Gotchi-v2_HomeFab.csv) | [Historical revision exports](hardware/reversions/)

The BOM is located in the HomeFab directory. It is a working component and purchasing list, not yet a verified manufacturing BOM. Some entries still refer to components omitted from the current HomeFab schematic and need to be reconciled before ordering PCB assembly.

## Development journal

The project is being documented from the first hardware decisions to the final physical build. The journal includes design choices, mistakes, revisions and timelapses from each development session.

[Read the development journal](JOURNAL.md)