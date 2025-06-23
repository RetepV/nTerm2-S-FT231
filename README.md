# nTerm2-S

> [!NOTE]
> This is the continuation of the design of the nTerm2-S (found here: https://github.com/RetepV/nTerm2-S). Now that I am reaching completion, I have decided to switch from using a CP2102 to using an FT231. The FT231 is just easier to solder with a soldering iron. Or better said: it CAN be soldered with a soldering iron, whereas with the CP2102 that's near impossible, it was a bad choice on my part.\
> \
> The version with the CP2102 will be 'archived', i.e. frozen. It really does work, you can build it if you like, it will run any FabGL code and example (except the MS-Dos emulator for lack of memory). But I will abandon it and continue developing on the FT231 version.

This is my rendition of the uTerm2-S by Just4Fun (https://hackaday.io/project/181583-uterm2-s-a-multi-emulation-color-rs232-terminal). My one is in SMD, and I hopefully made it easily hand-solderable for everyone. Obviously the 'n' in nTerm stands for 'nano.' :P

The nTerm2-S has a slightly more usable form than the uTerm2-S, I think. Smaller and more of a format of a useful tool. A "throw it in a drawer and grab it when you need it"- type of device. Taking little desk space and layed out for quick plugging and unplugging serial devices. For that I have also made it so that it can switch between normal and cross-cable (null-modem) from software, so that all you need is one cable and don't need extender boxes or a special null-modem cable.

It can also switch between hardware-supported RTS/CTS and DTR/DSR handshaking. With the sidenote that DTR/DSR are connected to RTS/CTS of the ESP32: the ESP32 does not have dedicated DSR/DTR signals in it's UART. It should work fine that way with old hardware, as the ESP32 is so fast that it will probably never have to deassert DTR (in reality RTS) anyway. And if the data set deasserts DSR, it means that the ESP32 will actually see CTS deasserted, and wait until CTS asserts again before continuing.

The nTerm2-S is ultimately based on the FABGL library and board design from Fabrizio Di Vittori. http://www.fabglib.org. However, I skipped Fabrizio's implementation and based my design on Just4Fun's uTerm2-S. https://j4f.info/uterm2-s

This board will run all FabGL software and examples, <ins>except</ins> the PC emulator. The PC emulator needs PSRAM, but the nTerm2-S does not have enough I/O pins left to connect a PSRAM. Maybe I will do a V2 at some point, using an I/O expander to free up ESP32 pins for connecting a PSRAM and an SD card. Or maybe I will make a new design using a newer version CPU. But let's finish the nTerm2-S first... ;)

# Progress

12-6-2025

I actually ordered 5 new boards from JLCPCB on 5-6-2025 (€15,99 including shipping). They were finished and shipped on 7-6-2025. Arrived in Leipzig (Germany) on 8-6-2025. They are taking their time and still have the package in their posession on 12-6-2025. So now waiting for them to pass it over to The Netherlands so that it can be delivered to me. What I learn from this is that the Chinese are very efficient, while the Germans seem to have lost their knack. I guess I shouldn't expect anything until end of next week.

Just before sending the board for manufacturing, I decided to add a few more solder bridges.

OPT1: Option to choose between using the on-board audi amplifier, or leave it out completely and have line-out.
OPT2-OPT5: Option to choose between having the null-modem switching option or not. If you don't want to use the null-modem option, you can leave out the relais and their control circuitry, and use the OPT2-OPT5 solder bridges to connect the UART directly to the RS-232 V24 connector. Note that this also removes the capability to switch between RTS/CTS and DTR/DSR.

So if you want to save a bit of money and don't want to use those subcomponents, you now can solder the bridges accordingly, instead of using a bypass bodge wire.

<img width="1378" alt="Screenshot 2025-06-12 at 13 13 25" src="https://github.com/user-attachments/assets/23701a7f-c75e-431a-af3f-898092846d47" />

Ah, I also found out that the footprint for the VGA connector had been wrong all along. With both the prototypes, it was kind of hard to put the VGA connector pins in their holes. It turned out that the spacing was wrong. The connector would fit with a little bit of pushing, but that's not right. So I updated the footprint, and it should be good now.

5-6-2025

Three things progressed.

1. I decided to drop the supercap. I don't think it will last so long, they are meant to bridge short power downs, not long ones. I tested with a CR2032 and a BAT43 diode, and that works just as reliable as battery backup, but will give waaaaay more backup life.
2. I decided to add OPT2...OPT5 solder bridges to give people the options of leaving out the resistors and driver circuits. Soldering 1-2 on the solder bridges will make the signals go through the relay circuits, i.e. have software switchable RxD/TxD, RTS/CTS, or DTR/DSR. Soldering 2-3 will connect RxD, TxD, RTS and CTS directly to the RS-232 connector. It can save you some money if you don't need the switchability, and you can always change your mind later.
3. I also decided that This Is It. No more features, no more nothing, nTerm2-S is feature complete, and I am going to order new PCBs to see if everything actually still works. If everything works as planned, then I will consider nTerm2-S as being released. Software is hard on it's way. But if you can't wait, you can already use FabGL's AnsiTerminal example: the nTerm2-S hardware is backwards compatible.

The scary part is the FT-231 circuit. I have not tested it, and have to hope that I didn't make a mistake. The other more or less scary thing is that I had to move the AUDIO_PWM track, and hope that I did not introduce a bunch of crossover. The AUDIO_PWM track had to cross over the RxD_0 and TxD_0 lines. But these are only used for the ESP32 console ouput or during updating of the firmware. So if console output is removed, there should simply be nothing to cross over.

29-5-2025

The real time clock survived the night on only the backup power. :) At the last moment, I decided to put a series resistor of 150 ohm with the supercap (there was none in [Dallas's reference design](https://nl.mouser.com/datasheet/2/272/rtc-4-click-manual-v100-1483841.pdf). The inrush current when the supercap is empty of charge could be quite high. It's just for a short time, but theoretically it could cause such a power dip that it would cause boot loops. I haven't measured the inrush current, but "the word" is that it could even be multiple Amperes, which could theoretically also cause a track to burn.

I used a used Panasonic 1F 5.5V supercap that I had lying around, while I waiting for new ones to arrive.

I will also do some experiments with using a battery and an extra BAT43 diode. That's also 2 components, and maybe a better plan. If the voltage drop over the diode is too high, maybe I could use a NID5100?

28-5-2025

I just wanted to share some images of the progress I am making. I am still using the previous board with the CP2102 for developing and prototyping.

This is my development set up. I am testing with an actual computer, a Synertek SYM-1. It has 8K of ram, and runs Microsoft BASIC. Also note the tiny screen that I'm using. It's an old 800x480 LCD screen that I had laying around. I found out that it was LVDS, and purchased a board that can convert VGA (and HDMI and CVBS) to LVDS to drive the screen. Next thing is to make a case for it, but I couldn't wait to use it.

![IMG_3453](https://github.com/user-attachments/assets/ca41f64e-099e-4f32-ae9c-2923e83725bc)

Closeup of the nTerm2-S and the screen.

![IMG_3450](https://github.com/user-attachments/assets/5420cfc8-9396-4ef1-8951-0213b310a427)


Here I have added the DS2417 real time clock. It was easy, thanks to a whole bunch of people who created the [Arduino OneWire library](https://www.pjrc.com/teensy/td_libs_OneWire.html). I'm using a supercap as backup solution now, but might maybe switch to using a CR2023. But I'm not sure if that will work. I would have to add another diode in series making the voltage (in theory) 3V - 0.4V = 2.6V. But the lowest voltage supported by the DS2417 is 2.5V, so it's right on the edge. With the supercap I have some 3.2V. But the supercap will maybe last 2 weeks or so, while the CR2023 would probably last for years.

23-5-2025

What makes the difference between a home computer and a professional computer? The Real Time Clock! While working on the nTerm2-S firmware, I was able to add a status bar to the terminal. Memory is really tight, but CPU performance is not. So I thought: let's update it 10 times a second and show some sort of status of Rx, Tx, RTS and CTS, some blinkenlights. And then I thought that it would also be nice to show the time (and maybe date) in the status bar. That was quickly added. But well, what good are the time and date if you need to enter it manually every time you reset the terminal. So, we need an RTC.

Looking at the GPIO's left on the ESP32, there was a problem. Only one bidirectional GPIO (14) and one input-only (36) left. What can we do with that? I2C is out of the question, we need 1 output (or bidirectional) and 1 bidirectional GPIO for that. SPI even needs 3 GPIOs, and might possibly be done with only 2. But we have only 1 bidirectional wire... 1-wire! And looking around, I found the [Dallas Semiconductor DS2417](https://www.analog.com/media/en/technical-documentation/data-sheets/ds2417.pdf).

The DS2417 is not truly a real-time clock, but rather a 64-bit counter that is auto-incremented once a second, i.e. a timestamp. This is fine, in firmware we can convert it to a proper date and time with some functions. The counter can be battery backed-up. Perfect enough for a real-time clock. I will power it from a supercap, like in Dallas's design. Hopefully it will hold a charge for many months. If not, I might have to change the design again, to use a normal battery with a diode. First I'll build it on a breadboard to test it out somewhat.

The chip also has a kind of alarm function. Basically we can program it to give a /INT pulse after 1, 4, 32, 64, 2048, 4096, 65536, or 131072 seconds. Not of too much use, but as we also have one input-only GPIO left, we can connect the INT to it. Fortunately the input-only GPIO is GPIO14, which is one of the so-called RTC-GPIO and can be used to wake up the ESP32 from deep sleep. I don't have any idea for what we could use it. But it fills up the last unused GPIO pin, and it's a fitting for a complete feature-freeze and release of the hardware.

So said, so done (Dutch proverb). It was a bit of a tight squeeze because of the supercap battery's size. And I have always tried to make sure that the AUDIO_PWM is not close to any of the high-speed signals to prevent crosstalk (which would be hearable in the audio), so I had to lay that out differently as well. Hopefully crosstalk from the video into the audio track will still be next-to-nonexistent.

So now we are here:

<img width="1818" alt="Screenshot 2025-05-23 at 11 27 28" src="https://github.com/user-attachments/assets/9a63f8f6-6236-4865-a0c3-fd630d14c377" />
<img width="1211" alt="Screenshot 2025-05-23 at 11 28 12" src="https://github.com/user-attachments/assets/35e091cb-8858-4227-a7e2-b4f4d4147348" />

# License

THIS IS A WORK IN PROGESS! I.E. UNLESS STATED AS WORKING, ASSUME THAT IT DOES NOT WORK! (but it's looking real good ;))

This design is available under GPL License V3. This means that I DO NOT release it for commercial use, unless with my explicit written and signed consent. Please contact me first if you have commercial questions.

uTerm2-S: https://hackaday.io/project/181583-uterm2-s-a-multi-emulation-color-rs232-terminal  
fabgl: http://www.fabglib.org/, https://github.com/fdivitto/FabGL
