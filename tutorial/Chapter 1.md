# Wii U Programming
A quick thrash for existing programmers

**Chapter 1 - An Overview of the Wii U**

Heya! (announcer voice)Welcome... to the world of Wii U programming. In all seriousness; hi! This is a quick guide for pre-existing programmers to get their head around the Wii U and what it has to offer. It's a great console that's been pretty much completely cracked open (despite what GBATemp will tell you, an IOSU exploit is *not* needed to make homebrew with all the power of a retail game). Programming is a challenge, but it's fun; and the end results are always satisfying. I hope to see your apps running on my console soon!

## Prerequisites
 - A Wii U and a computer
 - Fairly decent C or C++ experience (Personally, I used knowledge of Java to pick up C as I went. I don't recommend it.)
 - Facilities to compile homebrew (if you can compile Loadiine you can compile anything)
 - Basic knowledge of the Wii U scene (poke around GBATemp for a bit)
 - The right mindset (exactly what this means is up to you)

## A Quick Recap of Computer Science
Just in case you're a bit behind on your computer science, we're going to quickly tear through some basics so we're all up to speed. Ready? Let's go!

Any computer has a processor, some memory and various I/O things. The processor reads instructions from memory and executes them. Each instruction might be telling the processor to read something from memory, add some numbers, whatever. Memory (specifically RAM) is a spot where the processor can store data, and lots of it. Each byte of memory has an "address"; counted in hex (0 up to F). Due to the way computers are wired up inside, different memory addresses can often refer to different chips on the motherboard; the Wii U has several memory chips that all have their own address "ranges" (for example, 0xF4000000 to 0xF6000000 is the "MEM1" chip). These ranges are often not the same as the way the machine is wired up; processors usually have some method of "mapping" addresses.

That's a lot to take in, but if you can get it (especially memory addressing) you'll find that low-level poking on the Wii U comes much easier.

## The Wii U Environment
Running code on the Wii U can be acheived through several methods.
 - **Browserhax** exploits the Internet Browser to run code. Binaries are loaded into Javascript arrays or MP4 files (depending on the target firmware version) which are then ran by the exploit. Due to the Wii U's security model, the code has the permissions of the Internet Browser; which is quite limited. The method of launching the code also crashes the browser, leaving memory and system libraries in "limbo"; rendering them unusable. An ELF loader written for the exploit works around the inherent filesize limitations, but other oddities (such as not loading all the ELF, breaking C global variables) make browserhax a far from ideal environment. Luckily, on firmwares with a kernel exploit, better homebrew launching methods are available.
 - **The Homebrew Launcher** is a program (requiring a kernel exploit) that injects itself into Mii Maker. Mii Maker has far more permissions than the Internet Browser (such as SD card access). Since homebrew code is executed before Mii Maker even starts (as if the main() function was replaced) the environment is much cleaner since all memory and system libraries are in their initial states; however some Wii U features (such as the home menu overlay and certain library functions) are still unsupported. Homebrew code (as ELF files) is loaded from the SD card (or network) into a special memory range (0x00080000) and executed. The Homebrew Launcher is currently the best method of running homebrew code for development (due to its network features).
 - **Loadiine GX2**, suprisingly, can also run homebrew code. A special toolchain (wut) can be used to compile homebrew RPX files that can be ran with Loadiine. This method of launching is best for future-proofing since the RPX is exactly the same as a retail game, thus allowing installation to the home menu at the release of an IOSU exploit that allows non-Nintendo RPXs to be installed. Since homebrew is identical to a retail game in this situation, features such as the home menu overlay are fully supported. However, there's currently no way to load RPX files into Loadiine over the network, which is annoying for development.

*Interestingly, in theory you could also sign wut RPX files; allowing shenanigans such as burning to a disc or even submission to the eShop. Nintendo will probably reject it; however, and there's no obvious and easy way to sign code.*

## The Wii U's Security Model
The Wii U has an interesting security system. Two processors work together to make the system we have to deal with; the PowerPC and the ARM (aka IOSU).

The PowerPC is where pretty much all the code is ran; games, homebrew, most of the OS, etc. Other than the main program (aka "userspace", although this term is often (incorrectly) applied to browserhax as well) there's also a kernel which handles interfacing the IOSU and other system stuff. It also has access to all memory (while the main program is subject to the usual permissions system). The kernel exploit allows us to run custom code in the kernel; which we use to leave us a "backdoor" so the main program can also access all memory.

The IOSU is a seperate ARM processor that acts as a gatekeeper between the PowerPC and the Wii U's hardware. It enforces a permissions system where it applies restrictions based on what program is running on the PowerPC. It's no powerhouse but it does a rather good job; stopping stuff like USB from being accessible. It also checks the signatures of programs running on the PowerPC, which is why we can't just install homebrew. It has a userspace and a kernel; just like the PowerPC.

###### todo: compilers/languages + everything else

***Chapter 1: That's all folks!***
Head over to Chapter 2 [here](/tutorial/Chapter%202.md).
