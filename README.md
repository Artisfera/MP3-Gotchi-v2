# MP3-Gotchi v2

A pocket-sized offline MP3 player with a Tamagotchi-style monkey companion, built from scratch around a custom ESP32-S3 PCB.


## v1 version:
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

## Current status

> [!IMPORTANT]
> MP3-Gotchi v2 is under active hardware development


| Block                 | Component                  | Status   | Notes                                                                  |
| --------------------- | -------------------------- | -------- | ---------------------------------------------------------------------- |
| USB-C                 | USB-C port 12PIN           | Designed |                                                                        |
| Battery Charger       | TP4056                     | Designed |                                                                        |
| Battery Cell          | Li-Ion                     | Designed |                                                                        |
| Bttery Protection     | DW01A & FS8205A            | Designed |                                                                        |
| 3.3V Buck Converter   | SC189ZSKTRT                | Designed |                                                                        |
| Microcontroller       | ESP32-S3-WROOM             | Designed |                                                                        |
| DAC Converter         | PCM5102A                   | Designed |                                                                        |
| Power Amp.            | LM4808M / TS482IDT         | Designed | LM can be better for Battery supply, TS is maybe can be better for sound quality. |
| Mic Preamp.           | TLV9062IDR                 | Designed | CTIA Standard                                                          |
| TRRS mini Jack        | FC68129                    | Designed |                                                                        |
| micro SD Card         | 112I-TA01                  | Designed |                                                                        |
| Encoder               | Rotary Encoder with switch | Designed | 15mm                                                                   |
| Vibration Motor       | mini Vibration Motor       | Designed |                                                                        |
| 6-Axis IMU            | MPU6050                    | Designed |                                                                        |
| 96x64 RGB OLED        | SSD1331                    | Designed | 96×64, 0.95", SPI                                                     |
| Internal Speaker Amp. | PAM8302AAD                 | Designed |                                                                        |
| Buttons               | Tact switch                | Designed | Two functional buttons                                                 |
| Cool PCB art          |                            |          |                                                                        |


## Development journal

The project is being documented from the first hardware decisions to the final physical build. The journal includes design choices, mistakes, revisions and timelapses from each development session.

![JOURNAL](JOURNAL.md)