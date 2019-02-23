# [MSI GS65 Stealth Thin 8RF][laptop]

LAST UPDATED: **2019-02-23**


## Configuration
### bootloader

Clover-v2.4k-4884-X64

### kexts

[AirportBrcmFixup][wifi] (1.1.9)  
[AppleALC][sound] (1.3.5 **+custom verbs**)  
[AtherosE2200Ethernet][ethernet] (2.2.2)  
[BrcmFirmwareRepo][bluetooth] (2.2.10)  
[BrcmPatchRAM2][bluetooth] (2.2.10)  
[Lilu][lilu] (1.3.4)  
[SMCBatteryManager.kext][smc] (1.0)  
USBxhci (1.0.0 **=custom map for MSI GS65**)  
[VirtualSMC.kext][smc] (1.0.2)  
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
**fix:** VirtualSMC.kext

#### problem: waiting for root or usb issues
**fix:** USBxhci.kext

#### problem: USB-C
**fix:** SSDT-TYPC.aml + DSDT patches: "SSDT-TYPC"

#### problem: graphics
**fix:** WhateverGreen.kext + Lilu.kext

#### problem: display brightness
**fix:** SSDT-PNLF.aml

#### problem: nvidia dgpu
**fix:** nvidia web drivers not supported on OS X 10.14

#### problem: external monitor usb-c not working
**fix:** in clover GUI, drop table SSDT-DDGPU.aml

#### problem: sound
**fix:** AppleALC.kext + Lilu.kext

#### problem: battery
**fix:** SMCBatteryManager.kext + Lilu.kext

#### problem: touchpad/keyboard
**fix:** VoodooPS2Controller.kext

#### problem: ethernet
**fix:** AtherosE2200Ethernet.kext

#### problem: wifi
**fix:** replace hardware BCM94352Z then fix wifi (AirportBrcmFixup.kext) and bluetooth (BrcmPatchRAM2.kext + BrcmFirmwareRepo.kext)

#### problem: unable to activate apple services (Facetime, iMessage, etc)
**fix:** use EmuVariableUefi-64.efi just once to activate

## Instructions

### help
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
* disable touchpad when mouse is present by checking **Ignore built-in trackpad...** in **System Preferences: Accessibility**
* set **Timeout: 0** in config.plist for fast boot
* sync time in Windows as dual boot with [RealTimeIsUniversal]
* using optimus requires mac OS compatible version of nvidia drivers to be installed
* disabling nvidia dgpu with SSDT-DDGPU.aml (PEGP._OFF) or with boot arg **-wegnoegpu** will lead to usc-c video output failure

### resources:
- clover forum: [insanelymac]
- release of latest clover build: [clobber]
- external monitor brightness and volume control: [MonitorControl]
- subsystem packages management: [brew]
- [quick look plugins][qlplugins]

kudos to all community developers who share their knowledge  
special thanks to all and every member of [clover bootloader team][clover], [Rehabman], [vit9696].

[bluetooth]: https://bitbucket.org/RehabMan/os-x-brcmpatchram/downloads/
[brew]: https://brew.sh
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
[smc]: https://github.com/acidanthera/VirtualSMC/releases
[sound]: https://github.com/acidanthera/AppleALC/releases
[vit9696]: https://github.com/acidanthera
[wifi]: https://github.com/acidanthera/AirportBrcmFixup/releases
