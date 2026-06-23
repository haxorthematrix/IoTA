[← Back to IoTA Index](../README.md)

# Appendix A: Tool Reference

This appendix provides recommended software and hardware tools for each
methodology area. Tools are organized into three tiers:

- **Best of Breed:** The go-to tool. Mature, well-supported, widely
  used by professionals.
- **Good Breed:** Solid alternatives with trade-offs in polish,
  community, or features, but fully capable for professional work.
- **Focused:** Narrower scope tools that do one specific thing very
  well.

Tools are tagged as [OSS] (open-source), [COM] (commercial/paid), or
[HW] (physical hardware). Approximate prices noted where publicly
available.

## Reconnaissance and OSINT

- Shodan [COM] -- Internet-wide IoT device search. Free tier; $69/mo membership.
- Recon-ng [OSS] -- Modular Python recon framework.
- Censys [COM] -- Internet scanner with strong TLS/certificate analysis.
- SpiderFoot [OSS] -- Automated OSINT across 200+ sources.
- theHarvester [OSS] -- Email, subdomain, IP enumeration.
- FCCID.io [Free web] -- FCC equipment authorization filings database.
- crt.sh [Free web] -- Certificate transparency log search.
- GitDorker / TruffleHog [OSS] -- GitHub/GitLab secret scanning.

## SBOM and Software Composition Analysis

- Finite State Platform [COM] -- Binary firmware analysis, SBOM generation, CRA compliance.
- EMBA [OSS] -- Comprehensive firmware security analyzer with SBOM generation.
- Syft [OSS] -- SBOM generator for containers, filesystems, archives (CycloneDX/SPDX output).
- Grype [OSS] -- Vulnerability scanner consuming SBOMs from Syft.
- FACT [OSS] -- Fraunhofer firmware analysis framework with component ID.
- OSV-Scanner [OSS] -- Google vulnerability database scanner.
- Dependency-Track [OSS] -- OWASP SBOM monitoring platform.

## Hardware: Multi-Protocol Interface Adapters

- Tigard [HW/OSS] (~$50) -- FT2232H-based. UART, SPI, I2C, JTAG, SWD. Level shifting. Drop-in OpenOCD/flashrom compatible.
- Glasgow Interface Explorer [HW/OSS] (~$139) -- iCE40 FPGA-based. Python applet architecture. Unmatched flexibility.
- Bus Pirate 5 / ESP32 Bus Pirate [HW/OSS] (~$35-50) -- Interactive terminal interface. Wi-Fi/BLE on ESP32 variant.
- GreatFET One [HW/OSS] (~$100) -- USB-based, modular neighbor boards.
- JTAGulator [HW/OSS] (~$180) -- Dedicated JTAG/UART pin identification. 24 I/O channels.
- JTAGenum [OSS] -- Arduino-based JTAG/SWD/UART brute-force enumeration.
- blueTag [HW/OSS] -- Bluetooth-enabled JTAG scanner.

## Hardware: Logic Analyzers

- Saleae Logic Pro 16 [HW/COM] (~$1,500) -- 16ch, 500 MS/s, protocol decoders. Industry standard.
- Saleae Logic 8 [HW/COM] (~$500) -- 8ch, 100 MS/s. Same software.
- DSLogic Plus [HW] (~$150) -- 16ch, 400 MS/s, Sigrok-compatible.
- BitMagic Basic [HW/OSS] (~$40) -- 8ch, FX2-based. Designed for Tigard integration.
- Sigrok / PulseView [OSS] -- Open-source signal analysis software.

## Hardware: Flash/EEPROM Programmers

- FlashcatUSB xPort [HW/COM] (~$30-50) -- SPI, I2C, JTAG, parallel NOR/NAND, eMMC.
- CH341A Programmer [HW] (~$5-10) -- Ultra-cheap SPI/I2C programmer. Use with flashrom.
- Flashrom [OSS] -- Open-source flash programming utility. Dozens of hardware backends.
- Dediprog SF600 Plus [HW/COM] (~$300) -- Professional SPI flash programmer.
- eMMC adapter boards [HW] (~$15-30) -- ISP adapters for eMMC reading via SD card readers.

