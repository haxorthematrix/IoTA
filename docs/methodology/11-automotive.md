[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Automotive IoT Specific**

Connected vehicles and their supporting infrastructure represent a
rapidly growing and high-consequence IoT subsector. Regulatory
frameworks including UNECE WP.29 R155/R156 and ISO/SAE 21434 mandate
cybersecurity management and type approval for vehicles. The following
methodology addresses automotive-specific attack surfaces.

### CAN/CAN-FD Bus Security

The IoTA team should evaluate the vehicle's CAN bus architecture for
message authentication (or lack thereof), ECU spoofing and injection,
diagnostic service (UDS) abuse, and gateway bypass. Tools such as
python-can, SavvyCAN, CANalyze, and CaringCaribou support CAN/CAN-FD
analysis, with CaringCaribou providing specific modules for UDS
scanning, ECU discovery, and security access bypass.

### Automotive Ethernet and In-Vehicle Networking

SOME/IP (Scalable service-Oriented MiddlewarE over IP) service
discovery and exploitation, DoIP (Diagnostics over IP) authentication,
and VLAN segmentation within the vehicle network should be evaluated.
Scapy's automotive protocol layer provides custom packet crafting
capabilities for SOME/IP, DoIP, and related protocols.

### EV Charging Infrastructure

OCPP (Open Charge Point Protocol) implementations should be evaluated
for authentication bypass, charge session manipulation, and grid-level
denial of service. ISO 15118 (Plug and Charge) implementations should
be tested for certificate handling vulnerabilities and unauthorized
charge session establishment.

### Telematics and V2X

Telematics Control Unit (TCU) security should be evaluated including
cellular modem configuration, remote access capabilities, and OTA
update mechanisms. V2X (Vehicle-to-Everything) communications using
DSRC (802.11p) or C-V2X (cellular V2X) should be evaluated for
message spoofing, replay attacks, and PKI certificate management.
