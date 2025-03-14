# Hackintosh on ASUS ROG Strix Z790-A

## Introduction
This repository documents my experience installing macOS Sequoia on the ASUS ROG Strix Z790-A motherboard. It covers hardware configuration, OpenCore settings, troubleshooting, and optimizations to achieve a stable system.

## Hardware Specifications

- **Motherboard**: ASUS ROG Strix Z790-A
- **CPU**: Intel Core i9-14900K (iGPU disabled)
- **GPU**: AMD Radeon RX 6800 XT
- **Wi-Fi/Bluetooth**: Switched from Intel Wi-Fi to BCM943602CDP due to better macOS support
- **Storage**:
  - **WD Black SN850X 2TB** (OpenCore + macOS Sequoia installation)
  - **Samsung 970 EVO Plus 1TB** (Windows 11)

## Why Broadcom Wi-Fi over Intel?

1. **Intel Wi-Fi is not natively supported**: A third-party OpenIntelWireless driver enables it, but development has stalled after Sonoma. No updates for Sequoia are available.
2. **OCLP workaround for Intel Wi-Fi is cumbersome**: It requires device property modifications to disguise the Intel card as Broadcom or patching OpenCore Legacy Patcher (OCLP) itself. Broadcom Wi-Fi, however, still has native Bluetooth support in Sequoia and works with OCLP without extra tweaks.

## Installation Steps

### 1. Prepare OpenCore Bootloader
- Download OpenCore and create a USB installer
- Configure `config.plist`, ensuring correct ACPI patches, kexts, and device properties
- Use OpenCore Configurator to generate PlatformInfo (ROM based on motherboard MAC address)

### 2. Install macOS Sequoia
- Boot into OpenCore Boot Picker and start the macOS installer
- Format SSD as APFS and install macOS
- Copy EFI folder to internal storage post-installation
- Adjust BIOS settings for optimal stability

## Troubleshooting and Fixes

### USB Issues
- Used USBToolbox on Windows 11 to generate a `USBMap.kext`
- Reduced port count to below 15 to comply with macOS limitations

### Sleep/Wake Problems
- Adjusted SSDT and power management settings

### NVMe TRIM Compatibility
- Since Monterey, APFS has had compatibility issues with NVMe TRIM, causing long boot times or premature SSD failure
- Setting `SetApfsTrimTimeout` in OpenCore does not fully resolve the problem
- The **only** reliable solution is to use an NVMe SSD known to be compatible, such as the **WD Black SN850X**, which is widely regarded as stable

## Optimization

- If your SMBIOS is set to **MacPro7,1**, you can use `resourceconverter.sh` from CPUFriend to convert the `plist` from **iMacPro1,1** into `CPUFriendDataProvider.kext`, ensuring proper CPU performance.
- Ran extended stability tests to confirm reliability

## Conclusion
This Hackintosh setup is now stable and performs well. With precise OpenCore adjustments, the system offers excellent performance and compatibility. If you're planning a similar setup, I hope this documentation helps!

---

### Notes
- This repository **does not include** full EFI with SMBIOS details for security reasons. You must generate your own PlatformInfo.
- Contributions and discussions are welcome! Feel free to open an issue if you encounter similar challenges.
