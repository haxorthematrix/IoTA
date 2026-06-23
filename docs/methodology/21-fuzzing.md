[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Fuzzing Methodology**

Fuzzing is one of the most productive vulnerability discovery
techniques for IoT devices and should be applied systematically across
all identified interfaces and protocols. The constrained nature of
IoT devices (limited memory, no ASLR, minimal stack protections)
means that fuzzing-discovered crashes are more likely to be
exploitable than on general-purpose computing platforms.

### Protocol Fuzzing

For each identified network protocol, the IoTA team should define the
valid message format and then systematically generate malformed,
boundary, and unexpected inputs. Protocol-specific fuzzing should
cover: BLE GATT characteristic writes (oversized values, unexpected
types, writes to read-only characteristics), MQTT publish payloads
(oversized payloads, deeply nested JSON, malformed topic strings,
invalid QoS values, rapid connection/disconnection cycles), CoAP
request parameters (malformed URI paths, oversized options, invalid
content formats, block-wise transfer abuse), HTTP/REST API parameters
(SQL injection strings, command injection payloads, oversized headers,
malformed multipart bodies), Zigbee frame injection (malformed ZCL
commands, invalid cluster IDs, oversized payloads), and CAN/CAN-FD
message injection (invalid arbitration IDs, oversized payloads,
malformed UDS diagnostic requests). Tools such as boofuzz, Scapy, and
protocol-specific fuzzers should be used. Custom fuzz harnesses may
need to be developed for proprietary protocols.

### File Format and Input Fuzzing

Any structured data the device processes should be fuzzed. Priority
targets include firmware update images (corrupt headers, truncated
payloads, oversized metadata fields), configuration files (XML, JSON,
YAML, INI formats with malformed structure, injection payloads, and
encoding errors), certificate and key files (malformed X.509
certificates, invalid ASN.1 encoding, oversized fields), and media
files for devices that process images, audio, or video.

### Hardware Interface Fuzzing

The IoTA team should perform systematic injection of malformed data on
hardware communication buses. On UART interfaces, send unexpected
command sequences, oversized input, binary data to text-mode consoles,
and high-speed data to overwhelm buffer handling. On SPI and I2C
buses, inject malformed responses that mimic a compromised peripheral
device. On CAN buses, inject messages at high rates (bus flooding),
send messages with invalid DLC (Data Length Code) values, and test
error handling when the bus enters error-passive or bus-off states.

### Coverage-Guided Fuzzing on Extracted Binaries

Individual binaries extracted from the firmware can be fuzzed using
coverage-guided tools such as AFL++, honggfuzz, or libFuzzer running
under QEMU user-mode emulation or Unicorn. This approach is most
productive when applied to input-processing functions such as protocol
parsers, file format handlers, command interpreters, and cryptographic
operations. The team should identify high-value fuzzing targets
through static analysis (functions that handle external input, use
unsafe memory operations, or have complex parsing logic) and
instrument them for coverage-guided fuzzing.

### State Machine Fuzzing

Many IoT protocols have complex state machines where vulnerabilities
exist only in specific states or state transitions. The IoTA team
should use state-aware fuzzing approaches to explore protocol state
spaces: BLE pairing and bonding state machines, Matter PASE and CASE
commissioning sequences, TLS and DTLS handshake state machines, and
OTA firmware update sequences. Tools such as AFLNet (for network
protocol state machines) and custom Scapy scripts with state tracking
can be used for this purpose.
