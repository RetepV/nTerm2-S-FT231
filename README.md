# nTerm2-S

> [!NOTE]
> This is the continuation of the design of the nTerm2-S (found here: https://github.com/RetepV/nTerm2-S). Now that I am reaching completion, I have decided to switch from using a CP2102 to using an FT231. The FT231 is just easier to solder with a soldering iron. Or better said: it CAN be soldered with a soldering iron, whereas with the CP2102 that's near impossible, it was a bad choice on my part.\
> \
> The version with the CP2102 will be 'archived', i.e. frozen. It really does work, you can build it if you like, it will run any FabGL code and example (except the MS-Dos emulator for lack of memory). But I will abandon it and continue developing on the FT231 version.\
> \
> For instance: while developing the software, I found that it really did need a real-time-clock, so I have added that to nTerm2-S-FT231, and will not add it the CP2102 version (unless someone really, really wants it :))

This is my rendition of the uTerm2-S by Just4Fun (https://hackaday.io/project/181583-uterm2-s-a-multi-emulation-color-rs232-terminal). My one is in SMD, and I hopefully made it easily hand-solderable for everyone. Obviously the 'n' in nTerm stands for 'nano.' :P

The nTerm2-S has a slightly more usable form than the uTerm2-S, I think. Smaller and more of a format of a useful tool. A "throw it in a drawer and grab it when you need it"- type of device. Taking little desk space and layed out for quick plugging and unplugging serial devices. For that I have also made it so that it can switch between normal and cross-cable (null-modem) from software, so that all you need is one cable and don't need extender boxes or a special null-modem cable.

It can also switch between hardware-supported RTS/CTS and DTR/DSR handshaking. With the sidenote that DTR/DSR are connected to RTS/CTS of the ESP32: the ESP32 does not have dedicated DSR/DTR signals in it's UART. It should work fine that way with old hardware, as the ESP32 is so fast that it will probably never have to deassert DTR (in reality RTS) anyway. And if the data set deasserts DSR, it means that the ESP32 will actually see CTS deasserted, and wait until CTS asserts again before continuing.

The nTerm2-S is ultimately based on the FABGL library and board design from Fabrizio Di Vittori. http://www.fabglib.org. However, I skipped Fabrizio's implementation and based my design on Just4Fun's uTerm2-S. https://j4f.info/uterm2-s

This board will run all FabGL software and examples, <ins>except</ins> the PC emulator. The PC emulator needs PSRAM, but the nTerm2-S does not have enough I/O pins left to connect a PSRAM. Maybe I will do a V2 at some point, using an I/O expander to free up ESP32 pins for connecting a PSRAM and an SD card. Or maybe I will make a new design using a newer version CPU. But let's finish the nTerm2-S first... ;)

THIS IS A WORK IN PROGESS! I.E. UNLESS STATED AS WORKING, ASSUME THAT IT DOES NOT WORK! (but it's looking real good ;))

This design is available under GPL License V3. This means that I DO NOT release it for commercial use, unless with my explicit written and signed consent. Please contact me if you have commercial questions.

uTerm2-S: https://hackaday.io/project/181583-uterm2-s-a-multi-emulation-color-rs232-terminal  
fabgl: http://www.fabglib.org/, https://github.com/fdivitto/FabGL
