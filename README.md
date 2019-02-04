# [MSI GS65 Stealth Thin 8RF][laptop]

LAST UPDATED: **2019-02-04**


## Configuration
### bootloader

Clover-v2.4k-4871-X64

### kexts

[ACPIBatteryManager.kext][battery] (1.90.1)  
[AirportBrcmFixup][wifi] (1.1.9)  
[AppleALC][sound] (1.3.4 **+custom verbs**)  
[AtherosE2200Ethernet][ethernet] (2.2.2)  
[BrcmFirmwareRepo][bluetooth] (2.2.10)  
[BrcmPatchRAM2][bluetooth] (2.2.10)  
[BT4LEContiunityFixup][bt4le] (1.1.2)  
[FakeSMC][smc] (6.26-357-gceb835ea.1800)  
[Lilu][lilu] (1.3.1)  
USBxhci (1.0.0 **=custom map for MSI GS65**)  
[VoodooPS2Controller][ps2] (1.9.2 **+custom map**)  
[WhateverGreen][graphics] (1.2.6)  

## Status

#### problem: unable to boot partition
**fix:** injector (FSInject-64.efi) + adapter file
adapter: ApfsDriverLoader-64.efi (APFS)
adapter: HFSPlus-64.efi (HFS+)

#### problem: couldn't allocate runtime area
~~**fix:** AptioMemoryFix-64.efi (can't use for now as system doesn't shutdown/reboot)~~
**fix:** OsxAptioFixDrv-64.efi

#### problem: requested memory exceeds allocated relocation block
**fix:** CsrActiveConfig **Allow Unrestricted NVRAM** (NVRAM Protections: disabled)

#### problem: boot freeze
**fix:** DSDT drop table **DMAR** if you need VT-d enabled

#### problem: unable to boot (SMC mandatory)
**fix:** FakeSMC.kext

#### problem: waiting for root or usb issues
**fix:** USBxhci.kext

#### problem: USB-C
**fix:** SSDT-TYPC.aml + DSDT patches: "SSDT-TYPC"

#### problem: graphics
**fix:** WhateverGreen.kext + Lilu.kext

#### problem: nvidia dgpu (currently not supported on OS X 10.14)
**fix:** SSDT-DDGPU.aml

#### problem: display brightness
**fix:** SSDT-PNLF.aml

#### problem: sound
**fix:** AppleALC.kext + Lilu.kext

#### problem: battery
**fix:** ACPIBatteryManager.kext

#### problem: touchpad/keyboard
**fix:** VoodooPS2Controller.kext

#### problem: ethernet
**fix:** AtherosE2200Ethernet.kext

#### problem: wifi
**fix:** replace hardware BCM94352Z then fix wifi (AirportBrcmFixup.kext) and bluetooth (BrcmPatchRAM2.kext + BrcmFirmwareRepo.kext + BT4LEContiunityFixup.kext)

#### problem: unable to activate apple services (Facetime, iMessage, etc)
**fix:** use EmuVariableUefi-64.efi just once to activate

## Instructions

### boot options
* integrated graphics (config.plist)
* optimus graphics (config.dgpu.plist - waiting for compatible nvidia drivers)

### help
* kexts should be injected manually until install is completed or add **SystemParameters: InjectKexts: Detect** in config 
* kexts should be installed to /Library/Extensions/
* [disable power related options][disable-slow-sleep]:
 - sudo pmset -a disksleep 0
 - sudo pmset -a powernap 0
 - sudo pmset -a womp 0
 - sudo pmset -a standby 0
 - sudo pmset -a hibernatemode 0
   - sudo rm /var/vm/sleepimage
   - sudo mkdir /var/vm/sleepimage
 - sudo pmset -a autopoweroff 0
* using optimus requires mac OS compatible version of nvidia drivers to be installed and starting with **config.dgpu.plist** (drop table 'SSDT-DDGPU.aml' in Clover GUI as DisabledAML doesn't seem to work for now)
* disable touchpad when mouse is present by checking **Ignore built-in trackpad...** in **System Preferences: Accessibility**
* after install, set **Timeout: 0** in config.plist for fast boot
* if running Windows as dual boot, sync time with [RealTimeIsUniversal]

### resources:
- clover forum: [insanelymac]
- release of latest clover build: [clobber]
- external monitor brightness and volume control: [MonitorControl]
- subsystem packages management: [brew]
- [quick look plugins][qlplugins]

kudos to all community developers who share their knowledge
special thanks to all and every member of [clover bootloader team][clover], [Rehabman], [vit9696].

[battery]: https://bitbucket.org/RehabMan/os-x-acpi-battery-driver/downloads/
[bluetooth]: https://bitbucket.org/RehabMan/os-x-brcmpatchram/downloads/
[brew]: https://brew.sh
[bt4le]: https://github.com/acidanthera/BT4LEContiunityFixup/releases
[clobber]: https://github.com/Dids/clover-builder/releases
[clover]: https://www.insanelymac.com/forum/topic/304530-clover-change-explanations/
[disable-slow-sleep]: https://www.tonymacx86.com/threads/slow-sleep-times.145939/#post-902481
[ethernet]: https://www.insanelymac.com/forum/files/file/313-atherose2200ethernet/
[graphics]: https://github.com/acidanthera/WhateverGreen/releases
[insanelymac]: https://www.insanelymac.com/forum/327-clover/
[laptop]: https://www.msi.com/Laptop/GS65-Stealth-Thin-8RF
[lilu]: https://github.com/acidanthera/Lilu/releases
[MonitorControl]:  https://github.com/the0neyouseek/MonitorControl/releases
[ps2]: https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/
[qlplugins]: https://github.com/sindresorhus/quick-look-plugins
[RealTimeIsUniversal]: https://superuser.com/questions/482860/does-windows-8-support-utc-as-bios-time
[Rehabman]: https://bitbucket.org/RehabMan/
[smc]: https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads/
[sound]: https://github.com/acidanthera/AppleALC/releases
[vit9696]: https://github.com/acidanthera
[wifi]: https://github.com/acidanthera/AirportBrcmFixup/releases
