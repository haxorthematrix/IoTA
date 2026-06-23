[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **RF specific**

### Identification and RF Characterization

In order to perform network analysis (sniffing) and injection attacks,
the attack-team must first identify key radio information, including
frequency spectrum, modulation, channel selection, frequency hopping
patterns, and higher-level protocols.

An example set of this information follows for a ZigBee implementation:

-   Frequency range: 2.4GHz / 868KHz / 915KHz (802.15.4)

-   Modulation: 2.4GHz: MSK, 868/915KHz: BPSK

-   Channel Selection: Manual: 16/10/1 channels for 2.4GHz/915KHz
    /868KHz

-   Channel Access: CSMA-CA

-   Hopping Pattern: N/A

-   Higher-level protocol: ZigBee (Security-Enhanced Profile)

The attack-team can find information available from the communication
chip's datasheet and the FCC test reports.

Other examples of commonly used RF protocols include but are not limited
to:

-   Bluetooth Classic

-   BLE/BTLE/Bluetooth Smart/Bluetooth 4

-   Zigbee

-   ZWave

-   LoRa

-   RFID

-   NFC

-   Proprietary

Modern IoT deployments also utilize the following protocols, which
the IoTA team should be prepared to evaluate:

-   BLE 5.0/5.1/5.2/5.3/5.4 (including direction finding, LE Audio,
    and periodic advertising with responses)
-   Zigbee 3.0 and Zigbee Direct (BLE-to-Zigbee bridging)
-   Z-Wave Long Range (up to 1.6 km, 100 kbps)
-   Thread 1.3/1.4 (IEEE 802.15.4-based mesh, integral to Matter)
-   Matter/CHIP (application layer over Thread/Wi-Fi/Ethernet, with
    BLE commissioning)
-   UWB (IEEE 802.15.4z, used for secure ranging, digital car keys,
    and spatial awareness)
-   DECT ULE (smart home sensors, primarily European market)
-   LoRaWAN 1.0.x and 1.1 (including class A/B/C security differences)
-   Meshtastic (open-source LoRa mesh networking, rapidly growing)
-   Amazon Sidewalk (900 MHz FSK/BLE hybrid)
-   Wi-SUN (IEEE 802.15.4g, smart grid and utility networks)
-   NB-IoT (Cat-NB1/NB2)
-   LTE-M (Cat-M1)
-   5G NR (including URLLC for industrial IoT, mMTC for massive IoT)
-   Wi-Fi HaLow (802.11ah, sub-GHz long-range Wi-Fi)
-   Satellite IoT (Swarm, Starlink Direct-to-Cell, Iridium Certus)
-   Sigfox (declining but still deployed in some regions)
-   Infrared (IR) (increasingly rare but still present in some consumer
    devices for remote control and proximity communication)

### Unencrypted or Weak Encryption used in RF Communications

By use of either a compatible radio receiver or a bus protocol analyzer,
the team will capture a number of digital radio packets. The presence or
absence of cryptography should be immediately apparent by comparing just
a few packets visually. The bits of an encrypted packet appear random;
therefore, non-random bits indicate unencrypted traffic.

Equally important to using encryption is using a modern encryption free
of defects and known vulnerabilities that reduce or negate its
effectiveness in a wireless environment. A good example of weak wireless
encryption is the Wired Equivalent Privacy or WEP encryption which is
subject to multiple known exploitable vulnerabilities.

### No PIN/password or default PIN/password

Several wireless protocols can utilize a PIN or passcode during the
pairing or connection process with another device. If the IoT device
does not take advantage of these types of security controls or if it
uses a default value that is easily guessable or publicly available, an
attacker may be able to connect wirelessly to the device in question.
Once this is achieved, they can potentially prevent valid users and
devices from connecting or start attacking deeper layers of the IoT
device and its protocols.

### RF Sniffing

If the RF packets being transmitted are unencrypted or utilize weak
encryption implementations, it is possible to capture these
transmissions with either a radio receiver or a bus analyzer. This may
reveal potentially sensitive information sent to and from the device.

### Replay attacks 

If bits are random within each packet, yet identical packets are seen to
repeat, an analyst can deduce that the cryptography is likely vulnerable
to a ciphertext replay attack. An IoTA team may be able to obtain an
existing tool to perform these types of attacks. For example, an ApiMote
device can be used with KillerBee to perform replay attacks against
vulnerable Zigbee implementations. Alternatively, if a tool does not
exist or is difficult to obtain, an IoTA team may be able to create one
themselves using a development kit for the appropriate radio chip or
chipset involved.

### Impersonation attacks

### Jamming attacks

RF jamming is a type of denial of service (DoS) attack which occurs when
a device transmits interfering signals that prevent a target device from
successfully sending or receiving data within its wireless network. This
type of attack can be performed with tools like the HackRF or RTL-SDR.
From the perspective of the IoTA team, the end goal may not be to see if
a device can be jammed or not, rather a team may want to determine how
the device will fail if a signal is jammed. A jammed signal that
negatively impacts a device by causing a catastrophic software failure
or which causes other devices in the wireless network to fail should be
of the utmost concern.

Note: Jamming activities should not be taken lightly by the IoTA team.
Intentionally causing interference with other devices is prohibited by
law in many regions and the effective range of jamming could impact
devices outside of the scope of the test. A properly isolated lab
environment equipped with faraday cages and RF shielding should be used
to avoid potential conflicts.

### Reverse Engineering of Proprietary RF

Reverse engineering proprietary RF protocols involves a systematic approach to decipher the communication parameters and structure of the transmitted data. This process typically includes analyzing the frequency, modulation, encoding schemes, and packet breakdown.

#### Frequency

Identifying the operating frequency is the first step in reverse engineering RF communications. This involves using a spectrum analyzer to detect the frequency bands utilized by the target device. Common frequency bands include industrial, scientific, and medical (ISM) bands such as 433 MHz, 868 MHz, and 2.4 GHz. Accurate frequency identification is crucial for tuning receivers and further analysis.

### Modulation

Modulation defines how data is represented over RF signals. Common modulation schemes include Amplitude Shift Keying (ASK), Frequency Shift Keying (FSK), and Phase Shift Keying (PSK). Determining the modulation type can be achieved by analyzing the signal's waveform using an oscilloscope or software-defined radio (SDR) tools. Understanding the modulation scheme is essential for demodulating the signal into a baseband data stream.

### Encoding

Encoding refers to how binary data is formatted for transmission. Protocols may use encoding schemes such as Non-Return to Zero (NRZ), Manchester encoding, or others. Identifying the encoding method requires examining the demodulated data stream to interpret the bit patterns correctly. This step is vital for accurately reconstructing the transmitted data.

### Packet Breakdown

RF communications are typically structured into packets comprising several components:

Sync Word: A predefined sequence that indicates the start of a packet, allowing receivers to synchronize with the incoming data stream.

Headers: Contain metadata such as address information, packet type, and length, which are essential for proper data interpretation and routing.

Data Payload: The actual user data transmitted.

Analyzing the packet structure involves capturing multiple packets and identifying consistent patterns corresponding to these components. Tools like SDRs combined with protocol analysis software such as RTL_433 and Universal Radio Hacker (URH) can facilitate this process.

#### ***Rolling Code Analysis***

For access control and remote entry systems (garage door openers,
vehicle key fobs, gate controllers), the IoTA team should evaluate
rolling code implementations for known cryptographic weaknesses.
Common rolling code schemes include KeeLoq, AUT64, and Hitag2, all
of which have published cryptanalytic attacks. The team should test
whether the target system is vulnerable to known attacks against these
schemes, including algebraic attacks against KeeLoq, replay of
resynchronization sequences, and jam-and-replay attacks where the
attacker jams the legitimate signal while capturing the rolling code
for later use.

#### ***FHSS (Frequency Hopping Spread Spectrum) Analysis***

Some IoT devices and protocols use frequency hopping to improve
reliability and resistance to interference. The IoTA team should be
prepared to characterize hopping patterns, identify synchronization
mechanisms, and reassemble hopped transmissions into coherent data
streams. Wideband SDR capture (using hardware such as HackRF, USRP,
or LimeSDR) combined with custom GNU Radio flow graphs or
protocol-specific tools enables analysis of FHSS systems. The team
should document the hopping sequence, dwell time, and any
predictability in the hopping pattern that could enable targeted
interception or jamming.

#### ***Wideband Capture and Correlation***

Modern SDR hardware enables simultaneous capture across wide frequency
bands (20 MHz or more of instantaneous bandwidth). The IoTA team should
leverage wideband capture techniques for parallel channel monitoring,
identifying all RF emissions from a target device across multiple
frequency bands simultaneously. This approach can reveal hidden
communication channels, backup or fallback frequencies, and
correlations between transmissions on different bands that may not be
apparent when monitoring individual channels sequentially. SigMF
(Signal Metadata Format) should be used for documenting and sharing
RF captures to enable reproducible analysis and collaboration.


### BLE (Bluetooth Low Energy) Specific Testing

Bluetooth Low Energy has become one of the most prevalent wireless
protocols in the IoT ecosystem, used for device commissioning, data
transfer, proximity detection, and ongoing device control. The
proliferation of BLE 5.x has introduced new capabilities including
direction finding, extended advertising, and higher throughput, each of
which expands the attack surface. The IoTA team should evaluate BLE
implementations with the following considerations:

#### ***Advertisement Analysis and Device Fingerprinting***

The IoTA team should capture and analyze BLE advertising packets using
tools such as Sniffle (nRF52840/CC1352-based, the current best-of-breed
BLE 5.x sniffer replacing the end-of-life Ubertooth), nRF Connect, or
Bettercap. Advertising data should be examined for manufacturer-specific
data fields, service UUIDs, device names, and other identifying
information. Passive device fingerprinting through Company ID fields and
advertising payload patterns should be evaluated to determine whether
devices can be identified, tracked, or profiled without an active
connection. The team should assess whether the device implements BLE
address randomization (Resolvable Private Addresses) and whether the
randomization can be correlated across sessions through advertising data
content, timing patterns, or other side channels.

#### ***GATT/ATT Service Enumeration***

Once a BLE connection is established, the IoTA team should enumerate all
exposed GATT (Generic Attribute Profile) services and characteristics.
Each characteristic should be evaluated for its access permissions (read,
write, notify, indicate) and whether sensitive characteristics require
proper authentication before access is granted. Writable characteristics
that control device behavior (such as configuration parameters, firmware
update triggers, or actuator controls) should be tested for unauthorized
access. Tools such as nRF Connect, GATTacker, and Bettercap's BLE
modules facilitate this analysis.

#### ***BLE Pairing and Bonding Security***

The security of the BLE pairing process depends heavily on the pairing
method used. The IoTA team should evaluate whether the device uses Just
Works pairing (which provides no protection against man-in-the-middle
attacks), Passkey Entry (which can be vulnerable to brute-forcing the
6-digit passkey), Numeric Comparison, or Out-of-Band (OOB) pairing.
The team should test for pairing downgrade attacks where an attacker
forces a less secure pairing method, and should evaluate whether bonding
(long-term key storage) is implemented securely. Tools such as btlejack
and Sniffle support active BLE connection hijacking and MITM testing.

#### ***BLE Relay and Proximity Attacks***

For devices that use BLE for proximity-based authentication or access
control (door locks, vehicle access, payment terminals), the IoTA team
should evaluate susceptibility to relay attacks where the BLE connection
is transparently proxied over a longer distance. This is particularly
relevant for BLE-based digital car keys and smart locks.

### Meshtastic and LoRa Mesh Specific Testing

Meshtastic is an open-source, decentralized mesh networking platform
built on LoRa radio technology, designed for long-range, low-power
communication without cellular or internet infrastructure. Its rapid
adoption for emergency communications, outdoor activities, and community
mesh networks has made it a significant and growing component of the IoT
landscape. Recent security research has revealed critical vulnerabilities
in Meshtastic implementations that the IoTA team should evaluate.

#### ***Channel Key Security***

Meshtastic encrypts channel traffic using AES-256-CTR, but the default
primary channel uses a well-known default key ("AQ=="). The IoTA team
should evaluate whether deployed devices have changed the default channel
key, and should test whether custom channel keys can be recovered through
device compromise, traffic analysis, or exploitation of adjacent
vulnerabilities. Since channel keys are symmetric and shared among all
members of a channel, compromise of any single node on the channel
exposes all traffic.

#### ***Public Key Cryptography Implementation***

Meshtastic Direct Messages use X25519 key exchange for end-to-end
encryption. The IoTA team should evaluate the entropy of generated key
pairs, as CVE-2025-52464 (CVSSv4 9.5) demonstrated that vendor cloning
practices and insufficient entropy in the rweather/crypto library
resulted in duplicated private keys across entire production batches.
The team should test whether target devices are running firmware
versions affected by this vulnerability (versions 2.5.0 through 2.6.10)
and whether key material has been properly regenerated after patching.

#### ***Packet Injection and Protocol Exploitation***

The IoTA team should test whether malformed Protocol Buffers (protobuf)
packets can trigger memory corruption vulnerabilities. CVE-2025-24797
demonstrated that invalid protobuf data in mesh packets could cause an
attacker-controlled buffer overflow, allowing hijacking of execution
flow without authentication or user interaction on any device actively
rebroadcasting packets on the default mesh channel. The team should
also evaluate whether DM spoofing is possible through exploitation of
the pki_encrypted flag handling, where messages encrypted with the shared
channel key can be presented to the recipient as PKC-encrypted direct
messages, enabling impersonation of any node on the network.

#### ***MQTT Bridge Security***

Many Meshtastic deployments bridge mesh traffic to MQTT for internet
connectivity and data logging. The IoTA team should evaluate MQTT broker
authentication, topic access control lists (ACLs), TLS configuration,
and whether sensitive mesh traffic (including decrypted messages and
node telemetry) is exposed to unauthorized MQTT subscribers.

#### ***Node Impersonation and Remote Administration***

The IoTA team should test whether compromised key material allows
unauthorized remote administrative control of mesh nodes. Devices with
compromised X25519 key pairs are vulnerable to an attacker deriving the
shared secret using a known administrator's public key, enabling full
command execution on the target node. The team should evaluate the
robustness of the remote administration authentication model and test
for abuse of administrative commands.

#### ***Meshtastic Hardware Platform Security***

The IoTA team should evaluate the firmware security of popular
Meshtastic hardware platforms, including Heltec (LoRa 32 series),
LILYGO T-Beam, and RAK WisBlock modules. Evaluation should cover
boot security (whether secure boot is enabled or available on the
ESP32 or nRF52 SoC used), debug interface exposure (whether JTAG/SWD
is accessible and whether read-out protection is enabled), and flash
encryption status. Many Meshtastic devices ship with debug interfaces
fully accessible and no flash encryption, meaning that physical access
to the device provides immediate access to all stored key material
including the device's X25519 private key and any configured channel
encryption keys.

### Matter and Thread Specific Testing

Matter (formerly Project CHIP) is the Connectivity Standards Alliance's
unified smart home and IoT standard, supported by Apple, Google, Amazon,
Samsung, and hundreds of other manufacturers. Matter operates as an
application layer protocol over Thread (for low-power mesh devices),
Wi-Fi, and Ethernet, with BLE used for device commissioning. Thread is
an IEEE 802.15.4-based mesh networking protocol providing the low-power
transport layer for Matter devices. The security of Matter and Thread
implementations is critical as these protocols are rapidly becoming the
dominant standard for consumer and enterprise IoT.

#### ***Commissioning Security (PASE)***

The Matter commissioning process uses the Password-Authenticated Session
Establishment (PASE) protocol, which is based on SPAKE2+ with
PBKDF2-HMAC-SHA256 key derivation. The IoTA team should evaluate the
brute-force resistance of the setup passcode by verifying that the
specification's 20-attempt limit and 60-second commissioning window are
properly enforced by the device's SDK implementation. Research has
demonstrated that the official Matter JavaScript SDK failed to enforce
both limits despite logging that the threshold had been reached. The
team should also evaluate the PBKDF salt configuration, as the Matter
specification uses a hardcoded salt value, enabling pre-computation of
rainbow tables that can be used to recover passcodes from intercepted
commissioning exchanges efficiently.

#### ***Device Attestation Certificate (DAC) Security***

The Matter Device Attestation Certificate provides cryptographic proof
that a device is legitimate, certified, and compliant with Matter
standards. The IoTA team should evaluate the physical security of DAC
private keys on the target device. Research by Nozomi Networks
demonstrated successful extraction of DAC private keys from commercial
Matter devices by exploiting fault injection to unlock debug interfaces,
dumping flash memory, and reversing custom key obfuscation schemes. DAC
key compromise enables device cloning, allowing counterfeit devices to
join Matter networks and potentially access resources through the
compromised device's Access Control List (ACL) policies.

#### ***Operational Security (CASE)***

Once commissioned, Matter devices establish operational sessions using
the Certificate-Authenticated Session Establishment (CASE) protocol.
The IoTA team should evaluate CASE session establishment for replay
attacks, session hijacking, and proper certificate chain validation.
The team should also assess whether the device properly validates
certificate revocation status and handles expired certificates.

#### ***Thread Network Security***

The IoTA team should evaluate the security of the Thread network
underlying Matter device communications, including: the network-wide
key (used for MLE and MAC-layer encryption), commissioning credentials,
and Border Router key management. The team should test whether the
Thread network key can be extracted from a compromised device, whether
the key rotation mechanism is properly implemented, and whether a
compromised device can be used to intercept or inject traffic destined
for other devices on the Thread network.

#### ***Multi-Fabric and ACL Security***

Matter devices can participate in multiple fabrics simultaneously, each
with its own root of trust and administrative domain. The IoTA team
should test for cross-fabric information leakage and privilege
escalation. ACL policies should be evaluated to verify that devices
properly restrict operations based on the requesting fabric and
administrator's privilege level.

### Cellular IoT Specific Testing

The proliferation of cellular IoT protocols (NB-IoT, LTE-M, 4G LTE,
and 5G NR) has connected billions of devices to wide-area networks.
Cellular connectivity introduces unique attack surfaces related to
baseband modem security, SIM/eSIM management, and the interaction
between the device and carrier infrastructure.

#### ***Baseband Modem Analysis***

The IoTA team should identify the cellular modem chipset used by the
target device (common manufacturers include Quectel, Sierra Wireless,
u-blox, Nordic Semiconductor, Sequans, and Telit) and evaluate the AT
command interface for security weaknesses. Testing should include
enumeration of supported AT commands (including undocumented or
vendor-specific commands), evaluation of AT command access controls,
and testing for command injection through the AT interface. Undocumented
AT commands can sometimes allow configuration manipulation, baseband
access, SIM operations, or information disclosure beyond the intended
device functionality.

#### ***SIM and eSIM Security***

The IoTA team should evaluate SIM provisioning and management security,
including: whether the device is susceptible to SIM swap attacks,
whether eSIM profile management can be manipulated to redirect
connectivity, and whether the device properly validates the carrier
network it connects to. For devices using embedded SIMs, the security
of the remote SIM provisioning (RSP) infrastructure should be
evaluated. Tools such as SIMtrace 2 (for tracing SIM-modem
communication) and programmable SIM cards (sysmoISIM-SJA5) facilitate
this analysis.

#### ***Protocol Downgrade Attacks***

The IoTA team should test whether the target device can be forced to
downgrade from 5G/LTE to 3G or even 2G using rogue base stations built
with tools such as srsRAN, Open5GS, or OpenBTS (which require a
capable SDR such as the USRP B210). Downgrade to older cellular
generations exposes the device to well-documented vulnerabilities in
GSM/GPRS authentication and encryption.

#### ***Power Saving Mode and eDRX Security***

NB-IoT and LTE-M devices commonly use Power Saving Mode (PSM) and
Extended Discontinuous Reception (eDRX) to conserve battery life.
During PSM, the device is essentially unreachable by the network and
only communicates during brief wake windows. The IoTA team should
evaluate the security implications of these power-saving mechanisms,
including whether a device in deep sleep can be manipulated during
its wake windows (when it is briefly reachable), whether the
transition from PSM to active mode introduces a window where security
checks are relaxed or deferred, and whether an attacker can manipulate
PSM/eDRX timing parameters to force a device into a more
power-consumptive and continuously reachable state (effectively a
denial-of-service attack against battery-powered devices).

#### ***Data Exfiltration via Cellular***

The IoTA team should evaluate whether a compromised cellular IoT
device can be used to exfiltrate data from the local environment
through its cellular connection. A cellular link provides an
out-of-band communication channel that bypasses all local network
security controls (firewalls, IDS, proxy servers, and network
monitoring). For devices deployed in sensitive environments, the team
should assess whether the device can be reprogrammed or commanded to
establish a reverse tunnel over the cellular link, providing an
attacker with persistent remote access that is invisible to the
local network security infrastructure.

#### ***GNSS Security***

For cellular IoT devices with GPS/GNSS capability (common in asset
trackers, fleet management devices, and geofenced industrial
equipment), the IoTA team should evaluate susceptibility to GNSS
spoofing and jamming. Location spoofing can have operational
consequences for devices that make decisions based on their geographic
position, such as geofenced operational restrictions or location-based
access controls.
