[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Constructing a Lab**

Effective IoT security testing requires a dedicated lab environment
equipped to handle hardware analysis, RF testing, network simulation,
and firmware emulation. The lab should be designed to contain RF
emissions (preventing interference with production systems and
neighboring devices), isolate network traffic (preventing test
activities from reaching production networks), and provide the
electrical and physical infrastructure needed for safe hardware work.

### Physical Lab Requirements

The lab should include an ESD-safe workbench with proper grounding, a
stereo microscope with ring illumination for PCB inspection and
fine-pitch soldering, and adequate ventilation for soldering and
chemical work (conformal coating removal produces fumes that should be
properly exhausted). A Faraday cage or RF-shielded enclosure is
strongly recommended for wireless testing, as it prevents unintended
interference with nearby devices and ensures that RF testing is
conducted in a controlled environment. For teams conducting regular
automotive or industrial IoT testing, a separate workstation with
appropriate safety equipment (high-voltage isolation, current-limited
power supplies) may be necessary.

### Hardware Tool Requirements

The following categories of hardware tools represent the minimum
requirements for a professional IoT testing lab:

Multi-protocol interface adapters for UART, SPI, I2C, JTAG, and SWD
access. Tigard (FT2232H-based, open-source) provides a well-designed
starting point that covers the majority of common interface tasks. The
Glasgow Interface Explorer (iCE40 FPGA-based, open-source) provides
advanced capabilities for custom protocol implementation, unusual
timing requirements, and simultaneous multi-bus interaction.

Logic analyzers with protocol decode capability for monitoring bus
communications. The Saleae Logic Pro 16 is the industry standard, while
the BitMagic Basic provides a budget-friendly alternative that
integrates directly with Tigard and the Sigrok/PulseView software suite.

Flash and EEPROM programmers for firmware extraction. FlashcatUSB xPort
supports SPI, I2C, JTAG, parallel NOR/NAND, and eMMC. The CH341A
provides an extremely low-cost option for basic SPI flash dumping when
used with flashrom.

Software-defined radio hardware covering the frequency ranges used by
target devices. A HackRF One (1 MHz to 6 GHz, half-duplex TX/RX)
serves as the primary SDR for most IoT RF analysis. An RTL-SDR V4
(receive-only, 24 MHz to 1.7 GHz) provides an inexpensive option for
initial signal identification and monitoring with tools like rtl_433.
For cellular IoT testing, a USRP B210 or equivalent high-end SDR is
needed to run tools like srsRAN for creating private LTE/5G test
networks.

BLE analysis hardware. An nRF52840 USB Dongle running Sniffle firmware
provides BLE 5.x sniffing and injection capability, replacing the
end-of-life Ubertooth. An nRF Connect for Desktop installation with
additional nRF52840 dongles provides GATT browsing, advertisement
analysis, and RSSI monitoring.

RFID/NFC analysis hardware. A Proxmark3 RDV4 (with Iceman firmware)
provides comprehensive 125 kHz LF and 13.56 MHz HF RFID/NFC analysis
capability. A Flipper Zero provides a more portable option for quick
fieldwork across RFID, NFC, sub-GHz, and IR protocols.

Fault injection hardware. A ChipWhisperer Husky provides voltage
glitching, clock glitching, and side-channel power analysis in an
integrated open-source platform. A PicoEMP provides affordable
electromagnetic fault injection capability.

Soldering and rework equipment. A temperature-controlled soldering
station (Hakko FX-951 or equivalent) and hot air rework station (Quick
861DW or equivalent) are necessary for component-level work including
conformal coating removal, test point access, and chip removal.

Probing equipment. PCBite magnetic probe holders with spring-loaded
needle probes enable hands-free probing of small test points. Standard
test clips (SOIC-8, SOIC-16, TSSOP, and similar) are needed for
in-circuit flash reading.

### Network Lab Requirements

An isolated network environment is essential for IoT device testing.
This should include a managed switch with VLAN capability for
segmenting test networks, a dedicated Wi-Fi access point under the
tester's control (for testing device Wi-Fi behavior, client isolation,
and MITM scenarios), and a DHCP/DNS server that can be configured to
simulate or redirect the device's expected network environment. For
devices that communicate with cloud backends, the lab should support
transparent proxying (using tools like mitmproxy or Burp Suite) to
intercept and analyze device-to-cloud traffic. Where cellular IoT
testing is required, a software-defined cellular network (srsRAN +
Open5GS) with appropriate SDR hardware enables controlled testing of
device cellular behavior without interference with production networks.

### Software Environment

The testing workstation should run a Linux distribution with support
for the required hardware tools. Kali Linux provides a comprehensive
pre-installed toolset for network and web application testing, while
AttifyOS provides a distribution specifically focused on IoT security
assessment tools. Virtual machines or containers should be available for
firmware emulation (QEMU, Qiling, FIRMADYNE), cloud infrastructure
testing (using cloud provider CLIs and assessment tools), and isolated
malware analysis. A dedicated installation of Ghidra, Binary Ninja, or
IDA Pro is required for firmware reverse engineering. Burp Suite
Professional is recommended for web application and API testing.

### Budget Considerations

A functional IoT testing lab can be assembled at various budget levels.
A starter kit sufficient for basic hardware hacking, BLE analysis, and
sub-GHz RF work can be assembled for approximately $500-800, using
tools such as Tigard, a BitMagic Basic logic analyzer, CH341A flash
programmer, RTL-SDR V4, nRF52840 dongle, Pinecil V2 soldering iron,
and open-source software. A professional lab with comprehensive
capability across all protocol domains, including fault injection,
automotive interfaces, and cellular simulation, typically requires
$5,000-10,000 in hardware investment. A companion Tool Reference
document provides detailed hardware and software recommendations at
each budget tier.
