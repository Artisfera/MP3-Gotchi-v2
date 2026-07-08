# v0.0.1 - Starting over (7.7.2026)

[_Time spent: 6h 28m_](https://lapse.hackclub.com/timelapse/TcRorU8f71nD)

Today I decided to restart the hardware side of MP3 Gotchi instead of continuing with the first prototype approach.

The first version helped me understand the idea, but it also showed its limits. Using dev modules made the design bigger than it needed to be, limited the part choices, and hid too many things behind small black boxes. For a project that could be expanded by the community, that felt like the wrong direction.

So I started planning v2 as a proper single-board device. The main goals are to keep it small, cheap and battery-friendly, while also making the audio side much better. I want the output to work well with 24 ohm in-ear monitors, so the amplifier, DAC path, power supply and connector choices need to be picked more carefully than in the first version.

Most of today was spent researching and deciding the core hardware direction:
USB-C for power and programming, Li-Ion power, microSD storage, and a proper external audio path instead of relying on random modules.

![0.0.1 - Schematic](hardware/reversions/v0.0.1/schematic/MP3-Gotchi_kicad_v2-v0.0.1.svg)

---

# v0.0.2 - Finishing the audio path, microphone, and UART

[*Time spent: >7h (the lapse crashed on me)*](https://lapse.hackclub.com/timelapse/vYrmMcLXm_-O)

I never thought I would spend this much time on just a JACK port...

Since most in-ear headphones use TRRS and already include a microphone, I decided to support it in the project. Maybe the community will find some interesting use for it later:)

The connector datasheet was very unclear, so figuring out the TRRS pinout took a while. After that, the microphone circuit went fairly smoothly with some Android headset documentation and the op-amp knowledge I already had from school.

The final part was the DAC. Unlike the older ESP32, the ESP32-S3 has no built-in analog DAC, so I needed an external one. I chose the PCM5102A from TI because it has very good declared parameters and should work well for a high-quality headphone player, especially with in-ear monitors.
There were not many sensible alternatives. Most locally available DACs were either cheap single-channel parts, difficult to buy, or only available with expensive international shipping. The most practical option was a complete dev board, which I plan to desolder and reuse.

I will do the same with the ESP32-S3 module. A complete development board often costs almost the same as the WROOM module itself, so I can recover the module, TVS diode, USB-C port and maybe the button. The remaining board can then be used as a programmer.

At first I wanted to move the whole programming circuit onto the MP3-Gotchi PCB, but then I realised this is supposed to be an MP3-Gotchi, not another dev board. Exposing a few programming pins is enough and also makes the design less dependent on one specific UART layout.

I want the project to stay understandable and possible to build even by you:)

![0.0.2 - Schematic](hardware/reversions/v0.0.2/schematic/MP3-Gotchi_kicad_v2-v0.0.2.svg)

---


# v0.0.3 - Making the hardware plan, finishing the microphone, and adding ENC and MOT

[_Time spent: 5h 21m_](https://lapse.hackclub.com/timelapse/LOAxFAgoULYP)

Maybe I should have started with the plan, but better now than later. I finally got to the most important part and started planning everything properly. For now, I wrote down a list of the hardware components that will later be arranged into the final block diagram. You can find it in `/docs/diagram/components-list_MP3-Gotchi-v2.md`.

After making the list, I had everything in one place and could start adding more peripherals without constantly jumping between ideas. Today I added the rotary encoder, which was quick and simple.

Next were the vibration motors. It was obvious that they would be controlled with MOSFETs, but while reminding myself how to implement them properly instead of doing it quickly, I found many different approaches. Luckily, I managed to find what seems to be a reliable article and successfully implemented two of them. Maybe this could give some interesting “stereo” feeling in a future community use :)

I also decided to audit yesterday’s schematic with a clear head, which I really recommend, because otherwise the whole thing slowly turns into a Jenga tower. I found a few smaller and bigger issues. 

One example was the PCM5102A datasheet. It turned out that I had been reading the 2012 version, while a newer one from 2015 exists, so I had to rebuild some part of the circuit.

The bigger issue was the microphone path. It was supposed to be a simple thing added quickly, but it turned out like it usually does. After running a simulation in Falstad, many things did not match, so almost the entire microphone preamplifier had to be rebuilt.

![0.0.3 - Schematic](hardware/reversions/v0.0.3/schematic/MP3-Gotchi_kicad_v2-v0.0.3.svg)
