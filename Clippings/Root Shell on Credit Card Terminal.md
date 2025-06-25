---
title: "Root Shell on Credit Card Terminal"
source: "https://stefan-gloor.ch/yomani-hack"
author:
published:
created: 2025-06-21
description:
tags:
  - "clippings"
---
![Voip phone](https://stefan-gloor.ch/img/yomani.jpg)

In this project, I started to reverse engineer payment card terminals because they seemed to be an interesting target for security research, given the high stakes involved. Although I initially didn’t know much about this industry, I did expect a ton of security features and a very security-hardened device. And to some degree, this was also correct.

## First Look

The model I went with is a Worldline Yomani XR terminal. Although it seems to be discontinued at the time of writing, this is the model that is *everywhere* in Switzerland. From big grocery chains to the small repair shop on the corner, everyone has one or a whole fleet of this exact terminal.

After booting it up and aimlessly clicking through the UI, I did a quick port scan, but couldn’t find anything interesting. So naturally, I started to take it apart.

![Picture of the main PCB of the Yomani XR terminal with annotation of different board components.](https://stefan-gloor.ch/img/yomani_pcb.jpg)

Picture of the main PCB of the Yomani XR terminal with annotation of different board components.

The housing and the PCBs appear to be well-made. The design consists of multiple PCBs: a small connector board for the outward-facing connectors, the main board, and a vertical board the card slot sits on. The main SoC seems to be a custom ASIC, a dual-core Arm processor code-named “Samoa II” in the firmware, but I am jumping ahead. According to Worldline documentation, this seems to indeed be a custom ASIC, rather than just a rebranded off-the-shelf chip. Next to it, there is a small external flash and RAM.

## Tamper Protections

During disassembly, I kept looking for a tamper switch that would detect when the device’s housing was opened, like I had seen previously on laptops and other devices. However, I couldn’t find such a switch. Rather, they use the board-to-board interconnects as a way of detecting when the device is opened. Because they use relatively pressure-sensitive Zebra strips between the boards, they have to be tightly screwed together. Even unscrewing some of the screws is enough to break contact and trigger a tamper event. Of course, the tamper detection must also work when the power is disconnected, so that’s the purpose of the coin cell battery.

But this is not all in terms of tamper resistance: The “vulnerable” PCBs are covered by zig-zag traces that act as a tamper detection mechanism. Accidentally breaking a single copper trace during a physical penetration attempt (hole drill etc.) is enough to trigger the tamper detection.

![PCB showing a zig-zag line pattern across the whole board, acting as a temper detection mechanism.](https://stefan-gloor.ch/img/yomani_mesh2.jpg)

Trace meander on the display PCB acting as tamper detection.

The card slot itself is contained in an additional, internal housing. Wrapped around this housing, there is a flex PCB acting as tamper protection.

![Inner plastic housing is wrapped in a flex PCB with zig-zag traces, acting as a temper detection mechanism.](https://stefan-gloor.ch/img/yomani_mesh.jpg)

Card reader assembly wrapped in a tamper detection flex PCB.

After putting the device back together, it was clear that my intrusion had not gone undetected. Sure enough, the device would now only show a big red screen screaming “TAMPER DETECTED”. In this mode, the device seems to be fully unresponsive to any kind of external input. So, game over?

![Yomani Credit Card terminal showing an all-red screen saying OUT OF ORDER, TAMPERED STATE, no key loaded.](https://stefan-gloor.ch/img/yomani_tampered.jpg)

Yomani Credit Card terminal showing an all-red screen saying OUT OF ORDER, TAMPERED STATE, no key loaded.

## Chip-Off Firmware Extraction

Since any “runtime” exploration seems to be futile now, I wanted to take a look a the firmware. For this, I desoldered the on-board flash chip, soldered some wires to it in “dead bug” style and proceeded to dump the contents.

![Flash chip in BGA housing mounted upside-down on a perfboard. Thin copper wires are connected from the chip’s contacts to a flash reader.](https://stefan-gloor.ch/img/yomani_chipoff.jpg)

BGA flash chip of the card terminal desoldered and connected to a flash reader.

To my surprise, the contents seemed to be entirely unencrypted! However, the contents seemed to have a strange ECC layout. Instead of following the flash chip’s inherent page layout and putting the ECC data in the interleaved spare areas, it appeared as if the some ECC data was present in the page and some clear text was in the spare areas. With the help of some friends, I figured out the layout: instead of using the standard 2048 byte payload + 64 byte ECC/spare layout, it uses 3x 694 byte chunks of data each followed by 10 ECC bytes. The ECC bytes do not appear to be all used. Instead, the last 16 bytes of the spare area seem to act as metadata for the YAFFS2 file system. Now normally, there are less ECC bytes per page and therefore the usable metadata area for the filesystem is larger than 16 bytes. For this reason, the YAFFS2 filesystem had to be patched to work with smaller metadata structures.

I implemented a compatible filesystem reader and with it, I could successfully dump the filesystem contents!

[View yomani-unpacker on Github](https://github.com/stgloorious/yomani-unpacker)

Now it was clear that this device runs Linux. I found a standard Linux filesystem with a lot of interesting files to browse through. The system runs a 3.6 kernel, built with Buildroot 2010.02 (!) in February of 2023. The system seems to use a custom bootloader, “Booter v1.7”. Although I don’t know how recent the firware version was that I ended up dumping, it must have been released after February 2023. So finding such an ancient kernel is rather concerning. Userspace-wise, the system uses classy init scripts, busybox, and uClibc (final release was 13 years ago). libcrypt has version 0.9.26, ouch.

## Finding a Root Shell by Accident

Having seen the firmware gave me some new confidence that there might be more stuff to find. So I reattached the flash chip using wires, ignoring everything I know about signal integrity. To my surprise the device booted up again (showing the tamper message).

![BGA flash chip attached to PCB using wires](https://stefan-gloor.ch/img/yomani_flash_attached.jpg)

Flash chip reunited with the PCB, as good as new.

My next goal was to find the serial console of the Linux system. Surely, it must have one for debugging. By looking at a boot log, I might get some more valuable information. So, equipped with a logic analyzer, I started poking.

![PCB with needle probes attached to a logic analyzer](https://stefan-gloor.ch/img/yomani_probing.jpg)

Probing the debug connector.

It didn’t take long until I found some activity on one pad of the unpopulated debug connector. Bingo!

```
------------------------------------------------------------------------
Booter: 1.7b+00002:gbe6b338 Jun  3 2014 08:51:58 owi
Reset reason: Tamper
Start USB boot ...
Got address 0x00000015
Enumerated.
Dfu timeout
yaffs: checkpoint restore ... KO!
yaffs: clean up the mess caused by an aborted checkpoint
file "hwinfo-l0" found
file "hwinfo-l1" found
file "hwinfo-l2" found
file "loadercode" found
file "mp1.img" found
file "linux" found
Uncompressing Linux... done, booting the kernel.
Linux version 3.6.0-samoa-01844-g1f05798 (ppd@debian) (gcc version 4.3.4 (Buildroot 2010.02) ) #0 Fri, 10 Feb 2023 16:26:07 +0100
cpufreq: initial frequency: 264000 kHz
MAC-1G DMA: 430ab000 - 430ab9ff
MAC addr = 00:08:19:4e:56:2c
eth0: ioaddr: d00d0000, dev: c30ac000
probing samoafb: rc = 0
  DMA = 4F500000->c3a00000, IO =   (null)
UART 1 probing
UART 1 probing OK
UART 2 probing
UART 2 probing OK
UART 3 probing
UART 3 probing OK
pca953x 0-0049: failed reading register
ba315 ba315.0: NAND ID: id[0-3]=’EF A1 00 95’, manu=’Winbond’, dev=’NAND 128MiB 1,8V 8-bit’
drivers/rtc/hctosys.c: unable to open rtc device (rtc0)
yaffs: Attempting MTD mount of 31.0,"mtdblock0"
yaffs: yaffs_read_super: is_checkpointed 0
starting pid 398, tty ’’: ’/etc/init.d/rcS’
/etc/init.d/rcS started
Mounting local file systems: ok
rootfs on / type rootfs (rw)
/dev/root on / type yaffs2 (rw,relatime)
proc on /proc type proc (rw,relatime)
devpts on /dev/pts type devpts (rw,relatime,mode=600)
tmpfs on /tmp type tmpfs (rw,relatime)
none on /sys type sysfs (rw,relatime)
Mounting usbfs: failed
Terminal is initializing\nPlease wait
...
dropbear is not present
emv-engine-4.3e-with-trace is not present
tim-server-nexo-config_3.1.6.0-1140_arm is not present
mp1-samoa2-eft-sr-prod is not present
Checking for FW updates\nPlease wait

 FW is up to date.

Starting Application Monitoring Daemons
Mounting USB stick: failed
starting pid 600, tty ’/dev/ttyCU’: ’/sbin/getty 115200 ttyCU’

samoa login:
```

It even shows a login prompt, neat! So this is definitely the Linux boot log. Many embedded Linux systems will have such a more or less exposed serial console, but most of the time the login is disabled altogether or a random, hard-to-crack password is either hard-coded or generated at boot. Not expecting much, just out of curiosity, I entered “root” as the login, and...

```
samoa login: root
~ #
```

Wait, what!? That’s it? I’m in?

Well, yes. That was the boring story of how I got the root shell. It is just there, exposed. No sophisticated exploit chain, no brute-force password cracking required. Additionally, everything seems to still work although the device still shows the tamper message.

Now you may say: well okay, that’s not ideal, but in order to get access to the exposed root shell, you need to open up the device, which triggers the tamper protection and effectively renders the device useless. But in fact, the **serial port is accessible from the outside!**. Without opening up the device and triggering the tamper protection, the debug connector can be accessed through a little hatch on the back of the device. All an attacker needs is just 30 seconds alone with the device to attach to the serial port, log in, deploy malware, and leave. This sounds really bad.

![PCB showing 3 wires attached with labels, RX, TX, and GND.](https://stefan-gloor.ch/img/yomani_uart_conn.jpg)

The debug connector with the serial console is accessible from the outside of the device.

## Is This as Bad as It Looks?

![Two-panel meme of freight train hitting a school bus. Bus is standing on the tracks labelled ’extensive hardware tamper protection features’, in the second panel, freight train labelled ’exposed serial port with root shell’ rams the bus away.](https://stefan-gloor.ch/img/yomani_meme.jpg)

All this fancy hardware made useless by some lazy software engineers?

Actually, no. Hear me out: During my analysis of this device, it became clear that the Linux system is not the whole story. For example, there seems to be no graphics driver for the display and the only framebuffer device does not seem to do anything. Instead, only text strings seem to be passed to a binary (`display_tool`), that issues some inter-processor messages. The same goes for the key pad or the card reader itself. I could not find any evidence that these peripherals could be accessed directly from Linux.

Instead, there is an entirely separate processor, refered to as `mp1`, that seems to handle all the “secure” stuff, like handling the card, getting the pin and showing information on the screen. The “insecure” Linux, running on the second processor, `mp2`, only handles the networking, the updating, and the business logic.

The Linux core seems to always boot, regardless of the tamper state. From there, the secure core is booted. For this, Linux apparently loads a secure bootloader (named `loadercode`) into memory. This loadercode checks whether the tamper protections have been triggered and based on the result, either show the red screen or continue to boot the actual “secure” image (`mp1.img`, in the Linux filesystem). This secure image seems to be properly encrypted and signed by two entities.

![Diagram showing the presumed boot process of the Yomani device. Linux always boots on mp2, which is the insecure application core. After that, it will take loadercode and mp1.img from the filesystem and boot it on the other, secure core.](https://stefan-gloor.ch/img/yomani_diagram.jpg)

My current understanding of the Yomani boot process featuring the Samoa II ASIC. The first core (insecure, application) always boots Linux, which in turn loads a secure bootloader and the secure image on the second core. The loadercode is what displays the tamper messages on screen. The secure image, that handles the card, display, and key pad, is properly encrypted and signed.

## Disclosure Timeline

|  |  |
| --- | --- |
| 14/11/2024 | Discovered the root shell |
| 15/11/2024 | Informed manufacturer, announced 90 days until publication |
| 18/11/2024 | Manufacturer confirms that they got the report |
|  | \--- I forgot this project existed --- |
| 01/06/2025 | Publication |

## Conclusion

While concerning, the exposed root shell does not seem to be as big of a risk as initially feared. While still being a huge unnecessary attack surface, and a massive oversight from the engineers in my opinion, I could not find any evidence that sensitive data, such as card details, could become compromised this way. Also, I could not definitively prove which firmware versions were vulnerable. During my research, I also encountered devices that had a disabled root login. My guess is that somehow this debug feature accidentally made it into production firmware at some point. Chances are that this was discovered and fixed internally before I reported it to the manufacturer (as I had no way of updating the firmware, or verifying that I was running the latest firmware).

This project was a lot of fun and I wish I had some more time to dig deeper into the firmware and explore more possibilities. If you want to take on this challenge, feel free to reach out to me. I’d like to thank anyone involved in this project for their valuable input.