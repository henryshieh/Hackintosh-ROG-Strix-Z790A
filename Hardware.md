# Hackintosh Build: Comprehensive Hardware Configuration

## Processor and Cooling Solution: Preparing for Peak Performance

For this build, I chose the Intel Core i9-14900K, the flagship processor of its generation, delivering top-tier performance and marking the last in the line of Hackintosh-compatible Intel CPUs.

To cool the i9-14900K, I reused my Noctua NH-D15S air cooler, which I found sufficient for this CPU’s thermal needs. Initially, I purchased the NM-i17xx-MP83 mounting kit to ensure compatibility with the Z790 motherboard’s LGA1700 socket. However, I later discovered that my motherboard, being the DDR4 variant, also supported older mounting brackets. This allowed me to install the cooler without requiring the new kit.

---

## Memory Configuration: Utilizing Existing Resources

For this build, I reused my existing Kingston HyperX DDR4-3600 (OC) modules, each with a capacity of 32GB, totaling 64GB. Modern motherboards primarily use two types of memory bus topologies: **T-Topology** and **Daisy Chain**.  
- **T-Topology** is optimized for configurations where all four DIMM slots are populated, ensuring uniform signal timing across the slots.  
- **Daisy Chain** is designed to provide better performance when only two DIMM slots are in use, making it ideal for dual-channel setups.  

For those curious about the technical details, these terms can be explored further online. However, if you're not familiar with circuit design, there's no need to worry—just follow the slot placement recommendations in the motherboard manual to ensure optimal performance.

---

## Graphics Card: Balancing Performance and Compatibility

For this build, I selected a used Sapphire Radeon RX6800XT Special Edition. With macOS no longer supporting newer GPU series like the RX7000, the RX6800XT remains one of the most powerful options compatible with Hackintosh systems. Apart from the RX6900XT and RX6950XT, it offers an excellent balance of performance and compatibility for this project.

---

## NVMe Storage: Strategic Allocation for macOS and Windows

Before macOS Monterey, I used Samsung 970 EVO and 970 EVO Plus SSDs as boot drives for macOS. However, with Monterey introducing issues related to APFS and TRIM functionality, these SSDs have been relegated to data storage roles rather than serving as macOS boot drives.

For my new build, the NVMe storage configuration is as follows:
- **WD SN850X 2TB**: Serves as the primary boot drive for macOS Sequoia, chosen for its proven compatibility and stable performance.  
- **Samsung 970 EVO Plus 1TB**: Dedicated to Windows 11 installation, which is critical for tasks such as dumping the ACPI table, running `ssdttime`, and generating `usbmap.kext` using USBToolBox as part of the Hackintosh setup.  
- **Samsung 970 EVO 1TB**: Allocated as a data drive for macOS, storing files and resources without impacting the performance of the primary boot drive.

---

## WiFi and Bluetooth: Overcoming Compatibility Challenges

This motherboard’s built-in Intel wireless card is supported by OpenIntelWireless. However, as of 2024, development for this project has stalled, leaving the Intel wireless card unable to function directly under macOS Sequoia without additional configuration. Initially, I took the following steps to make it work:
1. **Device Properties Configuration**  
   Use OpenCore's configuration file to declare device properties, spoofing the Intel wireless card as a Broadcom BCM94360CD. This enables the OpenCore Legacy Patcher (OCLP) to recognize and activate its modern wireless patch.

2. **OpenCore Legacy Patcher Root Patch**  
   Execute a root patch via OCLP to implement the modern wireless patch. This modifies the macOS wireless networking components to rely on Ventura-era system elements, enabling support for legacy Broadcom drivers and indirectly supporting the Intel wireless card.

3. **Final Driver Setup**  
   Once the modern wireless patch was applied, the spoofed device properties could be removed. The Intel wireless card was then driven using the Ventura-compatible `Airportitlwm.kext`, restoring its functionality.

After successfully configuring the Intel card, I began to question whether all this effort was necessary. Given that Broadcom wireless cards like the BCM94360CD and BCM943602CDP also require OCLP for modern macOS versions starting with Sonoma, I decided to simplify the process. I ordered a few Broadcom cards from OSXWiFi, which are currently on their way. These cards should provide a more straightforward and reliable solution moving forward.

---

## Other Components: Leveraging Existing Hardware

For this build, I reused several components from my previous setup:
- **Case**: ThermalTake Level 20 XT (discontinued). I prefer horizontal cases, which are now rare in the market, making this model truly unique.  
- **Power Supply**: Super-Flower Leadex Titanium 1000W (SF-1000F14HT), also discontinued. While affordable and seemingly durable, its fan quality leaves room for improvement and can be somewhat noisy. I may consider replacing the fan with a Noctua unit to reduce noise. If I need to upgrade in the future, I’ll likely explore titanium-grade options from brands like Be Quiet.  
- **Case Fans**: Mostly Noctua fans, which fit perfectly into the enormous case. The spacious interior even accommodates 20cm fans, ensuring excellent airflow.  
- **RGB Lighting**: Added to enhance the system's visual appeal, though macOS does not support RGB lighting controls.

---

## Additional Notes on Peripheral and Audio Solutions

For external peripherals such as an optical drive or other devices, I opted for USB-connected solutions rather than internal PCIe cards. Currently, I have no additional PCIe expansion cards installed in this build.

Regarding audio, this motherboard employs a USB-connected design for its built-in audio solution, differing from traditional onboard setups. As I do not prioritize audio quality directly from the motherboard, I utilize an optical connection to an amplifier for high-quality sound when needed.

Finally, on NVMe storage cooling, modern motherboards like this one come equipped with integrated heatsinks. These have proven sufficient to manage the thermal needs of the installed drives, eliminating the need for additional cooling solutions.

---
