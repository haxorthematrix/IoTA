[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Traffic Capture, Protocol Sniffing, and Analysis**

Systematic traffic capture and analysis is a foundational activity
that informs nearly every other area of the IoTA methodology. Before
attempting to exploit any vulnerability, the IoTA team should build a
comprehensive understanding of the device's communication behavior
by capturing, decoding, and analyzing all traffic across every
communication channel the device uses.

### Establishing a Traffic Baseline

The IoTA team should capture traffic during all phases of device
operation: initial power-on and boot sequence, first-time setup and
provisioning, normal steady-state operation, configuration changes,
firmware update checks and downloads, error conditions and recovery,
and device shutdown or sleep transitions. The captured traffic forms a
baseline that reveals the device's communication partners, protocols
in use, data formats, authentication patterns, and encryption
posture. Any deviation from this baseline during subsequent testing
phases may indicate undocumented behavior, hidden communication
channels, or exploitable conditions.

### Wired Protocol Capture

For devices with wired network interfaces, the IoTA team should use
passive tapping (network TAPs for Ethernet, bus analyzers for CAN/
CAN-FD, logic analyzers for SPI/I2C/UART) to capture traffic without
altering the communication. Wireshark is the primary analysis tool
for Ethernet-based captures and supports deep dissection of IoT-
relevant protocols including MQTT, CoAP, AMQP, Modbus TCP, OPC UA,
SOME/IP, DoIP, and many others. For serial bus protocols (UART, SPI,
I2C), logic analyzers with protocol decode capability (Saleae Logic
Pro, Sigrok/PulseView) convert raw electrical signals into decoded
protocol data. For CAN/CAN-FD traffic, tools such as SavvyCAN,
candump (socketCAN), and python-can capture and decode bus traffic.

### Wireless Protocol Capture

Each wireless protocol requires appropriate hardware and software for
capture:

- Wi-Fi: A monitor-mode capable adapter (such as an Alfa AWUS036ACH)
  with Wireshark, Kismet, or tcpdump captures all Wi-Fi frames
  including management, control, and data frames. For encrypted
  Wi-Fi traffic, the team must capture the four-way handshake and
  possess the PSK to decrypt in Wireshark.
- BLE: Sniffle (on TI CC1352/CC26x2 hardware) captures BLE
  advertisements and connection traffic for BLE 4.x and 5.x. The
  nRF Sniffer for Bluetooth LE (on nRF52840 Dongle) provides
  Wireshark integration for BLE protocol analysis.
- Zigbee/Thread: The nRF52840 Dongle with Zigbee sniffer firmware or
  KillerBee with ApiMote hardware captures IEEE 802.15.4 frames.
  Wireshark decodes Zigbee and Thread protocols when provided with
  the network key.
- Sub-GHz (LoRa, proprietary): SDR hardware (HackRF One, RTL-SDR V4)
  combined with GNU Radio, URH, or rtl_433 captures and decodes
  sub-GHz transmissions.
- Cellular: QCSuper captures diagnostic messages from Qualcomm-based
  modems. For full cellular protocol capture, a software-defined
  cellular network (srsRAN + USRP B210) enables controlled capture
  of all device-to-network traffic.

### Encrypted Traffic Analysis

When traffic is encrypted (as it should be), the IoTA team can still
extract valuable information from metadata analysis. This includes:
connection timing and frequency (how often does the device phone
home?), destination IP addresses and DNS queries (what services does
the device communicate with?), traffic volume patterns (can device
activity be inferred from traffic volume even without decrypting
content?), TLS fingerprinting (JA3/JA4 hashes identify the TLS
client implementation), and certificate exchange analysis during TLS
handshakes. Where the IoTA team has access to private keys (extracted
from firmware or a secure element), Wireshark can decrypt TLS 1.2
traffic using the private key and TLS 1.3 traffic using per-session
keys logged via a key log file from an intercepting proxy.

### Cross-Protocol Correlation

Modern IoT devices often use multiple protocols simultaneously (BLE
for local control, Wi-Fi for cloud connectivity, MQTT for telemetry,
HTTP for management). The IoTA team should correlate captures across
protocols to identify relationships between events on different
channels. For example, a BLE command sent from the mobile app may
trigger a corresponding MQTT message to the cloud backend, and
comparing the content and timing of both reveals how the device
translates between protocols and whether security properties
(authentication, encryption, integrity) are maintained across the
translation.
