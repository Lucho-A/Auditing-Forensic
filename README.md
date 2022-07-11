# Auditing-Forensic

***(Under Development)*** The main idea of the repo is, simply, provide an essential set of tools for carrying out a forensic activity in line with the [RFC 3227: "Guidelines for Evidence Collection and Archiving"](https://datatracker.ietf.org/doc/html/rfc3227) 

For this purpose, when OS system particularities arise, I'll cover AIX/Linux and Windows system, only, and the software referred will be, in all the cases, free ones.

Finally, as we already know, one thing is obtain the information and, other, interpretate it and obtaining particular conclusions. The idea of the repo is covering, essentially, regardless of technical aspects, the first part. For deeper and more exahustive analysis, in particular, about memory and Volatility framework, I recommend the reading of ["The Art of Memory Forensics"](https://repo.zenk-security.com/Forensic/The%20Art%20of%20Memory%20Forensics%20-%20Detecting%20Malware%20and%20Threats%20in%20Windows,%20Linux,%20and%20Mac%20Memory%20(2014).pdf).

## Let's start

According to the RFC, some of the most important aspects to take into consideration are: 
- make a bit-level copy of the system's media to be audited.
- do it, taking into consideration the volatility order.

In this regard, I'll start covering: RAM and HDD/USB, in this order.

## Image creation

Referred to this topic, is for pointing out that is recommended not to save the images created into the same system. Instead, as good practice, save the images in a different partition than the system under revision, and the system that are you using for performing the analysis. For instance: if you are analyzing a plausible malware infected windows system, from a Kali USB device, it's recommendable store the images in a third, let' say, USB device that operate like a sand box.

### "RAM"

For performming RAM images, I could recommend:

***Linux/AIX***: 
  - [LiME](https://github.com/504ensicsLabs/LiME). Due to its simplicity, and the probed compatibility with Volatility, this is my first option. It's an LKM, and its usage is very well descripted into the repo. For instace:
```
git clone https://github.com/504ensicsLabs/LiME
cd src/
make
sudo insmod lime-X.XX-XX.ko "path=/home/userId/.../.../dumpRAM.lime format=lime"
```
  - [fmem](https://github.com/NateBrune/fmem). This is other great software that load a module into the kernel in order to allow the creation of a device (/dev/fmem) that can be used for dumping the memory with, for instance, 'dd' command. Into the README file of the fmem repo, you can find the way of usage.

Note: if you run the 'dd' command for dumping the memory, take into consideration the use of count parameter. Indeed, if not, as is mentioned in the fmem repo, the dumping could never stop or, even worst, a segmentation fault can arise. So, I recommend you using 1MB as bs, and the sum of the sizes displayed by 'fmem' command after loaded as count. 

***Win32/64***:
  - [winpmem](https://github.com/Velocidex/WinPmem). Other very simple tool for creating RAM images. Just:
```
winpmem_mini_x64_rc2 [path_to_outputImage.raw]
```
... and that's all.

### "HDD/USB"
In this case, I recommend just using '[dd](https://man7.org/linux/man-pages/man1/dd.1.html)' command. Remember creating images for the entire filesystem involved (including different partitions, and USB's devices). If you are running an AIX/Linux machine, for sure, you already have it, but if you are running a Windows system, maybe you have to download it. [Here](http://www.chrysocome.net/downloads/dd-0.5.zip) you can find it. 

Example of usage for an USB device:

***Linux/AIX***<sup>1</sup>:
```
sudo dd.exe if=/dev/sdc1 of=path_to_outputImage.dd bs=4M
```
***Win32/64***:
```
dd.exe if=\\.\G: of=path_to_outputImage.dd bs=4M
```
###### <sup>1</sup> remember, you can find the USB devices loaded by using the 'lsusb' command, and/or the partitions using 'fdisk -l'.

## Next steps. Analizing Data.

Now, we are ready for analyzing the created images. 

### "RAM"

In order to analyze the RAM image, I'll show you the usage of the well-known software [Volatility](https://www.volatilityfoundation.org/). The software can be downloaded from its website. You can find the standalone versions (2.6) for Linux and Windows. However, I encourage you to obtain the latest version cloning the repo:
```
git clone https://github.com/volatilityfoundation/volatility
```
#### Some useful queries
Maybe, the first "query" that you can run, in order to obtain suggestions about the image is:
```
python2 vol.py -f imageRAM.lime imageinfo
```
However, this query takes too long and if you already know the OS associated with the image, maybe you want to skip this query and search for a profile according to the OS, for instance:
```
python2 vol.py --info | grep Linux
```
As you can observe running --info query, that there are generics profiles, in particular, for linux. It's highly recommended to make a particular profile, according to the version of kernel associated to the image. It's very simple, I'll add the way to do that in future versions of the document. However, the generic profiles works pretty well for obtaining useful information.

Now, let's suppose that the image correspond to a linux, so you could use the profile called "LinuxSystemProfilex64".

  - linux_pslist: in order to list the processes, you can use this "plugin" (you can access to the plugin list by running the programm with parameter "--info":
```
python2 vol.py -f imageRAM.lime --profile=LinuxSystemProfilex64 linux_pslist
```
Note: if you run the command, you can see that in the field "Start Time" the UTC used for showing the information. This is very aligned to the RFC 3227 section 2 contents.
  - linux_netstat, for showing opened sockets:
```
python2 vol.py -f imageRAM.lime --profile=LinuxSystemProfilex64 linux_netstat
```
  - linux_ifconfig, for showing active interfaces:
```
python2 vol.py -f imageRAM.lime --profile=LinuxSystemProfilex64 linux_ifconfig
```
  - linux_malfind, for showing suspicious process:
```
python2 vol.py -f imageRAM.lime --profile=LinuxSystemProfilex64 linux_malfind
```
These are just some examples. The list of plugins is very extensive and you can develope your own ones if you want (the web is plenty of site where you can find useful information about it).

### "HDD/USB"
(to be continue...)