## Hardware: Fault Injection and Side-Channel

- ChipWhisperer Husky [HW/OSS] (~$550) -- Voltage/clock glitching, SPA/DPA/CPA. Python API.
- ChipWhisperer Nano [HW/OSS] (~$60) -- Entry-level voltage glitching.
- PicoEMP [HW/OSS] (~$50-80) -- Electromagnetic fault injection.
- Riscure Inspector [HW/COM] ($$$$) -- Commercial-grade SCA and fault injection.

## Hardware: Soldering and Probing

- Hakko FX-951 [HW] (~$250) -- Professional soldering station.
- Quick 861DW [HW] (~$300) -- Hot air rework with temperature profiling.
- Pinecil V2 [HW] (~$30) -- Portable USB-C soldering iron, open-source firmware.
- PCBite system [HW] (~$200) -- Magnetic probe holders with needle probes.
- Amscope SM-4TZ-144A [HW] (~$350) -- Stereo microscope with ring light.

## Software Defined Radio

- HackRF One [HW/OSS] (~$350) -- 1 MHz to 6 GHz, half-duplex TX/RX. The workhorse SDR.
- RTL-SDR V4 [HW] (~$30) -- RX-only, 24 MHz to 1.7 GHz. Best price-to-capability for receive.
- ADALM-Pluto (PlutoSDR) [HW] (~$200) -- 325 MHz to 3.8 GHz, full duplex. Better signal quality.
- LimeSDR Mini 2.0 [HW] (~$230) -- Full duplex, MIMO capable.
- USRP B210 [HW/COM] (~$2,500) -- Professional grade. 70 MHz to 6 GHz, 2x2 MIMO. For cellular.
- Yard Stick One [HW/OSS] (~$120) -- Sub-GHz transceiver for 300-928 MHz attacks.
- Flipper Zero [HW] (~$170) -- Multi-tool: sub-GHz, RFID, NFC, IR, GPIO.
- GNU Radio [OSS] -- Signal processing framework. Gold standard for SDR.
- SDR++ [OSS] -- Modern cross-platform SDR receiver application.
- Universal Radio Hacker (URH) [OSS] -- Protocol reverse engineering focused.
- Inspectrum [OSS] -- Offline signal analysis and visualization.
- rtl_433 [OSS] -- Decoder for hundreds of sub-GHz OOK/FSK devices.
- SigMF [OSS standard] -- Signal Metadata Format for reproducible RF captures.

## BLE (Bluetooth Low Energy)

- Sniffle [HW/OSS] (requires CC1352/CC26x2 LaunchPad, ~$30-50) -- BLE 5.x sniffer/injector. Replaces Ubertooth.
- nRF Connect for Desktop [Free/COM] (with nRF52840 Dongle, ~$10) -- BLE analysis suite.
- btlejack [OSS] (requires Micro:Bit boards) -- BLE connection hijacking.
- GATTacker [OSS] -- BLE MITM and GATT proxy.
- Bettercap [OSS] -- Network attack framework with BLE modules.

## Zigbee / IEEE 802.15.4

- KillerBee [OSS] (with ApiMote or RZ Raven) -- Zigbee/802.15.4 attack framework.
- Z3sec [OSS] -- Zigbee security testing via Scapy.
- nRF52840 Dongle with Zigbee sniffer firmware [HW] (~$10) -- Works with Wireshark.
- Silicon Labs WSTK [HW/COM] (~$100-300) -- Thread/Zigbee/BLE development kit.

## LoRa / Meshtastic

- HackRF One + gr-lora [OSS] -- GNU Radio LoRa decoder blocks.
- Meshtastic Python CLI [OSS] -- Device config, packet monitoring, MQTT bridging.
- LoStik [HW] (~$50) -- USB LoRa transceiver (SX1276).
- rtl_433 [OSS] -- Decodes many LoRa-adjacent sub-GHz protocols.

