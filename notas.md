# NOTAS 


### Qualcomm MSM (Mobile Station Modem)
MSM (Mobile Station Modem) is a series of 2G/3G/4G-capable system on chips designed by Qualcomm since the early 1990s to date.

Son los procesadores.

Qualcomm MSM based devices contain a special mode of operation, called Emergency Download Mode (EDL). In this mode, the device identifies itself as Qualcomm HS-USB 9008 through USB, and can communicate with a PC host. EDL is implemented by the SoC ROM code (also called PBL). The EDL mode itself implements the Qualcomm Sahara protocol, which accepts an OEM-digitally-signed programmer over USB. The programmer implements the Firehose protocol which allows the host PC to send commands to write into the onboard storage (eMMC, UFS).

As open source tool (for Linux) that implements the Qualcomm Sahara and Firehose protocols has been developed by Linaro, and can be used for program (or unbrick) MSM based devices, such as Dragonboard 410c or Dragonboard 820c.

### Qualcomm EDL (Emergency Download Mode)
https://alephsecurity.com/2018/01/22/qualcomm-edl-1/

There are many guides [1,2,3,4,5,6,7] across the Internet for ‘unbricking’ Qualcomm-based mobile devices. All of these guides make use of Emergency Download Mode (EDL), an alternate boot-mode of the Qualcomm Boot ROM (Primary Bootloader).

To make any use of this mode, users must get hold of OEM-signed programmers, which seem to be publicly available for various such devices. While the reason of their public availability is unknown, our best guess is that these programmers are often leaked from OEM device repair labs. Some OEMs (e.g. Xiaomi) also publish them on their official forums.

Qualcomm Secure Boot
An abstract overview of the boot process of Qualcomm MSM devices is as follows:

```
[Primary Bootloader (PBL)]
|
`---NORMAL BOOT---.
                  [Secondary Bootloader (SBL)]
                  |-. 
                  | [Android Bootloader (ABOOT)]
                  | `-.    
                  |   [boot.img]
                  |   |-- Linux Kernel
                  |   `-- initramfs
                  |       `-.
                  |         [system.img]
                  |
                  `-[TrustZone]
```
#### Primary Bootloader (PBL)
Primary or Single Bootloader (PBL): Primary Bootloader (PBL) is installed in the ROM and is the first block to execute on boot reset. The main function of the Primary Bootloader is to download the Secondary Bootloader in RAM and activate the SBL.


#### Secondary Bootloader (SBL)
Secondary Bootloader (SBL) is needed in order to initialize the execution environment for
running the application. This includes setting up the clocks, powering on the I/O peripherals,
initializing the secondary memory i.e. DDR, loading application images and booting slave cores.
Advanced Driver Assist Systems (ADAS) have very specific boot requirements regarding boot
time and functional safety. 