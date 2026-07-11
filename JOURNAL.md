# v0.0.1 - Starting over (7.7.2026)

[_Time spent: 6h 28m_](https://lapse.hackclub.com/timelapse/TcRorU8f71nD)

Today I decided to restart the hardware side of MP3 Gotchi instead of continuing with the first prototype approach.

The first version helped me understand the idea, but it also showed its limits. Using dev modules made the design bigger than it needed to be, limited the part choices, and hid too many things behind small black boxes. For a project that could be expanded by the community, that felt like the wrong direction.

So I started planning v2 as a proper single-board device. The main goals are to keep it small, cheap and battery-friendly, while also making the audio side much better. I want the output to work well with 24 ohm in-ear monitors, so the amplifier, DAC path, power supply and connector choices need to be picked more carefully than in the first version.

Most of today was spent researching and deciding the core hardware direction:
USB-C for power and programming, Li-Ion power, microSD storage, and a proper external audio path instead of relying on random modules.

![0.0.1 - Schematic](hardware/reversions/v0.0.1/schematic/MP3-Gotchi_kicad_v2-v0.0.1.svg)

---

# v0.0.2 - Finishing the audio path, microphone, and UART (8.7.2026)

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


---

# v0.0.4 - Dodanie OLED i reszty akcesoriów i wstępne skończenie schematu

[_Time spent: 3h 23m_](https://lapse.hackclub.com/timelapse/LOAxFAgoULYP)

Już prawie koniec schematu! Dzisiaj już odchaczyłem wszystko z components list i zostały tylko poprawki które wręcz zawsze są przy projektowaniu PCB co zostawiam już na następną część, stąd tylko tyle godzin.

Głównie robiłem dzisiaj peryferia takie jak IMU na klasycznym MPU6050 który wymagał od siebie tylko 2 kondensatorów co było mi miłym zaskoczeniem.

Następnie zabrałem się za [SSD1331 kupionego wcześniej](https://sklep.msalamon.pl/produkt/wyswietlacz-oled-095-spi-kolorowy/) do v1 wersji. Niestety po długim reasearchu, ciężko mi było odszyfrować jego schemat by przenieść na płytkę, ale nie było to potrzebne finalnie. Lepszym wyborem będzie użycie jej jako oddzielnej płytki, połącznie przez PH złącze i przymocnowanie do obudowy, co nałoży znacznie mniejsze ograniczenia na finalną obudowę względem stale przylutowanego ekranu na PCB.

Jako, że mam w jacku tip shunt, to postanowiłem go wykorzystać, dodając możliwość podłączenia wewnętrz obudowy głośniczka 8ohm, by można było w razie potrzeb słuchać muzyki nie tylko samemu, ale też z innymi przykładowo grając w karty w parku:)
Planowałem by to była szybka i "aby było" opcja, ale jednak jak już założyłem że będzie to porządnie zrobiony projekt, to będzie porządnie. Dodałem więc PAM8302, który finanlie dał mi sporo endorifn jak go znalazłem, bo akurat mono, wspiera 3.3v, nie wymaga teoretycznie nic zewnętrznego i najtańszy z wzmacniaczy audio klasy D. Trafił więc do schematu, jedynie co musiałem zrobić to dać 2 rezystory na wejściu, by dostosować wzmocnienie do wyższego napięcia które przecież idzie z pre ampa.

Na koniec zostały dwa przyciski które będą pełniły "jakąś" funkcje, bo jeden enkoder z jednym przyciskiem średnio zdadzą egzamin na dłuższą mete.

![0.0.4 - Schematic](hardware/reversions/v0.0.4/schematic/MP3-Gotchi_kicad_v2-v0.0.4.svg)


# v0.0.4 - Adding the OLED and the remaining peripherals, and finishing the first full schematic

[*Time spent: 3h 23m*](https://lapse.hackclub.com/timelapse/U2HmR3q3GXm7)

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
