# Auditing-Forensic

***(Under Development)*** The main idea of the repo is, simply, provide an essential set of tools for carrying out a forensic activity in line with the [RFC 3227: "Guidelines for Evidence Collection and Archiving"](https://datatracker.ietf.org/doc/html/rfc3227) 

For this purpose, when OS system particularities arise, I'll cover AIX/Linux, and Windows system, only, and the software referred will be, in all the cases, free ones.

Finally, as we already know, one thing is obtain the information and, other, interpretate it and obtaining particular conclusions. The idea of the repo is covering, essentially, regardless of technical aspects, the first part. For deeper and more exahustive analysis, in particular, about memory and Volatility framework, I recommend the reading of ["The Art of Memory Forensics"](https://repo.zenk-security.com/Forensic/The%20Art%20of%20Memory%20Forensics%20-%20Detecting%20Malware%20and%20Threats%20in%20Windows,%20Linux,%20and%20Mac%20Memory%20(2014).pdf).

## Let's start

According to the RFC, some of the most important aspects to take into consideration are: 
- make a bit-level copy of the system's media to be audited.
- do it, taking into consideration the volatility order.

In this regard, I'll start covering: RAM and HDD/USB, in this order.

### Image creation

Referred to this topic, is for pointing out that is recommended not to save the images created into the same system and, as good practice, save the images in a different partition than the system under revision, and system that are you using for performing the analysis. For instance: if you are analyzing a plausible malware infected windows system, from a Kali USB device, it's recommendable store the images in a third, let' say, USB device that operate like a sand box.

#### "RAM"

For performming RAM images, I could recommend:
- Linux/AIX: [fmem](https://github.com/NateBrune/fmem). This is a great software that load a module into the kernel in order to allow the creation of a device (/dev/fmem) that can be used for dumping the memory with, for instance, 'dd' command. Into the README file of the fmem repo, you can find, very well descripted, the way of usage.

- Win32/64: [winpmem](https://github.com/Velocidex/WinPmem). Other very simple tool for creating RAM images. Just "> winpmem_mini_x64_rc2 [path_to_outputImage.raw]" and that's all.

#### "HDD/USB"
In this case, I recommend just using '[dd](https://man7.org/linux/man-pages/man1/dd.1.html)' command. Remember creating images for the entire filesystem involved (including USB's devices). If you are running an AIX/Linux machine, for sure, you already have it, but if you are running a Windows system, maybe you have to download it. [Here](http://www.chrysocome.net/downloads/dd-0.5.zip) you can find it. 

Example of usage for an USB device:
- Linux/AIX<sup>1</sup>: # dd.exe if=/dev/sdc1 of="path_to_outputImage.dd" bs=4M.
- Win32/64: > dd.exe if=\\.\G: of="path_to_outputImage.dd" bs=4M.

###### <sup>1</sup> remember, you can find the USB devices loaded by using the 'lsusb' command.

## Next steps

(to be continue...)
