# Hackintosh on ASUS ROG Strix Z790-A

## Introduction
This repository documents my experience installing macOS Sequoia on the ASUS ROG Strix Z790-A motherboard. It covers hardware configuration, OpenCore settings, troubleshooting, and optimizations to achieve a stable system.

## Hardware Specifications

For detailed hardware information, please refer to [`Hardware.md`](./Hardware.md).

### Key Components
- **Motherboard**: ASUS ROG Strix Z790-A WIFI D4
- **CPU**: Intel Core i9-14900K (iGPU disabled)
- **GPU**: AMD Radeon RX6800XT
- **Wi-Fi/Bluetooth**: BCM943602CDP (replacing Intel Wi-Fi)
- **Storage**:
  - **WD Black SN850X 2TB** as OpenCore + macOS Sequoia installation
  - **Samsung 970 EVO Plus 1TB** as Windows 11
  - **Samsung 970 EVO 1TB** as Data Storage

## Installation Steps

### Prepare OpenCore Bootloader

- **Install Windows 11**  
  Disconnect all other drives with existing EFI partitions during the Windows installation process. This prevents Windows from placing its boot manager on the wrong disk. After installation, reconnect the drives and verify the boot priority in your BIOS.

- **Perform Essential Preparations on Windows 11**  
  Use **SSDTTime** to dump the ACPI tables for your system and generate all necessary ACPI patch files for OpenCore, such as those for CPU power management or disabling unsupported features.  
  Use **USBToolbox** to map USB ports and create a `USBMap.kext`. You can manually edit the resulting file to disable unused ports and ensure compliance with macOS's USB port limit.

- **Create a macOS USB Installer**  
  Download the macOS installer app and use Apple’s `createinstallmedia` command to create a bootable USB installer. If the USB fails to boot, reformat the drive and try again with a reliable USB stick.

- **Download and Configure OpenCore**  
  Obtain the latest OpenCore release and set up the `EFI` folder. Configure `config.plist` to include the required ACPI patches, kexts (e.g., Lilu, WhateverGreen, VirtualSMC), and device properties for GPU, Wi-Fi, and other hardware. GPU-related issues, like black screens, may require adjustments in framebuffer settings.

- **Set Up SMBIOS**  
  Use a compatible system definition, such as **MacPro7,1** or **iMacPro1,1**, and generate PlatformInfo with OpenCore Configurator. Set the ROM value based on your motherboard's MAC address to enable iMessage and FaceTime functionality.

- **Adjust BIOS Settings**  
  Configure the following in the BIOS for Hackintosh compatibility:  
  - Secure Boot: Disabled  
  - Boot Mode: UEFI  
  - CFG Lock: Disabled (enable `AppleCpuPmCfgLock` if unable to disable in BIOS)  
  - Above 4G Decoding: Enabled  
  - XHCI Hand-off: Enabled  

### Install macOS Sequoia

- Boot from the macOS USB installer and select the installer from the OpenCore Boot Picker.  
- Use Disk Utility to format the target SSD as **APFS** with a **GUID Partition Map**.  
- Proceed with the macOS installation.  
- After installation, copy the `EFI` folder to the SSD's EFI partition to ensure it can boot without the USB.  
- **Troubleshooting Tips**:  
  If the installer hangs, verify the `config.plist` settings, especially the ACPI patches and kexts.  
  Unexpected reboots during installation may indicate misconfigured BIOS settings or missing ACPI patches.

## Post-Installation, Troubleshooting and Fixes

### USB Issues
- Reduced port count to below 15 to comply with macOS limitations.
- Sleep/Wake may be broken if you connect the motherboard's Type-C port to a monitor (e.g., LG 34WK95U-W) with Thunderbolt/Type-C support due to unknown power source conflict. Use the motherboard's Type-A port or monitor's Type-B port instead.

### NVMe TRIM Compatibility
- Since Monterey, APFS has had compatibility issues with NVMe TRIM, causing long boot times or premature SSD failure.
- Setting `SetApfsTrimTimeout` in OpenCore does not fully resolve the problem.
- The **only** reliable solution is to use an NVMe SSD well-known to be compatible, such as the **WD Black SN850X**.

## Optimization

- If your SMBIOS is set to **MacPro7,1**, you can use `resourceconverter.sh` from CPUFriend to convert the X86PlatformPlugin plist for **iMacPro1,1** into `CPUFriendDataProvider.kext`, ensuring proper peak CPU performance. This is the easiest way.
- Run benchmarks (such as GeekBench6) to compare with Windows 11 CPU/GPU performance scores.

## Conclusion
This Hackintosh setup is now stable and performs well. With precise OpenCore adjustments, the system offers excellent performance and compatibility. If you're planning a similar setup, I hope this documentation helps!

---

### Notes
- This repository **does not include** full EFI with SMBIOS details for security reasons. You must generate your own PlatformInfo.
- Contributions and discussions are welcome! Feel free to open an issue if you encounter similar challenges.
