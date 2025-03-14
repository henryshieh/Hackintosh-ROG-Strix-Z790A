# Hackintosh on ASUS ROG Strix Z790-A

## Introduction
This repository documents my experience installing macOS Sequoia on the ASUS ROG Strix Z790-A motherboard. It covers hardware configuration, OpenCore settings, troubleshooting, and optimizations to achieve a stable system.

## Hardware Specifications

- **Motherboard**: ASUS ROG Strix Z790-A
- **CPU**: Intel Core i9-14900K (iGPU disabled)
- **GPU**: AMD Radeon RX 6800 XT
- **Wi-Fi/Bluetooth**: Switched from Intel Wi-Fi to BCM943602CDP due to easier Sequoia support via OCLP root patch
- **Storage**:
  - **WD Black SN850X 2TB** (OpenCore + macOS Sequoia installation)
  - **Samsung 970 EVO Plus 1TB** (Windows 11)

## Why Broadcom Wi-Fi over Intel?

1. **Intel Wi-Fi is no longer supported**: OpenIntelWireless driver development has stalled after Sonoma. No updates for Sequoia are available.
2. **OCLP workaround for Intel Wi-Fi is cumbersome**: It requires deviceproperties workaround to fake the Intel Wi-Fi as Broadcom to trigger OCLP (OpenCore Legacy Patcher) to perform Modern Wireless Patch. 

## Installation Steps

### 1. Prepare OpenCore Bootloader
- Download MacOS Install App and create a USB installer
- Download OpenCore Release and configure `config.plist`, ensuring correct ACPI patches, kexts, and device properties
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
- Sleep/Wake may be broken if you connect mainboard's Type-C port to a monitor (in my case, LG 34WK95U-W) with Thunderbolt/Type-C support. Use mainboard's Type-A port or monitor's Type-B port instead.

### NVMe TRIM Compatibility
- Since Monterey, APFS has had compatibility issues with NVMe TRIM, causing long boot times or premature SSD failure
- Setting `SetApfsTrimTimeout` in OpenCore does not fully resolve the problem
- The **only** reliable solution is to use an NVMe SSD known to be compatible, such as the **WD Black SN850X**, which is widely regarded as stable

## Optimization

- If your SMBIOS is set to **MacPro7,1**, you can use `resourceconverter.sh` from CPUFriend to convert the `plist` from **iMacPro1,1** into `CPUFriendDataProvider.kext`, ensuring proper CPU performance.
- Run benchmark (such as GeekBench6) to compare with Windows 11 CPU/GPU performance scores.

## Conclusion
This Hackintosh setup is now stable and performs well. With precise OpenCore adjustments, the system offers excellent performance and compatibility. If you're planning a similar setup, I hope this documentation helps!

---

### Notes
- This repository **does not include** full EFI with SMBIOS details for security reasons. You must generate your own PlatformInfo.
- Contributions and discussions are welcome! Feel free to open an issue if you encounter similar challenges.
