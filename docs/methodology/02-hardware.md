[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Hardware specific**

### Direct Tampering

Tamper-protection mechanisms are intended to protect against malicious
modification of an IoT device, and should be part of the IoTA Red Team
analysis. As part of a defense-in-depth component, the tamperproof
mechanisms represent an opportunity for a vendor to protect the consumer
and vendor from undesirable system modification.

With enough experimentation, tamper-protection mechanisms will
inevitably fail. As with most defenses, the goal must be to afford
significant response time and detection ability preventing devices from
being misused in an avenue to attack backend systems. Appropriate
tamper-protections will delay an attacker from fully compromising the
integrity of backend systems, while possibly forcing attackers to obtain
multiple IoT devices.

Due to the overall cost of IoT devices, tamper protection is not always
a consideration. This is especially true when the vendor intends to
support open modification of an IoT device, allowing it to be used
outside of its intended purpose

Tamper-protection mechanisms which should be evaluated include:

-   Remote tamper detection systems, where an IoT device can remotely notify the manufacturer office that someone is tampering with a device;

-   System integrity protection systems, where an IoT device can protect the integrity of a system, including self-erasure of keys and firmware;

-   Intended repair modes, used for authorized repair personnel from the manufacturer, company or vendor;

-   Security of physical locks.

Vendors and utilities must assume that an attacker has physical
possession of an IoT Device, either through sale from the vendor being
targeted, or as the result of theft from a customer premise, or through
purchase through an authorized retailer. Based on the analysis of the
first IoT device, the attacker will be better prepared to compromise a
second device.

Unlike real-world adversaries, a contracted analysis team must maintain
a delicate balance of time and monetary resources. If an analysis team
is unable to devote the required time to successfully compromise the
tamper-protections, it is in the vendor or utility's best interest to
provide devices without tamper-protections. The analysis should report
the effectiveness of the tamper-protections, and move on to deeper
layers of analysis. Assuming an attacker with enough time and IoT
Devices can defeat the tamper-protection methods, the rest of the IoT
device still requires analysis.

### Removal of Potting, conformal coating

Some electronic circuit boards have potting or conformal coating on the
PCB, board components or conductors to prevent corrosion or to prevent
damage from vibration or impact. For the IoTA team, this coating can
inhibit testing activities by preventing conduction to probes or by
preventing access to components. The use of chemical strippers, heat or
brute force methods can sometimes be used to remove these coatings to
gain access to the underlying PCB or component. However, it should be
noted that some of these methods have a high risk of damaging the device
and can also produce toxic vapors which should be extracted in a proper
lab environment for the health and safety of the IoTA team.

If these coatings cannot be removed within an appropriate time period or
if there is a high risk of monetary loss or damage to the device, it is
in the vendors best interest to provide a testing device without these
protective coatings to the IoTA team so that they can continue to the
next phase of analysis. A vendor or utility should always work under the
assumption that an adversary would have the appropriate time and ability
to remove these coatings from a device, and that they should not be
treated as a form of security control.

However, in cases where conformal coating is present, simple tools can
be employed to remove it for areas that need to be tested via:

-   Chemical removal - several inexpensive, readily available chemicals
    are available through online retailers (some in the form of a pen)
    that are useful for dissolving the conformal compound from specific
    areas of the PCB in which you wish to interact with, as opposed to
    removing it from the PCB in its entirety. Care should be taken with
    the use of these chemicals, and the manufacturer's instructions
    should be followed for their use.

-   Abrasion - several abrasive methods can be employed to remove the
    conformal coating, such as scraping with a pick or blade, abrasion
    with medium to fine grit sandpaper, or even abrasion with a steel or
    brass wire brush. These methods are applied to specific, targeted
    areas of the PCB, as opposed to its entirety. Extreme care should be
    used when utilizing abrasive methods, as over application can cause
    serious, irreparable damage to surrounding components and PCB
    traces.

### Improper Cryptography

While it is often trivial to identify the absence of cryptography it 
is significantly more difficult to detect cryptography which is 
present but improperly used.

Consider, for example, the Debian/OpenSSL vulnerability in which the
OpenSSL RNG was mistakenly crippled\[JF2\]. This critical
vulnerability limited the number of possible keys of a given type to
fewer than forty thousand, a number sufficiently small for an attacker
to generate and store all possible keys in advance. Further, this
issue went unnoticed for two years before being repaired, allowing
many organizations to deploy systems with vulnerable keys unaware of
their risk and exposure. Despite later patches that resolved the RNG
flaw, users who have not replaced all keys generated with the flawed
software remain vulnerable.

Throughout the analysis of IoT-related technology, the IoTA Red Team
should evaluate multiple cryptographic mistakes that would otherwise
threaten the integrity of the system.

#### ***Weak Key Derivation***

Many cryptographic algorithms require unpredictable data as an input
to the key derivation functions responsible for creating symmetric or
asymmetric keys. Without sufficiently random content as an input, all
keys used by the algorithm are suspect, allowing an attacker who can
reproduce the input data to reproduce keys and decrypt data or
otherwise impersonate trusted devices. Weak key derivation has been
observed in cryptographic implementation-flaws such as the
OpenSSL/Debian flaw described earlier, as well as algorithmic flaws in
the DES, Blowfish and RC4 ciphers.

The IoTA Red Team should evaluate the input values used for deriving
keys at device initialization time, measuring the entropy of input
data and evaluating the Chi-square test results to evaluate the
randomness of data.

#### ***Improper Reuse of Keystream Data***

In stream ciphers, key stream data cannot be re-used without
threatening the integrity of the cryptosystem. For performance reasons
and through implementation mistakes, past cryptographic protocol
implementations have reused key stream data, thereby allowing an
attacker who observes a plaintext/ciphertext pair to recover the
plaintext of an unknown ciphertext value. This flaw was identified in
late version of the Windows NT operating system, allowing an attacker
to decrypt locally stored passwords.

This vulnerability can also extend to block ciphers in Cipher Block
Chaining (CBC) mode where the initial initialization vector is
re-used, although this is generally less common.

The IoTA Red Team should evaluate the use of key stream data to
identify inappropriate re-use, identifying areas of protocols and
system components that are threatened by this flaw.

#### ***Lack of Replay Protection***

Some cryptographic primitives accept received data as valid after
checking that the data decrypts properly. Without additional
verification functions, such as sequence enforcement, the algorithm is
vulnerable to replay attacks, where an attacker can capture a valid
encrypted data stream and retransmit the content. This vulnerability
affects both stream ciphers and some modes of block ciphers, and has
been observed in many cases including an implementation flaw in the
FreeBSD IPsec stack where an attacker who observes encrypted data may
retransmit the data repeatedly, potentially manipulating the source
and destination systems

The analyst can identify systems vulnerable to replay attacks by
identifying the lack of unique identifiers in each cryptographic
frame, or by observing repeated ciphertext content transmitted by one
or more sources. Active analysis can also be used to evaluate replay
attack vulnerabilities by replaying valid cryptographic data and
observing the system state and operation after receiving the replayed
data.

#### ***Insecure Cipher Modes***

While some encryption algorithms are considered secure, such as the
Advanced Encryption System (AES) cipher, the use of a cipher in an
insecure mode can threaten the integrity of the cryptosystem. For
example, the AES Electronic Cookbook (ECB) mode is considered weak,
allowing an attacker to deduce repetitious plaintext content from
repeated ciphertext blocks.

Through the analysis of firmware and cryptographic chips, the IoTA Red
Team should identify ciphersuite modes used in IoT technology,
identifying insecure modes that represent a threat to the system.

#### ***Weak Integrity Protection***

Many encryption algorithms, particularly stream ciphers, do not
validate the content of decrypted content without a separate integrity
check function. By transmitting an integrity check value (ICV)
associated with the plaintext content, the receiving station can
decrypt data and validate the resulting plaintext against the observed
ICV.

The use of weak integrity check functions allows an attacker to
manipulate ciphertext data, allowing them to selectively modify data
while preserving a valid ICV (so-called \"bit flipping\" attacks), and
may allow an attacker to decrypt ciphertext without knowledge of the
encryption key. These vulnerabilities have been observed in the
Temporal Key Integrity Protocol (TKIP) used by the IEEE 802.11i
protocol for wireless networks, allowing an attacker to decrypt
arbitrary frames.

#### ***Weak or Missing Authentication Mechanism***

A simple implementation of a strong cipher such as AES with an
accepted cipher mode such as cipher block chaining (CBC) can still
result in an attacker sending or modifying commands or data with no
way to confirm the authenticity of that information.

Use of trusted asymmetric keys to establish an initial communication
over which new keys are negotiated can reduce the chances of this
happening by ensuring that a different key is used for each session.
This can be computationally expensive, however, and a serious problem
for low-power devices running on limited batteries. The use of
authenticated encryption with associated data (AEAD) encryption
mechanisms, such as use of the GCM and OCB modes, can reduce the
overhead associated with these connections while still allowing strong
encryption.

Establishing a proper trust relationship can be exceptionally tricky,
however. For example, without an Internet connection, there is no way
to see if a certificate has been revoked. Even with an Internet
connection, CRL/OCSP requests can be blocked, and the default behavior
is usually to proceed without the response. Improperly protected
certificate stores can be overwritten, while read-only stores can
never be updated.

Ultimately, most IoT devices involve relatively trivial physical
access of at least an example device, so recovering critical
cryptographic material---which is often duplicated across much or even
all of a device's production---often turns into a relatively trivial
exercise.

#### ***Insufficient Key Length***

Encryption ciphers may prove to provide inadequate protection against
an attacker if an insufficient key length is used. This is most
notable in symmetric ciphers such as the Data Encryption Standard
(DES) where an attacker with approximately \$15,000/USD in available
computing resources can recover a DES key in approximately 12 hours.
Asymmetric ciphers are also vulnerable to insufficient key length
attacks, where, at the time of this writing, it is possible to factor
the prime numbers used for RSA 512-bit encryption within one month.

The IoTA Red Team should identify the key lengths used for both
symmetric and asymmetric protocols. The appropriateness of the key
lengths will be evaluated based on the determined resources of a
potential adversary, discussed in Section.

#### ***Cryptographically Weak Initialization Vectors***

Stream ciphers including the RC4 algorithm have a key stream
generation weakness when cryptographically weak initialization vectors
(IVs) are used. If an attacker is able to identify consistent known
plaintext in frames (such as protocol header information) and can
collect a group of cryptographically weak IVs, they may be able to
recover the encryption key used to protect data with fewer operations
than the entire keyspace bounds.

The RC4 weakness in the selection of IVs was first publicized by Scott
Fluhrer, Itsik Mantin and Adi Shamir ., describing a vulnerability in the Wired Equivalence Protocol
(WEP), once part of the IEEE 802.11 specification. This flaw was later
widely exploited by tools such as the Aircrack-ng suite, a contributing factor to an attack
against a US-based retailer which revealed payment card data for 45.7
million customers.

The IoTA Red Team should evaluate IoT technology for the use of stream
ciphers, investigating the length and section of IV values.

### Insecure primary interfaces (UART, RS232, RS485, CAN, USB, etc.)

IoT devices use a variety of different interfaces and connectors to
communicate both with each other and with the outside world. The IoTA
team should investigate each device interface for its function as well
as the data that is transmitted through it. In some cases an exposed
UART interface may be used to gain shell access to an embedded linux
system. In other cases exposed USB ports may be used to attach other
interface devices. For example, laptop or desktop computers, keyboards,
mice, ethernet adapters or external drives could all be attached to a
device via USB interfaces, each of which have the potential to provide
an attacker with the means to cause unexpected behavior or even provide
unintended access to a system. Wireshark and a good logic analyser or
oscilloscope can provide valuable insight into the types of data sent
through these buses and what kinds of interception or modification of
those signals is possible.

Beyond these traditional interfaces, the IoTA team should evaluate the
following modern interfaces commonly found in current IoT devices:

CAN-FD (Controller Area Network with Flexible Data rate) has largely
replaced classic CAN in automotive and industrial IoT. CAN-FD supports
higher data rates (up to 8 Mbps vs. 1 Mbps for classic CAN) and larger
payload sizes (64 bytes vs. 8 bytes). Testing must account for these
differences in data rate, payload handling, and arbitration behavior.
Tools such as python-can, SavvyCAN, and CANalyze support CAN-FD analysis.

Automotive Ethernet (100BASE-T1 and 1000BASE-T1) uses a single twisted
pair with PAM3 encoding, requiring different tapping methodology than
standard Ethernet. Automotive Ethernet carries higher-layer protocols
such as SOME/IP (Scalable service-Oriented MiddlewarE over IP) and DoIP
(Diagnostics over IP), each of which presents its own vulnerability
surface. Specialized tapping hardware from vendors like Technica
Engineering or Intrepid Control Systems may be required to access these
interfaces non-destructively.

SWD (Serial Wire Debug) is the predominant debug interface for ARM
Cortex-M based devices, using only two pins (SWDIO and SWCLK) plus
ground. The methodology should cover SWD enumeration using tools like
JTAGenum and OpenOCD alongside traditional JTAG, as many modern IoT
SoCs expose SWD rather than full JTAG.

USB Type-C and USB Power Delivery (PD) are increasingly common in IoT
devices. The IoTA team should evaluate USB-C implementations for PD
voltage manipulation attacks, unauthorized accessory mode enumeration,
and potential exploitation of the USB-C Configuration Channel (CC)
for device fingerprinting or attack.

SD/eMMC/UFS interfaces have largely replaced SPI flash in modern SoCs.
Many current IoT devices boot from eMMC or UFS storage, requiring
different extraction techniques than SPI flash dumping. ISP
(in-system programming) via CMD0/CMD1 sequences, eMMC adapter boards,
and chip-off techniques for BGA packages are relevant approaches.

PCIe/M.2 interfaces appear in higher-end IoT gateways and edge compute
devices. These interfaces may be vulnerable to DMA (Direct Memory
Access) attacks that can read or write system memory without CPU
involvement.

### Insecure internal and external buses

Embedded systems commonly use peripheral devices such as radios or
EEPROM chips, interfacing with microcontrollers through SPI, I2C or
other types of serial bus interfaces. Although this is a convenient and
industry standard method for interfacing peripheral devices with each
other, it represents a security risk for a device with little or no
physical protection. For example, the use of two electrical probes
constructed from medical syringes connected to a protocol adapter to
extract the firmware from an EEPROM device over an I2C bus. The
extracted EEPROM data could contain executable code, configuration
information or cryptographic keys, each of which could be stolen or
modified.

From a security perspective, it is helpful for developers to evaluate
the potential gains for an attacker through bus snooping. While many
hardware engineers would recognize the risk of using external memory to
boot a secure device, the same engineers use an insecure serial bus to
connect a radio chip to a microcontroller. Many radio chip
manufacturers, in effort to reduce development costs and accelerate
platform adoption have implemented cryptographic algorithms internally
in hardware. Implementing cryptography in hardware, which would be
claimed as a security feature on many datasheets, has introduced a
vulnerability: traffic between the microcontroller and the radio is left
unencrypted on the bus. Using a bus sniffer, an attacker is
free to passively read information from the bus in an attempt to capture
sensitive information.

In order to sniff packets on the network, an attacker must simply
connect an appropriate bus sniffer to the serial interface between the
microcontroller and the radio. By capturing the data transmitted over
this interface, an attacker is able to observe all communications
between the two peripherals, capturing radio configuration information,
cryptographic keys, network authentication credentials and other
sensitive data. This collected data can then be used on third-party
devices to extend the attacker's access into the target network.

Alternatively, an attacker could manipulate the target network by
injecting new packets onto the bus. This provides them with a reliable
communications mechanism to actively participate in the network, where
any data originates from a legitimate node on the network. Through this
mechanism, the attacker may choose to exploit any trust relationships
established with the victim device, to deliver manipulated frames
intended to exploit other systems, or for the manipulation of other
services supporting the network and services it provides.

As a defense against this attack, several unified radio chips
manufacturers have produced technology which includes both the
microcontroller and radio within the same physical package. These chips,
which include the TI CC2480A1, Freescale MC1322X, and Ember EM250, limit
the effectiveness of this style of attack by protecting the bus
connecting the radio to the microcontroller. However, many radios
include hardware-sniffing or similar functionality into the chip, and
these unified chips may still communicate with other devices over
exposed data buses. The Ember chip documentation recommends enabling
sniffer modes and pins by default for rapid debugging and analysis,
providing detailed register settings required to do so.

***Fuzzing***

Fuzzing is the art of interacting with an application in unorthodox and
sometimes random ways often causing a vulnerable application to crash or
otherwise behave incorrectly. Fuzzers often inject increasingly large
data into buffers attempting to cause an overflow, manipulate numbers in
protocol fields to cause an overflow or underflow condition, and/or
insert special characters into various fields hoping to cause some
unexpected control sequence. Fuzzing should be performed wherever
interaction with the target system is allowed, including:

-   Network interaction over radio interfaces;

-   Direct system bus interaction;

-   Local infrared management ports;

-   Local serial management ports.

While vulnerabilities which can be remotely exploited are the highest
value due to the scale of attack potential, vulnerabilities which
require touching an individual IoT Device are also valuable.

### Glitching Attacks

#### ***Power-Glitching Attacks***

Due to the operating characteristics of microcontrollers, manipulating
the power input to a microcontroller may cause it to behave
inappropriately. Carefully executed, an attacker can manipulate this
behavior in a predictable fashion. For instance, providing slightly
less than the required power for a given microcontroller to operate
may allow the microcontroller's instruction pointer to increment but
not perform the instruction.

An attacker may leverage a power-glitching attack to manipulate the
system microcontroller to skip authentication failure processing
routines or other undesirable instructions, granting access to IoT
devices and resources without adequate authentication credentials.

#### ***Clock-Glitching Attacks***

If a microcontroller is configured for use with an external clock, an
attacker may be able to manipulate the system behavior to their
benefit. For example, if an attacker forces two clock signals at a
rate that is slightly faster than the target microcontroller can
accommodate, they may be able to manipulate the system to skip the
execution of a particular instruction. In this technique, an attacker
may use a digital I/O pin from an "attack" microcontroller to provide
the accelerated clock for the target microcontroller.

Similar to power-glitching, this technique may provide the ability to
skip failed-authentication routines or other undesirable instructions,
granting the attacker greater control over the system.

#### ***NAND-Glitching Attacks***

If a microcontroller utilizes external NAND flash memory, an attacker
may be able to manipulate the behavior of the boot process, leading to
compromise of the system. For example, if an attacker grounds one of
the NAND flash IO pins during the boot sequence of a system utilizing
U-Boot, the failure condition may expose the U-Boot command prompt to
the adversary. By altering the boot arguments passed to the embedded
Linux kernel, an attacker could then force the device to boot directly
into a shell or perform an action under their control.

### Firmware dumping 

#### ***EEPROM Dumping***

Serial flash and EEPROM memory devices on a circuit board provide no
means of protection, and they are thus immediately available to any
attacker who cares to dump them with the equipment described in
Section. The team will review the use of such devices, manipulating
them by a variety of methods to extract interesting data.

The simplest of these attacks to perform, involves the use of two
electric probes and a shared ground to dump an I2C EEPROM's
contents. The multi-master features of I2C allow this to be
performed while the device is still active. The two probes, modified
hypodermic syringes, are tapping the Serial Data (SDA) and Serial
Clock (SCL) lines.

SPI is more difficult to tap, yet not insurmountable. Because it lacks
the multi-master mode of I2C, any attempt to read or write the
memory chip will result in interference from the master
microcontroller. Three methods exist by which the memory chip may be
accessed without the microcontroller's interference.

First, the EEPROM may be desoldered by use of the SMD rework station
described in Section. Once removed, it can be soldered to a fresh
board or connected to a ZIF socket and its contents extracted to a
computer. While this method is certainly among the easiest, two
alternative methods are available, each with a discrete advantage.

The second method is a passive tap, in which the MISO and MOSI lines
are observed but not modified. In this configuration, memory requests
are recorded. The analyst has the advantage of knowing the sequence of
memory fetches and writes, but it is inconvenient for the tester to
change a request.

A third method involves lifting the CE (Chip Enable) pin of either the
microcontroller or the memory chip. This allows the chip to be
disabled temporarily, causing I/O pins to switch to a high impedance
mode. By lifting the CE pin of a memory chip and soldering another
into the bus, the attacker can temporarily replace a surface-mount
chip with a ZIF socket holding a DIP chip that can be quickly replaced
for experimentation.

#### ***Exposed Debug/Programming Interfaces and Internal Microcontroller Memory***

In addition to the valuable information found in external, serial
EEPROM chips, many microcontrollers and system-on-chip (SOC) devices
hold very useful information. In some ways, this data is more valuable
in that often it is the only source of executable code. Due to the
increased complexity of these devices and less standardization between
vendors, many different interfaces exist for programming and debugging
these chips. The following are some of the most common.

**JTAG**, also known as the Joint Test Action Group or IEEE 1149.1, is
a protocol for accessing test-points within an ASIC chip, debugging a
microcontroller, and programming the memory of a device, such as a
microcontroller or an FPGA. While most microcontrollers have a JTAG
fuse which can be blown to disable debugging access, many
manufacturers leave the fuse intact for debugging purposes. The fuse
itself might be physical, in which case it can be bypassed with an
invasive micro-electronic probe station. It might also be an EEPROM
cell, in which case semi-invasive optical attacks can often reset the
chip to its unprotected state. With access to the JTAG interface, the
attack team can utilize the appropriate Flash Emulation Tool (FET) to
retrieve the contents of volatile and non-volatile memory for further
analysis. An example of the MSP Flash Emulation Toolkit used for
retrieving the firmware of a MSP430 device is shown in.

**Spy-Bi-Wire** and similar protocols are variants of JTAG which
require fewer wires. This is made possible by the use of bidirectional
I/O. Spy-Bi-Wire is popular in newer TI chipsets.

**Serial Bootstrap Loaders** are a software method by which
microcontrollers may be programmed. Rather than requiring custom
testing hardware, such as that used by JTAG, a bootloader is placed
within the permanent masked ROM of each chip. The chip can then be
programmed with little more than a level-converter and a serial port.
Bootloaders are often vulnerable to timing attacks which allow code to
be forcibly extracted. Further, voltage-glitching attacks can be used
to skip individual instructions of the bootloader code, allowing
software protection measures to be bypassed. Serial Bootstrap Loaders
are common in many microcontrollers, and have even been added to some
architectures which do not include them from the factory (e.g. AVR
ATmega processors when used in Arduino toolsets).

**I^2^C, SPI** and other protocols are sometimes used for the
programming of microcontrollers. Such chips rarely offer a
code-protection feature. InCircuit Serial Programmers (ICSP), common
in AVR and PIC microcontrollers, often use the SPI protocol and
wiring.

#### ***Obtaining Firmware from the Vendor***

IoT vendors often provide firmware on their websites for direct
download by customers and IT professionals. In cases where the
firmware location is not as obvious, a firmware URL can be discovered
by analyzing network traffic of the device in Wireshark or an
intercepting proxy like Burp Suite. A firmware URL (or even an
embedded version of the firmware) can also sometimes be found by
reverse engineering the associated Android or iOS application.

#### ***Modern Firmware Acquisition Techniques***

Beyond traditional methods, the IoTA team should consider the following
approaches for obtaining firmware from current-generation devices:

Fault injection against secure boot: Modern SoCs such as the ESP32,
STM32, and nRF series implement secure boot and read-out protection
(RDP on STM32, APPROTECT on nRF, Flash Encryption on ESP32). These
protections can sometimes be bypassed through voltage glitching, 
electromagnetic fault injection (EMFI), or laser fault injection. Tools
such as the ChipWhisperer Husky and PicoEMP provide accessible
platforms for these techniques.

JTAG/SWD alternatives: When traditional JTAG pins are not readily
identifiable, tools such as JTAGenum (Arduino-based pin brute-forcing),
blueTag (Bluetooth-enabled JTAG scanning), and the ESP32 Bus Pirate
can automate the process of discovering debug interfaces on targets
with unknown pinouts.

eMMC and UFS extraction: For devices using eMMC or UFS storage, ISP
(in-system programming) techniques can read the storage media through
its native interface using adapter boards. For BGA packages where
in-circuit reading is impractical, controlled chip removal (chip-off)
using hot air rework stations with temperature profiling may be
necessary. Reballing and adapter boards allow the removed chip to be
read externally.

Firmware update interception: Modern IoT devices receive updates through
various channels including MQTT-based OTA delivery, CoAP firmware
endpoints, and LwM2M firmware update protocol. The IoTA team should
intercept and analyze firmware update traffic to obtain firmware images
and evaluate the security of the update delivery mechanism itself.

### Static & Dynamic Firmware Analysis

#### ***Weak password policies***

The team should analyze the password policies of the device to ensure
that strong passwords of sufficient length and complexity are required
and that default passwords are required to be changed during initial
setup. Evaluation of password hashes discovered on the device should
also be performed to ensure that cryptographically strong hashing
algorithms with salts are used to prevent password cracking attempts.
The ability to utilize 2FA or MFA when accessing an IoT device should
also be assessed.

#### ***Hardcoded secrets in firmware***

The IoTA team should analyze the contents of a recovered firmware for
hard coded secrets in files and folders. Secrets may include
usernames, plaintext passwords, password hashes, certificates, private
keys, authentication tokens or other sensitive data. This type of
information could lead to compromise of the IoT device itself or other
systems in the related IoT ecosystem.

#### ***Firmware Signing and Integrity Verification***

The IoTA team should evaluate the firmware signing and integrity
verification mechanisms implemented by the device manufacturer. This
evaluation encompasses both the signing infrastructure (how firmware
images are signed before distribution) and the verification
implementation (how the device validates firmware before execution or
installation).

Signature Verification Analysis: The team should determine the
cryptographic algorithm used for firmware signing (RSA, ECDSA, EdDSA),
the key length, and the hash algorithm. The team should obtain a
legitimately signed firmware image and analyze the signature format,
location (appended to the image, in a separate file, embedded in a
header), and the verification code path in the bootloader or update
application. Where possible, the team should extract the public
verification key from the device firmware or hardware (OTP fuses,
secure element) and independently verify the signature against the
firmware image to confirm that the verification is implemented
correctly.

Firmware Modification and Repackaging: Once the signing scheme is
understood, the team should attempt to modify the firmware image and
have the device accept the modified version. Approaches include:

- No signing: If the firmware is not signed at all, any modification
  will be accepted. The team should unpack the firmware using tools
  such as binwalk, firmware-mod-kit, or unblob, make alterations
  (such as changing a visible text string, inserting a bind shell, or
  modifying a startup script), repack the firmware, and upload it to
  the device. Successful installation of unsigned modified firmware is
  a critical finding.

- Hash-only verification: Some devices verify firmware integrity using
  a hash (CRC32, MD5, SHA-256) without a cryptographic signature. If
  the hash is stored alongside or within the firmware image itself
  (rather than in a separate, immutable location), the team can
  modify the firmware and recalculate the hash, bypassing the
  integrity check entirely.

- Weak signature schemes: If the signing key can be extracted from the
  device (from firmware, a debug interface, or side-channel analysis),
  the team can sign arbitrary firmware images. Development or test
  keys left in production firmware are a common finding.

- Signature verification bypass: The team should attempt to bypass
  signature verification through the methods described in the Secure
  Boot Chain Analysis section (fault injection, bootloader
  vulnerabilities, alternative boot modes).

- Partial verification: Some implementations only verify a header or
  a subset of the firmware image, allowing modifications to unverified
  regions. The team should determine exactly which bytes of the
  firmware image are covered by the signature.

A word of caution for the IoTA team: Modifying a firmware image can
cause unintended consequences or result in the bricking of a device.
Ensure that a backup device is available or that the device has a
firmware recovery feature. Where possible, test firmware modifications
in an emulated environment before attempting them on physical hardware.

Firmware Update Channel Security: Beyond the signing of the firmware
image itself, the team should evaluate the entire update delivery
pipeline. This includes whether the device authenticates the update
server (does it validate the server's TLS certificate?), whether the
update metadata (version number, target device model, changelog) is
signed independently of the firmware payload, whether the device
accepts update commands from any source or only from authenticated
management endpoints, and whether the update process can be forced
or triggered by an attacker (for example, by spoofing a DNS response
to redirect the update check to an attacker-controlled server).

#### ***Simulation and Emulation***

Through the use of available system emulators, the attack team may
simulate the hardware of an IoT Device with extracted firmware to aid
in the analysis of firmware functionality and response to malformed
stimuli through fuzzing. By using an emulated environment, the attack
team will have access to more sophisticated monitoring and debugging
tools which may prove useful in the development and testing of
exploits in a controlled environment.

#### ***Vulnerable third party components***

File system analysis of the firmware can be performed to reveal the
use of vulnerable third party services, applications and libraries. If
public exploit code is available and an attacker can exploit one of
these services, it can lead to the initial compromise or privilege
escalation within an IoT device.

#### ***Custom scripts and binaries***

Analysis of custom scripts and binaries found within a firmware image
can provide the IoTA team with potential attack paths to gain initial
access or privilege execution on a device. Custom scripts can include
hardcoded values such as usernames and passwords or contain coding
mistakes potentially exploitable by an attacker.

Reverse engineering a binary executable may also reveal similar
hardcoded information and logic flaws or memory corruption
vulnerabilities that can lead to Denial of Service or Remote Code
Execution conditions. Tools like strings, IDA and Ghidra can provide
valuable information to the IoTA team, but may require significant
analyst time to complete.

#### ***File System Permissions***

Embedded operating systems on IoT devices are prone to the same types
of file system vulnerabilities often found in their desktop and server
counterparts. Analysis should be performed by the IoTA team to
determine if the confidentiality, integrity or availability of the
device can be negatively impacted by an adversary due to excessive
permissions granted on the file system.

#### ***Vulnerable configurations***

The IoTA team should analyze the device configurations (both default
and user configurable) for potential vulnerabilities and risks. For
example, a device with plaintext communication protocols such as
telnet enabled is at risk of MITM interception and exploitation.

#### ***Web applications, cgi-bin***

Many IoT devices contain embedded web applications used to interact
with or configure the device. This is convenient for developers and
end users alike, but opens a device up to all of the potential
vulnerabilities associated with typical web applications.
Additionally, when inspecting the raw web app source files or CGI
binaries extracted from the firmware, secrets can be obtained and
logic flaws can be observed for potential exploitation. While the
detailed specifics of performing a web application penetration test
and the associated vulnerabilities fall outside of the scope for this
document, embedded web applications are common in the IoT landscape
and should be tested thoroughly by the IoTA team.

#### ***SBOM Generation and Software Composition Analysis***

For every firmware image obtained during an engagement, the IoTA team
should generate a Software Bill of Materials (SBOM) using binary
analysis tools such as EMBA, FACT, or commercial platforms like
Finite State. The SBOM should document all identified components
including name, version, supplier or origin, license, and known CVE
associations, and should be produced in a machine-readable format such
as CycloneDX or SPDX. All identified components should be
cross-referenced against multiple vulnerability databases including
the National Vulnerability Database (NVD), the Open Source
Vulnerabilities database (OSV), vendor-specific advisories, and the
CISA Known Exploited Vulnerabilities (KEV) catalog. Dependency
analysis should map the complete dependency tree of identified
components, as transitive dependencies can introduce vulnerabilities
not apparent from top-level analysis alone. Residual build environment
artifacts (compiler strings, build paths, debug symbols, CI/CD
configuration fragments) should be documented as they may reveal
information about the vendor's development environment and supply
chain practices. SBOM generation is now a regulatory requirement under
the EU Cyber Resilience Act and should be a standard deliverable for
any IoT security assessment.

#### ***RTOS-Specific Analysis***

Many modern IoT devices run on real-time operating systems such as
FreeRTOS, Zephyr, Mbed OS, ThreadX/Azure RTOS, NuttX, or QNX rather
than embedded Linux. The testing methodology must account for the
fundamentally different security models of these platforms. Unlike
Linux-based systems, many RTOS implementations run all code in a
single address space without memory protection, meaning that a
vulnerability in any component can compromise the entire system.
Where memory protection is available, such as FreeRTOS task isolation
using MPU (Memory Protection Unit) regions or Zephyr's userspace/kernel
boundary, the IoTA team should evaluate whether these protections are
properly configured and enforced. The team should also evaluate
interrupt handler security, as RTOS interrupt service routines often
run in privileged mode and may be reachable through external
interfaces. Stack overflow and heap corruption vulnerabilities are
particularly dangerous in RTOS environments because they can
immediately compromise the entire system rather than being contained
by process isolation.

#### ***Secure Boot Chain Analysis***

The IoTA team should evaluate the complete boot chain from the ROM
bootloader through any secondary bootloaders to the application
firmware. A typical IoT secure boot chain consists of several stages,
each of which must be evaluated independently:

Stage 0 (Boot ROM): The immutable code in the SoC's ROM that executes
first after reset. This is the hardware root of trust. The team should
determine whether the Boot ROM validates the signature of the next
stage, what cryptographic algorithm and key length are used, and
whether the public key or its hash is stored in OTP (One-Time
Programmable) fuses or eFuses. On Espressif ESP32 devices, the
Secure Boot V2 scheme stores a SHA-256 digest of the RSA-3072 public
key in eFuse. On STM32 devices, the Root Security Services (RSS)
validate the initial firmware using keys stored in OTP. On Nordic
nRF devices, the Secure Boot (B0/B0n) scheme uses ECDSA-P256 with
the public key hash in OTP.

Stage 1 (Secondary Bootloader): Many devices use a secondary
bootloader (such as MCUboot, U-Boot, or a vendor-specific bootloader)
that provides more complex functionality including firmware slot
management, A/B partitioning, and update application. The team should
evaluate whether this stage properly validates the application
firmware before execution, whether the bootloader itself is signed and
validated by Stage 0, and whether the bootloader contains any debug
or development features that could be exploited (such as a serial
console that accepts unsigned firmware, or a DFU mode that bypasses
signature checks).

Stage 2 (Application Firmware): The main application firmware should
be signed with a production key. The team should verify that the
signature algorithm and key length provide adequate security (RSA-2048
is the minimum; RSA-3072 or ECDSA-P256/P384 is preferred), that
development or test keys are not present in production firmware, and
that the signature covers the entire firmware image (not just a header
or a subset of the image).

Rollback Protection: The team should evaluate whether anti-rollback
counters (monotonic counters stored in OTP fuses or secure storage)
prevent an attacker from reflashing an older firmware version that
contains known vulnerabilities. Lack of rollback protection is a
critical finding, as it allows an attacker with physical access to
downgrade the device to any previous firmware version.

Secure Boot Bypass Techniques: The team should attempt to bypass
secure boot through the following methods where applicable:

- Voltage glitching during signature verification (using ChipWhisperer
  Husky or similar fault injection hardware) to cause the verification
  function to return success despite an invalid signature.
- Electromagnetic fault injection (EMFI) targeting the signature
  verification code path (using PicoEMP or similar).
- Exploiting bootloader vulnerabilities (buffer overflows in image
  parsing, TOCTOU races between signature check and firmware
  execution).
- Debug interface re-enablement: On some platforms (notably older
  ESP32 revisions), fault injection can re-enable JTAG/SWD debug
  interfaces that were disabled by fuse configuration, providing
  full control over the boot process.
- Boot mode pin manipulation: Some SoCs enter alternative boot modes
  (UART boot, USB DFU, SD card boot) when specific pins are held
  high or low during reset. These alternative boot paths may bypass
  signature verification entirely.

#### ***Cryptographic Implementation Review***

Beyond evaluating the cryptographic algorithms chosen by the device
manufacturer, the IoTA team should evaluate the specific
implementations in use. IoT devices commonly use lightweight TLS/DTLS
libraries such as mbedTLS, wolfSSL, or BearSSL rather than OpenSSL.
Each of these libraries has its own history of vulnerabilities and
implementation-specific configuration pitfalls. The team should
evaluate: whether the device's TLS library is a current, patched
version; whether certificate validation is properly implemented (does
the device reject expired, self-signed, or hostname-mismatched
certificates?); whether the random number generator produces
sufficient entropy on the target platform (constrained devices with
limited entropy sources are particularly vulnerable to weak key
generation); and whether cryptographic key material is stored securely
(plaintext in flash vs. secure element vs. TEE-protected storage).

#### ***Container and Runtime Analysis***

For Linux-based IoT gateways and edge compute devices that use Docker,
containerd, or LXC for application isolation, the IoTA team should
evaluate container escape risks, shared kernel vulnerabilities, and
privilege escalation from containerized services to the host system.
The team should assess whether containers run with excessive
privileges (such as the --privileged flag or host network mode),
whether sensitive host resources (such as /dev or /proc) are mounted
into containers, and whether the container runtime itself is a current,
patched version. Container image contents should be subjected to the
same SBOM and vulnerability analysis applied to traditional firmware
images.

#### ***Firmware Update Mechanism Security***

The security of the over-the-air (OTA) update pipeline deserves
focused evaluation. The IoTA team should assess whether firmware
updates are downloaded over encrypted channels (TLS/DTLS), whether
the device validates the integrity and authenticity of firmware images
before applying them through cryptographic signatures (evaluating the
algorithm strength, key management practices, and certificate chain
validation), whether rollback protection prevents installation of
older firmware versions, whether the update process is atomic (a
failed update should not render the device inoperable), and whether
the update metadata or control channel itself is vulnerable to
injection attacks. The team should also evaluate whether the device
checks for updates automatically, whether this check can be
manipulated by an attacker to serve malicious firmware, and whether
delta/differential update mechanisms introduce parsing vulnerabilities.
