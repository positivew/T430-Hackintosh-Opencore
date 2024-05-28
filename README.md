# Thinkpad T430 - Hackintosh - OpenCore
[![T430](https://img.shields.io/badge/ThinkPad-T430-blueviolet.svg)](https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_T430.pdf)
[![OC](https://img.shields.io/badge/OpenCore-1.0.0-informational.svg)](https://github.com/acidanthera/OpenCorePkg/releases/tag/1.0.0)
[![10.15](https://img.shields.io/badge/macOS-10.15-9cf.svg)]()
[![11](https://img.shields.io/badge/macOS-11-red.svg)]()
[![12](https://img.shields.io/badge/macOS-12-blueviolet.svg)]()
[![download](https://img.shields.io/badge/Download-latest-success.svg)](https://github.com/positivew/T430-Hackintosh-Opencore/releases/latest)
<img align="left" src="/resources/T430-new.png" alt="Lenovo Thinkpad T430" width="300">
<img align="right" src="/resources/homepage.png" alt="Opencore" width="200">
<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
## SUMMARY 
The guide is forked from [jozews321's guide](https://github.com/jozews321/T430-Hackintosh-Opencore), but has been tweaked based on my experience with it and the EFI folder has been added to the repository. I tried installing macOS 13 and 14 as well, but those did not work. The original guide suggested using MacBookPro10,1 SMBIOS, but with that I couldn't go higher than macOS 10.15, so I'm using MacBookPro12,1. OpenCore has also been updated to 1.0.0.

|:warning: This guide assumes prior knowledge on how to do basic Hackintosh stuff |
|:--------------------------------------------------------------------|
If you are new to this, it is suggested to read [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) first.
There will be references to the linked guide throughout this process for those that are new to this.

## REQUIREMENTS
- Lenovo Thinkpad T430
- Intel Core i3 3110M or better (Celeron and Pentium CPUs are not supported)
- Integrated Intel HD4000 graphics (NVIDIA discrete GPUs are not supported)
- At least 4GB RAM 
- Stock Intel Centrino WiFI card (optional)
- Internal Broadcom BCM20702 Bluetooth 4.2 card (optional)
- Integrated Fingerprint reader and WWAN card are not supported
- SSD
- USB drive (at least 16gb for full installer or 2gb for internet recovery)

## ABOUT
The provided ACPI tables in this OC EFI folder are made using a DSDT-less method via hotpatching to increase compatibility with different BIOS versions.

## HARDWARE TESTED
### ThinkPad T430 Specs 
| Component           | Details                                       |
| ------------------: | :-------------------------------------------- |
| Model               | Lenovo ThinkPad T430                          |
| BIOS Version        | 2.82                   |
| Processor           | Intel Core i5 3320M                          |
| Memory              | 8GB DDR3 2133MHz in Dual-Channel             |
| SSD                 | SSD 120GB                    |
| Graphics            | Intel HD Graphics 4000                        |
| Display             | 14.0" 1600x900                                |
| Audio               | Realtek ALC269VC                              |
| Ethernet            | Intel 82579LM Gigabit Network                 |
| WIFI                | Intel Centrino Advanced-N 6235              |
| Bluetooth           | Integrated Broadcom BCM20702 Bluetooth 4.2    |


## INSTALL GUIDE
<details>
<summary><strong>Tools required</strong></summary>
<br />

* [Python](https://www.python.org/downloads/)
* [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS/)
* [MountEFI](https://github.com/corpnewt/MountEFI)
* [Propertree](https://github.com/corpnewt/ProperTree)

</details>
<details>
<summary><strong>Getting the EFI ready</strong></summary>
<br />

Download the [latest release](https://github.com/positivew/T430-Hackintosh-Opencore/releases/latest), extract it and set aside the `EFI` folder within.
	
</details>
<details>
<summary><strong>BIOS Settings</strong></summary>

### BIOS Settings
Latest BIOS Version: `2.82` stock

**CONFIG TAB**

* USB UEFI BIOS Support: `Enabled`
* USB 3.0 Mode: `Enabled`
* Display > Boot Display Device: `ThinkPad LCD`
* SATA > SATA Controller Mode: `AHCI`
* CPU > Core Multi-Processing: `Enabled`
* CPU > Intel (R) Hyper-Threading: `Enabled`

**SECURITY TAB**

* Security Chip: `Disabled`
* UEFI BIOS Update Options > Flash BIOS Updating by End-Users: `Enabled`
* UEFI BIOS Update Options > Secure Rollback Prevention: `Enabled`
* Memory Protection: `Enabled`
* Virtualization > Intel (R) Virtualization Technology: `Enabled` 
* I/O Port Access (`Disable` the following):
	* Wireless WAN
	* ExpressCard Slot
	* eSATA Port
	* Fingerprint Reader
* Antitheft: `Disabled`
* Computrace: `Disabled`
* Secure Boot: `Disabled`

**STARTUP TAB**

* Boot (HDD/SSD as first device)
* UEFI/Legacy Boot: `UEFI only`
* CSM Support: `Disabled`
* Boot Mode: `Quick`

</details>

<details>
<summary><strong>USB installation media</strong></summary>

### Creating the USB installer 
In this step we will create macOS installation media.

Go to [OpenCore Guide - Creating the USB](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) where you can find the step by step instructions to create the installation media with your respective OS. You should end up with a USB drive that has been formatted to FAT32 and contains a folder called `com.apple.recovery.boot` with a `.chunklist` and a `.dmg` file in it.
</details>

<details>
<summary><strong>Adding the EFI folder to the USB</strong></summary>
<br />

Copy the EFI folder to the root of your USB drive in order to boot from it. You can also unzip and copy the previously downloaded tools to the USB drive, so you can easily use them right after you have the system up and running.

</details> 

<details>
<summary><strong>Installing macOS</strong></summary>
<br /> 
Boot from the USB by pressing F12 on the Thinkpad BIOS and choose your USB.

- You will see the OpenCore Boot Picker: press Space and choose the `OpenCore (dmg)` option to boot from your installation media.

- After that select Disk Utility and format your SSD in APFS (or HFS+ for macOS 10.15).

- If running the internet installer connect an ethernet cable or connect to WiFi.
	
- Install as normal.
	
You can consult [OpenCore Guide - Installation Process](https://dortania.github.io/OpenCore-Install-Guide/installation/installation-process.html) to get some instructions if you need them.
</details> 

## POST INSTALL
Follow the next steps carefully to get a fully working system.

<details>
<summary><strong>Generating SMBIOS serial</strong></summary>
<br />
	
Now it's time to generate the Serial, MLB, UUID and ROM to the config.plist.

- Open <path to USB drive>/EFI/OC/config.plist with ProperTree
  <br /> <br /> 
- Open GenSMBIOS
  <br /> <br /> 
- Choose 1 to install MacSerial
  <br /> <br /> 
- Choose 3 to generate some new serials
  <br /> <br /> 
- Write MacBookPro12,1
  <br /> <br /> 
- You will get something like this:
  <br /> <br />  
<img src="/resources/gensmbios.png" width="600">
  <br /> <br />   
- Add the generated serials in the config.plist at /PlatformInfo/Generic
  (Serial to SystemSerialNumber, Board Serial to MLB, SmUUID to SystemUUID, AppleRom to ROM)
<img src="/resources/configsmbios.png" width="600">
   <br /> <br /> 
- Save and close ProperTree

</details>

<details>
<summary><strong>Boot without USB</strong></summary>
<br /> 

* Run MountEFI
* Choose your macOS drive and it should be mounted in Finder 
* Copy your EFI folder to the root of the EFI partition on your macOS drive
* Reboot and disconnect your USB drive	
* Boot from disk
	
</details> 

<details>
<summary><strong>macOS 12 - OpenCore Legacy Patcher</strong></summary>
<br /> 
Apple dropped the HD 4000 iGPU with macOS 12. If you dont install this you won't have any kind of graphics acceleration and your macOS 12+ experience will be completely miserable.

* Download [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases)
* Run OLCP
* Choose Post Install Root Patch and follow instructions
* Reboot
	
After successful patching, you will be able to control the display brightness and enjoy fully Metal accelerated UI.
</details> 

<details>
<summary><strong>Optimizing CPU power management</strong></summary>
<br /> 
Follow this guide if you encounter any performance problems (CPU not using higher than base clock):
https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management

	
</details> 

## Credits

Thanks to:

* [Apple](https://www.apple.com) (macOS)
* [Acidanthera](https://github.com/acidanthera) (OpenCore, VirtualSMC, Lilu, WhateverGreen and a lot more)
* [Dortania](https://dortania.github.io) (Opencore Install Guide, Opencore Legacy Patcher)
* [jozews321](https://github.com/jozews321/T430-Hackintosh-Opencore)
* [OpenIntelWireless](https://github.com/OpenIntelWireless/itlwm) (Airportitlwm)
* [5T33Z0](https://github.com/5T33Z0/Lenovo-T530-Hackinosh-OpenCore) (T530 ACPI fixes)
* [zhen-zen](https://github.com/zhen-zen/YogaSMC) (YogaSMC)











 











