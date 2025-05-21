# nTerm2-S
My rendition of the uTerm2-S by Just4Fun (https://hackaday.io/project/181583-uterm2-s-a-multi-emulation-color-rs232-terminal). This one is in SMD, and I hopefully made it easily hand-solderable for everyone (with maybe the exception of the CP2102). Obviously the 'n' in nTerm stands for 'nano.' :P

The nTerm2-S has a slightly more usable form the uTerm2-S, I think. Smaller and a little more usable when sitting permanently on a desk. And as an extra, it can switch between normal and cross (null-modem) from the software so that you wil never have to search for that null-modem cable ever again. It can also switch between RTS/CTS or DTR/DSR handshaking.

> [!NOTE]
> This is basically the same design as the nTerm2-S (found here: https://github.com/RetepV/nTerm2-S). But now that I am reaching completion, I have decided to switch from using a CP2102 to using an FT231 because it is just easier to solder with a soldering iron (or better: it CAN be soldered with a soldering iron, whereas with the CP2102 that's near impossible, it was a bad choice on my part). The version with the CP2102 will be 'archived'. It really does work, you can build it if you like, but I will continue developing on the FT231 version. For instance: while developing the software, I found that it really did need a real-time-clock, so I have added that to nTerm2-S-FT231. But I might be open to requests about the CP2102 version, so you can always ask. :)

This design is available under GPL License V3. This means that I DO NOT release it for commercial use, unless with my explicit written and signed consent. Please contact me if you have commercial questions.

uTerm2-S: https://hackaday.io/project/181583-uterm2-s-a-multi-emulation-color-rs232-terminal  
fabgl: http://www.fabglib.org/, https://github.com/fdivitto/FabGL