## Wi-Fi

- Aircrack-ng suite [OSS] -- Comprehensive Wi-Fi auditing toolkit.
- Bettercap [OSS] -- Wi-Fi recon, deauth, MITM, credential harvesting.
- hcxtools / hcxdumptool [OSS] -- WPA/WPA2/PMKID capture and hashcat integration.
- Kismet [OSS] -- Wireless network detector, sniffer, and IDS.
- ESP32 Marauder [HW/OSS] (~$30) -- ESP32-based Wi-Fi/BLE offensive tool.
- Wi-Fi Pineapple [HW/COM] (~$100-300) -- Hak5 dedicated Wi-Fi auditing platform.

## RFID / NFC

- Proxmark3 RDV4 [HW/OSS] (~$300) -- Gold standard. 125 kHz LF, 13.56 MHz HF. Iceman firmware.
- Flipper Zero [HW] (~$170) -- Basic RFID/NFC read/write/emulate. Portable.
- ChameleonUltra [HW/OSS] (~$70) -- NFC emulation, credit card form factor.
- ACR122U [HW] (~$40) -- USB NFC reader/writer. Works with libnfc.

## Cellular IoT

- srsRAN Project [OSS] -- Open-source 4G/5G RAN. Requires USRP B210 or similar.
- Open5GS [OSS] -- Open-source 5G core network.
- QCSuper [OSS] -- Qualcomm baseband diagnostic tool.
- SIMtrace 2 [HW/OSS] (~$100) -- SIM card protocol tracer.
- sysmoISIM-SJA5 [HW] (~$15) -- Programmable SIM card for research.

## Firmware Analysis

- Binwalk [OSS] -- Firmware image analysis, extraction, decompression.
- Unblob [OSS] -- Modern firmware extraction, 30+ formats.
- Ghidra [OSS] -- Disassembler/decompiler. ARM, MIPS, x86, RISC-V.
- Binary Ninja [COM] (~$300 personal) -- Modern RE platform with strong API.
- IDA Pro [COM] (~$1,900+) -- Industry standard RE tool.
- Cutter / Rizin [OSS] -- Open-source RE framework (radare2 successor).
- Frida [OSS] -- Dynamic instrumentation for Linux, iOS, Android, embedded.
- QEMU [OSS] -- System and user-mode emulation for firmware analysis.
- Unicorn Engine [OSS] -- Lightweight CPU emulator for code snippet analysis.
- Qiling [OSS] -- Higher-level emulation with OS API support.
- FIRMADYNE [OSS] -- Automated Linux firmware emulation.
- hashcat [OSS] -- Password hash cracking. GPU accelerated.
- John the Ripper [OSS] -- Password cracking with broad format support.
- GDB + GEF/pwndbg [OSS] -- Debugger with exploitation extensions.

## Web Application and API

- Burp Suite Professional [COM] (~$450/yr) -- Industry standard web proxy/scanner.
- mitmproxy [OSS] -- Scriptable HTTPS proxy in Python.
- OWASP ZAP [OSS] -- Open-source web security scanner.
- Nuclei [OSS] -- Template-based vulnerability scanner.
- ffuf [OSS] -- Fast web fuzzer.
- SQLMap [OSS] -- Automated SQL injection.
- MQTT Explorer [OSS] -- MQTT client with topic visualization.
- MQTT-PWN [OSS] -- MQTT penetration testing tool.
- Postman [COM, free tier] -- API development and testing.
- Hoppscotch [OSS] -- Open-source API testing (REST, GraphQL, WebSocket, MQTT).

## Mobile Application

- Frida [OSS] -- Dynamic instrumentation for iOS and Android.
- jadx [OSS] -- Android APK decompiler.
- MobSF [OSS] -- Automated mobile app static/dynamic analysis.
- Objection [OSS] -- Frida-powered runtime exploration toolkit.
- apktool [OSS] -- Android APK disassembly/reassembly.
- Hopper [COM] (~$100) -- macOS/iOS disassembler.

