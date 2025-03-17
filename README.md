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
  - **Samsung 970 EVO Plus 1TB** as Windows 11.
  - **Samsung 970 EVO 1TB** as Data Storage

## Installation Steps

### 1. Prepare OpenCore Bootloader
- Download MacOS Install App and create a USB installer
- Download OpenCore Release and configure `config.plist`, ensuring correct ACPI patches, kexts, and device properties
- SMBIOS: MacPro7,1 or iMacPro1,1
- Use USBToolbox on Windows 11 to generate a `USBMap.kext`
- Use OpenCore Configurator to generate PlatformInfo (ROM based on motherboard MAC address)
- Adjust BIOS settings for Hackintosh

### 2. Install macOS Sequoia
- Boot into OpenCore Boot Picker and start the macOS installer
- Format SSD as APFS and install macOS
- Copy EFI folder to internal storage post-installation

## Post-Installation, Troubleshooting and Fixes

### USB Issues
- Reduced port count to below 15 to comply with macOS limitations
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
