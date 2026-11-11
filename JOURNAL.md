---
title: "MP3-Gotchi-v2"
author: "Patryk Ankudowicz"
description: "Hardware development journal for the MP3-Gotchi-v2 music player."
created_at: "2026-07-07"
---

# July 7 - v0.0.1 - Starting over

Today I decided to restart the hardware side of MP3 Gotchi instead of continuing with the first prototype approach.

The first version helped me understand the idea, but it also showed its limits. Using dev modules made the design bigger than it needed to be, limited the part choices, and hid too many things behind small black boxes. For a project that could be expanded by the community, that felt like the wrong direction.

So I started planning v2 as a proper single-board device. The main goals are to keep it small, cheap and battery-friendly, while also making the audio side much better. I want the output to work well with 24 ohm in-ear monitors, so the amplifier, DAC path, power supply and connector choices need to be picked more carefully than in the first version.

Most of today was spent researching and deciding the core hardware direction:
USB-C for power and programming, Li-Ion power, microSD storage, and a proper external audio path instead of relying on random modules.

![0.0.1 - Schematic](hardware/reversions/v0.0.1/schematic/MP3-Gotchi_kicad_v2-v0.0.1.svg)

**Total time spent: [6h 28m](https://lapse.hackclub.com/timelapse/TcRorU8f71nD)**

# July 8 - v0.0.2 - Finishing the audio path, microphone, and UART

I never thought I would spend this much time on just a JACK port...

Since most in-ear headphones use TRRS and already include a microphone, I decided to support it in the project. Maybe the community will find some interesting use for it later:)

The connector datasheet was very unclear, so figuring out the TRRS pinout took a while. After that, the microphone circuit went fairly smoothly with some Android headset documentation and the op-amp knowledge I already had from school.

The final part was the DAC. Unlike the older ESP32, the ESP32-S3 has no built-in analog DAC, so I needed an external one. I chose the PCM5102A from TI because it has very good declared parameters and should work well for a high-quality headphone player, especially with in-ear monitors.
There were not many sensible alternatives. Most locally available DACs were either cheap single-channel parts, difficult to buy, or only available with expensive international shipping. The most practical option was a complete dev board, which I plan to desolder and reuse.

I will do the same with the ESP32-S3 module. A complete development board often costs almost the same as the WROOM module itself, so I can recover the module, TVS diode, USB-C port and maybe the button. The remaining board can then be used as a programmer.

At first I wanted to move the whole programming circuit onto the MP3-Gotchi PCB, but then I realised this is supposed to be an MP3-Gotchi, not another dev board. Exposing a few programming pins is enough and also makes the design less dependent on one specific UART layout.

I want the project to stay understandable and possible to build even by you:)

![0.0.2 - Schematic](hardware/reversions/v0.0.2/schematic/MP3-Gotchi_kicad_v2-v0.0.2.svg)

**Total time spent: [7h 07m](https://lapse.hackclub.com/timelapse/vYrmMcLXm_-O)**

# v0.0.3 - Making the hardware plan, finishing the microphone, and adding ENC and MOT

Maybe I should have started with the plan, but better now than later. I finally got to the most important part and started planning everything properly. For now, I wrote down a list of the hardware components that will later be arranged into the final block diagram. You can find it in `/docs/diagram/components-list_MP3-Gotchi-v2.md`.

After making the list, I had everything in one place and could start adding more peripherals without constantly jumping between ideas. Today I added the rotary encoder, which was quick and simple.

Next were the vibration motors. It was obvious that they would be controlled with MOSFETs, but while reminding myself how to implement them properly instead of doing it quickly, I found many different approaches. Luckily, I managed to find what seems to be a reliable article and successfully implemented two of them. Maybe this could give some interesting “stereo” feeling in a future community use :)

I also decided to audit yesterday’s schematic with a clear head, which I really recommend, because otherwise the whole thing slowly turns into a Jenga tower. I found a few smaller and bigger issues.

One example was the PCM5102A datasheet. It turned out that I had been reading the 2012 version, while a newer one from 2015 exists, so I had to rebuild some part of the circuit.

The bigger issue was the microphone path. It was supposed to be a simple thing added quickly, but it turned out like it usually does. After running a simulation in Falstad, many things did not match, so almost the entire microphone preamplifier had to be rebuilt.

![0.0.3 - Schematic](hardware/reversions/v0.0.3/schematic/MP3-Gotchi_kicad_v2-v0.0.3.svg)

