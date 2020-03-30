TODO
- USB Mapping
- Audio
- HDMI with audio
- Sleep
- Keyboard
- Backlight control
- SSDTs: https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/ktext#ssdts
- Embedded Controller (EC): https://1revenger1.gitbook.io/laptop-guide/prepare-install-
macos/embedded-controller-ec
- config.plist DeviceProperties :
  - https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/intel-
config.plist/coffee-lake#deviceproperties
  - https://1revenger1.gitbook.io/laptop-guide/prepare-install-macos/display-
configuration#intel-coffee-lake-comet-lake-14-nm
- ScanPolicy. Initially you want 0 here, but once you are up and running
check Scanpolicy Docs for why you should change
- We set Generic -> ROM to either an Apple ROM (dumped from a real Mac), your NIC MAC
address, or any random MAC address (could be just 6 random bytes, for this guide we'll use
11223300 0000. After install follow the Fixing iServices page on how to find your real MAC
Address)
Reminder that you want either an invalid serial or valid serial numbers but those not in
use, you want to get a message back like: "Invalid Serial" or "Purchase Date not
Validated"
Apple Check Coverage page

A VOIR
- NVMeFix: https://github.com/acidanthera/NVMeFix
- AWAC ou pas: https://khronokernel.github.io/Getting-Started-With-
ACPI/Universal/awac.html
- For users with black screen issues after verbose on B360, B365, H310, H370, Z390, please
see the BusID iGPU patching page
- framebuffer-fbmem: This patches framebuffer memory, and is used when you cannot configure
DVMT to 64MB in the BIOS. Do not use if the DVMT BIOS option is available.
- A voir si XHCI-unsupported.kext nécessaire: https://github.com/RehabMan/OS-X-USB-Inject-
All

CONFIG.PLIST:
AppleCpuPmCfgLock: YES
• Only needed when CFG-Lock can't be disabled in BIOS, Clover counterpart would be
AppleIntelCPUPM. Please verify you can disable CFG-Lock, most systems won't
boot with it on so requiring use of this quirk
AppleXcpmCfgLock: YES
• Only needed when CFG-Lock can't be disabled in BIOS, Clover counterpart would be
KernelPM. Please verify you can disable CFG-Lock, most systems won't boot with
it on so requiring use of this quirk
• DisableIoMapper: YES
• Needed to get around VT-D if either unable to disable in BIOS or needed for other operating
systems, much better alternative to dart=0 as SIP can stay on in Catalina
• XhciPortLimit: YES
• This is actually the 15 port limit patch, don't rely on it as it's not a guaranteed solution for
fixing USB. Please create a USB map when possible.


Intel BIOS settings
Disable:
• Fast Boot
• VT-d(can be enabled if you set DisableIoMapper to YES)
• CSM
• Thunderbolt
• Intel SGX
• Intel Platform Trust
• CFG Lock(MSR 0xE2 write protection)(This must be off, if you can't find the option then
enable both AppleCpuPmCfgLock and AppleXcpmCfgLock under Kernel -> Quirks.
Your hack will not boot with CFG-Lock enabled)
Enable:
• VT-x
• Above 4G decoding
• Hyper-Threading
• Execute Disable Bit
• EHCI/XHCI Hand-off
• OS type: Windows 8.1/10 UEFI Mode
• DVMT Pre-Allocated(iGPU Memory): 64MB

SMBIOS
Type: MacBookPro16,1
Serial: C02C9FY2MD6N
Board Serial: C02008303J9N9PRUE
SmUUID: 4D2642EC-DF68-40AD-B441-6A78262298C4

USB Ports:
- Left: HS05 SS05
- Right Up: HS04 SS04
- Right down: HS03 SS03
- USB-C: SS01
- Bluetooth: HS14 Internal