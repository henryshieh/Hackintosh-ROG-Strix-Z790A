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

### BIOS Settings
Before installing any operating system, configure your BIOS to ensure compatibility for both Windows and macOS:
- **Secure Boot**: Disabled  
- **Boot Mode**: UEFI  
- **CFG Lock**: Disabled (use `AppleCpuPmCfgLock` in OpenCore if CFG Lock cannot be disabled).  
- **Above 4G Decoding**: Enabled  
- **XHCI Hand-off**: Enabled  
Setting these options upfront ensures a smooth installation process for both Windows 11 and macOS.

### Install Windows 11
Install Windows 11 on a dedicated SSD:
- Disconnect all other drives with existing EFI partitions during the installation process to prevent Windows from placing its boot manager on the wrong disk.  
- After installation, reconnect your drives and verify the boot priorities in your BIOS.

### Preparations on Windows 11
While in Windows 11, complete these essential tasks:
- Use **SSDTTime** to dump the original ACPI tables and generate the ACPI patch files required for OpenCore (e.g., for USB, CPU power management).  
- Use **USBToolbox** to map USB ports and generate a `USBMap.kext`. You can manually edit the resulting file to ensure compliance with macOS's 15-port limit.

### Preparations on Another Mac
On a macOS system, perform the following steps:
- Prepare a USB installer for macOS Sequoia using the `createinstallmedia` command. Ensure the USB drive is at least **32GB**, as newer macOS versions like Sonoma and Sequoia require more space than earlier versions.  
- Set up OpenCore by gathering and organizing all necessary files:
  - Core OpenCore files  
  - `config.plist`  
  - ACPI patches  
  - Kexts (e.g., Lilu, WhateverGreen, VirtualSMC)  
- Ensure all required files are placed correctly in the `EFI` folder.

### Configuration Details
Details about configuring `config.plist` (e.g., Device Properties, SMBIOS, kernel settings) are available in a separate file for reference.

### Install macOS Sequoia
- Boot from the macOS USB installer and select the installer from the OpenCore Boot Picker.  
- Use Disk Utility to format the target SSD as **APFS** with a **GUID Partition Map**.  
- Proceed with the macOS installation.  
- After installation, copy the `EFI` folder to the SSD's EFI partition to enable standalone booting.  
- **Troubleshooting Tips**:  
  If the installation hangs, verify the `config.plist` settings, especially ACPI patches and kexts.  
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