## Network

- Nmap [OSS] -- Network discovery and security auditing.
- Wireshark [OSS] -- Network protocol analyzer.
- Masscan [OSS] -- High-speed port scanner.
- Bettercap [OSS] -- ARP/DNS spoofing, MITM, credential capture.
- Scapy [OSS] -- Python packet manipulation library.
- Responder [OSS] -- LLMNR/NBT-NS/mDNS poisoner.

## Automotive

- CANalyze [HW/OSS] (~$30) -- USB CAN/CAN-FD analyzer.
- PCAN-USB FD [HW/COM] (~$350) -- Peak Systems CAN-FD adapter.
- ValueCAN 4-2 [HW/COM] (~$1,200) -- Professional dual-channel CAN-FD.
- python-can [OSS] -- Python CAN bus library.
- SavvyCAN [OSS] -- CAN bus RE and analysis tool.
- CaringCaribou [OSS] -- Automotive security testing (UDS, ECU discovery).
- can-utils (socketCAN) [OSS] -- Linux CAN utilities.
- Scapy automotive layer [OSS] -- CAN, UDS, DoIP, SOME/IP packet crafting.

## Cloud and Backend

- ScoutSuite [OSS] -- Multi-cloud security auditing (AWS, Azure, GCP).
- Prowler [OSS] -- AWS/Azure security assessment.
- Pacu [OSS] -- AWS exploitation framework.
- S3Scanner [OSS] -- Misconfigured S3 bucket scanner.

## Reporting and Compliance

- Dradis CE [OSS] -- Collaborative pentest reporting platform.
- PlexTrac [COM] -- Commercial pentest management platform.
- OWASP IoT Top 10 [Free reference] -- Standardized vulnerability categories.
- ETSI EN 303 645 [Free reference] -- 13-provision consumer IoT security checklist.
- MITRE EMB3D [Free reference] -- Threat model for embedded devices.

## Starter Kit (~$500-800)

| Category | Tool | Approx. Cost |
|----------|------|-------------|
| Interface adapter | Tigard | $50 |
| Logic analyzer | BitMagic Basic | $40 |
| Flash programmer | CH341A + clips | $10 |
| SDR | RTL-SDR V4 | $30 |
| BLE | nRF52840 Dongle + Sniffle | $15 |
| Wi-Fi adapter | Alfa AWUS036ACH | $35 |
| RFID/NFC | Flipper Zero | $170 |
| Soldering | Pinecil V2 | $30 |
| Probing | PCBite or test clips | $50-200 |
| Software | All OSS tools listed above | $0 |

## Professional Lab (~$5,000-10,000)

| Category | Tool | Approx. Cost |
|----------|------|-------------|
| Interface adapters | Tigard + Glasgow | $190 |
| Logic analyzer | Saleae Logic Pro 16 | $1,500 |
| Flash programmer | FlashcatUSB + eMMC adapters | $80 |
| SDR | HackRF One + PlutoSDR | $550 |
| BLE | Sniffle (CC1352 LP) + nRF52840 | $60 |
| Zigbee | KillerBee + ApiMote | $200 |
| Wi-Fi | Alfa cards + Wi-Fi Pineapple | $350 |
| RFID/NFC | Proxmark3 RDV4 | $300 |
| Fault injection | ChipWhisperer Husky + PicoEMP | $600 |
| Automotive | CANalyze + PCAN-USB FD | $380 |
| Soldering | Hakko FX-951 + Quick 861DW | $550 |
| Microscope | Amscope stereo + ring light | $350 |
| Probing | PCBite kit | $200 |
| Web/API | Burp Suite Pro | $450/yr |
| Cellular | SIMtrace 2 + programmable SIMs | $130 |
| Software | All OSS + Binary Ninja personal | $300 |