**Total time spent: [5h 21m](https://lapse.hackclub.com/timelapse/LOAxFAgoULYP)**

# v0.0.4 - Adding the OLED and the remaining peripherals, and finishing the first full schematic

The schematic is almost finished!

Today I finally checked off everything from the components list. Only the usual fixes that appear during PCB design remain, but I am leaving those for the next part.

Most of today was spent adding peripherals, including the classic MPU6050 IMU. It only needed two external capacitors, which was a nice surprise.

Next, I moved on to the [SSD1331 display I had previously bought](https://sklep.msalamon.pl/produkt/wyswietlacz-oled-095-spi-kolorowy/) for the v1 version. Even after a lot of research, I could not understand its full circuit well enough to move it directly onto the main board. In the end, that was not necessary. Using it as a separate board connected through a PH connector and mounted directly to the enclosure will be a better option. It also gives me more freedom when designing the final enclosure.

Since the jack has a tip shunt, I decided to use it to support a small 8 ohm speaker inside the enclosure. This way, the music will not always have to stay inside the headphones and could be shared with other people, for example while sitting in a park and playing cards:)
At first, this was supposed to be a quick extra feature. But since the whole project should be properly made, this part should be done properly too.
I added the PAM8302, which honestly gave me a small dopamine rush when I found it. It is mono, supports 3.3 V, needs almost no external components and was the cheapest Class D audio amplifier I could find.
It went straight into the schematic. I only had to add a pair of input resistors to adjust its gain for the higher signal level coming from the previous amplifier stage.

The final additions were two buttons that will eventually have “some” function, because one rotary encoder with a single button probably will not be enough in the long run, or maybe its can be special features?

![0.0.4 - Schematic](hardware/reversions/v0.0.4/schematic/MP3-Gotchi_kicad_v2-v0.0.4.svg)

**Total time spent: [3h 23m](https://lapse.hackclub.com/timelapse/U2HmR3q3GXm7)**

---

### A quick update before the next revisions

Well, summer ended, school started, and somehow three months went by without a single project update.

After finishing v0.1.0, I decided to make things a little harder for myself and try manufacturing the PCB at home. So, even though I wasn't posting updates, I spent most of that time working on an Ender3CNC conversion.

It's almost finished now, but I eventually realized that it's much better suited for engraving wood than milling tiny PCB traces with 0.1 mm precision. Looking back, I also spent way too much time convincing myself that I had to finish the CNC before I could continue with MP3-Gotchi.

And that's how a three-month break happened :)

But this was supposed to be a fun gift, not another unfinished project sitting in a drawer. With school competitions coming up, I finally decided to stop delaying things and get back to finishing it properly.

For now, I'm switching to professional PCB manufacturing. I'll eventually rebuild the CNC into a smaller, more precise machine with lead screws, so I can use it for future projects whenever I need a PCB quickly.

The next few revisions were actually made during this break and have been sitting on my drive waiting to be documented and committed. That's why I'm adding this note here rather than at the end of the journal.

Now, back to MP3-Gotchi!

---

# v0.0.5 - Preparing the PCB and moving to Linux

With the schematic mostly finished, I could finally move on to the actual PCB design!

I started by going through the component footprints, adjusting the design rules and preparing everything for placing the components on the board. I also spent quite a bit of time researching parts on TME and putting together the BOM, because there's not much point in designing a PCB around components I can't actually buy :)

Around the same time, I decided to move from Windows to Linux Mint. I thought it would be a fairly straightforward switch, but of course, a few unexpected software problems showed up along the way.

So instead of spending the entire session on the PCB, I ended up dealing with the new environment too. Still, I managed to make progress, and the project was finally moving beyond just drawing a schematic.

**Total time spent: [11h 57m](https://lapse.hackclub.com/timelapse/H-KEtL5mgBXW)**

# v0.0.6 - More PCB work, software issues, and ordering components

I continued working on the PCB, although things didn't go quite as smoothly as I had hoped.

Switching between Windows and Linux Mint turned out to be more troublesome than expected. I ran into software errors that slowed down the whole process, so a good part of this session was spent troubleshooting instead of actually designing the board.

Not exactly the kind of progress I wanted to make, but I guess that's part of working on a project like this :)

The good news is that I finally ordered all the components from my BOM! At least now I won't have to worry about finding and ordering everything when the PCB is ready.

With the components on their way, I could focus on finishing the board itself.

**Total time spent: [2h 45m](https://lapse.hackclub.com/timelapse/Vv391Sbhwzbg)**

# v0.1.0 - Working on the first PCB layout

Finally, some real PCB work!

With the schematic mostly finished and all the components ordered, I could focus on turning everything into an actual board.

Most of this revision was spent placing components, routing traces and checking the design rules. This is where the schematic started turning into something that could eventually be manufactured.

I also began preparing the design for Gerber export, although the PCB still needed some refinement before I could call it ready for production.

It's definitely more complicated than just connecting everything in the schematic. Now I also have to think about where each component goes, how the traces are routed and whether the whole thing can actually be manufactured.

Still, seeing the project slowly take the shape of a real PCB after all the work on the schematic feels like a pretty big step forward.

Not quite ready to manufacture yet, but getting closer!

**Total time spent: [6h 02m](https://lapse.hackclub.com/timelapse/XBWMIvsa8Shx)**