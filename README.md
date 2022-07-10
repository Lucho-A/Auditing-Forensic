# Auditing-Forensic

***(Under Development)*** The main idea of the repo, is simply provide a set of tools for carrying out a forensic activity in line with the [RFC 3227: "Guidelines for Evidence Collection and Archiving"](https://datatracker.ietf.org/doc/html/rfc3227) 

For this purpose, when OS system particularities arise, I'll cover AIX/Linux, and Windows system, only, and the software referred will be, in all the cases, free ones.

Finally, as we already know, one thing is obtain the information and, other, interpretate it and obtaining particular conclusions. The idea of the repo, is just covering the first part.

## Let's start

According to the RFC, one of the first aspect to take into consideration are: 
- make a bit-level copy of the system's media to be audited.
- do it, taking into consideration the volatility order.

In this regard, I'm going to cover: RAM and HDD, in this order.

### RAM

For performming RAM images, I could recommend:
- Linux/AIX: [fmem](https://github.com/NateBrune/fmem). This is a great software that load a module into the kernel in order to allow the creation of a device (/dev/fmem) that can be used for dumping the memory with, for instance, 'dd' command. Into the README file of the fmem repo, you can find the way of usage.

- Win32/64: [winpmem](https://github.com/Velocidex/WinPmem). Other very simple tool for creating RAM images. Just ">winpmem_mini_x64_rc2 [outputImage.raw]" and that's all.

### HDD
In this case, I recommend just using '[dd](https://man7.org/linux/man-pages/man1/dd.1.html)' command. Remember creating images for the entire filesystem that exists (including USB's devices). If you are running an AIX/Linux machine, for sure, you already have it, but if you are running a Windows system, maybe you have to download it. [Here](http://www.chrysocome.net/downloads/dd-0.5.zip) you can find it.

## Next steps

(to be continue...)
