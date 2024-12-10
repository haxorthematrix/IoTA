# Internet of Things Attack (IoTA) Methodology


# Introduction

## Purpose and Scope

This document describes the Internet of Things Assessment (IoTA) Red
Team approach to security testing of different IoT devices and
architectures. The definition of IoT varies depending on the
organization providing the definition. This document will proceed on the
basis of the definitions in NIST Special Publication SP800-183.
That document is written around the concept of NoTs (Networks of Things)
and treats IoTs as "an instantiation of a NoT, more specifically, IoT
has its 'things' tethered to the Internet." A NoT is usually composed of
five *primitives* or building blocks, and may have one or more of each
primitive. They are, briefly:

-   Sensor: An electronic utility that measures physical properties
    using some interface into a process or environment. Sensors are
    physical, provide data (possibly via a transmission capability), and
    often have minimal or no extra functionality or computing power.

-   Aggregator: Software that transforms groups of raw data into
    intermediate, aggregated data. It involves computational power by
    necessity, may come in a hard- or soft-coded implementation, and may
    run on either a physical or a virtual platform.

-   Communication Channel: A wired or wireless medium by which data is
    transmitted, either unidirectional or bidirectional.

-   eUtility (external utility): A software or hardware product or
    service that executes processes or feeds or processes data in the
    workflow of a NoT. Currently a very abstract concept, it includes
    almost anything else: databases, computers, cloud environments,
    mobile devices, and even humans.

-   Decision Trigger: A conditional expression that triggers an action,
    which may include controlling an actuator or performing a
    transaction. Decisions may be binary or have a range of values, may
    adapt to the environment, and will usually be implemented in code
    but may be mechanical.

This document focuses on equipment placed in the hands of a consumer,
whether residential or enterprise, employing embedded computer
architectures not physically protected by on premise security measures,
as well as its supporting infrastructure. That is to say, IoT-type
devices, placed in the marketplace at large, in the hands of consumers,
who hold some expectation of reasonable "right to repair," and the
services and systems that extend their capabilities by processing,
storing, and distributing data associated with IoT device operations.
This equipment and technology includes the following resources:

-   The IoT hardware device and embedded firmware/operating system (OS)
    that the customer interacts with or deploys;

-   Network data streams and related upstream network gear, up to but
    not including the backend data storage and collection;

-   The backend data storage and collection, including various network
    services such as webservers, database services and web based
    applications;

-   Mobile device applications used to interact either directly with the
    IoT device or via a cloud-based service;

-   Supporting data networks between the IoT device and eUtilities,
    including radio frequency communication systems (such as IEEE
    802.15.4, ZigBee, 6LoWPAN, IEEE 802.11, IEEE 802.16, LoRa, GSM/LTE,
    and proprietary systems) and wired communication media such as
    Ethernet, USB, and even HDMI.

Combining all of these concepts results in an ecosystem. Examining any
one part of it opens a path to other components, and within a short
distance, if not immediately, components begin to rely on each other's
proper behavior. A sensor device performs a function, relying on the
connected network to convey the data to other things. A mobile app
provides an interface and relies on the same network, and the network
relies on the mobile app and the sensor device to not send data across
at unmanageable rates or in unrecognizable form. Any of these
components, or of the many more that commonly make up a NoT or IoT, can
undertake an action that cascades through the entire ecosystem.

The scope of a given NoT may be ill-defined depending on the things that
make up the NoT. If a cloud environment is involved, the virtual servers
that make up the cloud environment can be included, as can the physical
servers that host the virtual servers. Networked KVM (keyboard, video,
mouse) consoles that connect to the physical servers could also be in
scope. But the extension is not endless: the physical servers' hosting
facility's power distribution network (PDN) is not an eUtility despite
potentially being used as a side-channel attack because it only arguably
fits the definition of a communication channel and does not match any
other primitive. Likewise, the computers controlling the PDN are not
part of the NoT. (Those controllers and the PDN itself might be a
separate NoT, however.)

While home or commercial networks are not primary targets within the
scope of the initial IoTA Red Team security testing, the approach and
attack-methodology described in this document may be applied to these
networks as well.

The IoTA Red Team consists of a group of experts in the field of
security analysis and penetration testing who are authorized to perform
in-depth security evaluations of IoT technologies. Through their
efforts, the IoTA project gains the perspective of how an adversary or
group of attackers can exploit IoT devices, providing a valuable account
of current strengths and weaknesses.

This document includes a description of the principles of testing and
vulnerability classes which can be pursued; a description of the lab
equipment and toolset in use; and a detailed attack methodology.

The purpose of this document is to provide guidelines for utilities and
vendors to test their own equipment or to outsource such testing with a
better understanding of what to expect from their attack team. While
this document's authors have attempted to communicate a significant
level of depth clearly, it is important to note that these are simply
guidelines for testing. A deep technical understanding of the involved
equipment, protocols, electronics, and vulnerability research must be
coupled with creativity and time for valuable testing results to be
achieved.

This document is comprised of four major sections for each technology:
Principles of Testing, Constructing a Lab, Common Vulnerability Types,
and Attack Methodologies. Wherever appropriate, we have attempted to
provide a step-by-step walk-through with a hands-on feel to our
descriptions. In many cases, perfectly valid attack methodologies exist
for various technologies; there is no need to reinvent the wheel in
these situations. In cases where valid attack methodologies exist, we
will reference them and provide additional insight where appropriate.

The scope of this document does not include other IoT company
components, such as e-mail systems, "marketing" websites, customer
relationship management (CRM) systems, or other related services which
complete an overall business model. It should be noted that the
principles of testing these systems are similar to the techniques
described in this document. However, as they have different access
constraints and tend to be implemented on larger-scale computers with
complex multiprocessing operating-systems, the toolset, methodology,
vulnerabilities and impact are quite different. Testing of these
components is vital to the security of the IoT ecosystem, and future
IoTA work will likely focus on them.

## Executive Summary

Vulnerabilities exist in any complex system. Identifying weaknesses and
evaluating their associated risk allow for these vulnerabilities to be
addressed, protecting customers, manufacturers, and vendors alike. The
stakes are relatively high, impacting the viability of a produce in the
marketplace, protection of PCI related data and financial transactions,
and privacy issues related to user tracking and behavior (especially
minors).

This document is best read by IoT security teams and penetration
testers. It has been prepared to provide vendors the ability to
understand the vulnerabilities and attacks in order to protect their
equipment properly; to provide utilities the knowledge required to
enlist appropriate skill sets for testing their own implementations; and
to ensure test consistency among attack teams between different vendor
architectures. Throughout the document, the authors aim to bring value
both to the attack-team and the IoT vendor employing them.

The IoTA Red Team recommends IoT vendors create full-time security teams
if none currently exist, and additionally employ internal and
third-party attack teams to search for and illustrate vulnerabilities in
the IoT vendor architectures and the impact of exploitation. Security
must be designed into and tested at every stage in the IoT rollout
process, including overall IoT system design, manufacturing, delivery,
storage, and implementation. Weaknesses in any stage of this process can
lead to failure in privacy, financial transactions and company
reputation. Regular penetration-tests executed by qualified attack teams
are a necessary proof of such security measures and can provide valuable
insight to improve existing architectures.

The IoTA Red Team recommends IoT vendors perform the following security
exercises regularly (see Conclusions for a more descriptive list):

-   Physical Security Penetration Tests

-   Embedded Device Security Analysis and Vulnerability Assessment

    This document has a broad scope for securing the end to end solution
    for IoT solutions from the consumer devices (Embedded Device
    Penetration Tests from the list above), to systems which reside
    inside the IoT vendor perimeter or cloud provider. Securing vendor
    inside systems is even more important than the systems which reside
    externally, because they can often affect more damage if
    compromised. For this reason, IoTA plans to address security testing
    of those systems as well. Many security devices and processes exist
    to attack, analyze and secure back-end systems used in IoT
    infrastructure, and we have attempted to document the processes used
    there as well, without reinventing the wheel. Nearly every hacker
    conference discloses new and old attack methodologies for back-end
    and networking infrastructure, while training organizations such as
    the SANS Institute offer classes on defending them.

# Principles of IoT Vulnerability Assessment

To maximize the effectiveness and applicability of the security
evaluation of IoT-related technology, the attack team should follow a
series of testing principles supporting consistent technology analysis.
Through these techniques, a team of analysts can produce a cohesive
assessment of one or more target devices or IoT deployments. The
resulting documentation will provide necessary information to remediate
the threats and vulnerabilities that would otherwise hinder the
successful marketing and sales of IoT-based technology.

## Practical and Pertinent Vulnerability Analysis 

As a critical component of each security evaluation exercise, the attack
team should work with vendors, the IoT security team, utilities and
applicable third-parties to identify an appropriate scope for the
analysis. The scope must take into account the resources of an adversary
that may attempt to compromise the security and integrity of IoT-related
technology. The resources accessible to a potential adversary will
significantly influence the threats that require defense and mitigation
strategies. For example, an attacker who has less than US\$10,000 in
accessible resources for the purpose of exploiting IoT technology
represents a different threat than a well-funded adversary whose goals
exceed that of common mischief or exposure of private data. The scope
for the analysis should address the highest level of attacker funding
feasible, while the team must keep in mind all of the different levels
of attack. For instance, the scope may include the resources available
to a nation-state, but the attack team should be cautious not to ignore
the attack vectors likely to be targeted by self-funded individuals.

Based on the scope, the attack team should apply scientific analysis
methods to enumerate and evaluate potential threats, collecting
supporting data through observation and experimentation, then design
further analysis test cases to evaluate through experimentation. Threats
that are deemed irrelevant or impractical based on the identified
adversary resources can be disregarded, focusing on the practical and
pertinent threats immediately affecting customers, utilities and
vendors. Noting these discarded threats, their likelihood and impact of
exploitation should be considered additional value to a final report.
Vendors and utilities must understand the contextual constraints the
analysis project scope places on any engagement and subsequent report.

Through this method, the attack team will focus on the areas of direct
value for affected parties, providing the greatest possible benefit in
the analysis and findings reporting.

## Testing Team Expertise

In order to be effective in the analysis of IoT technology, the attack
team will require expertise in multiple areas of information security,
mobile device, web application and API testing, electrical and radio
engineering and protocol analysis. Due to the varied expertise
requirements, it is anticipated that the attack team will be composed of
several individuals with diverse backgrounds, collaborating on the
analysis of IoT technology.

Fields of expertise required for effective analysis include:

-   Basic and advanced understanding of electronics and principles of
    electricity.

-   Safety training and experience working with high-voltage
    electronics.

-   Knowledge of the assembly language for processors used by IoT
    devices associated hardware. This may require, depending on the
    platform(s) involved, knowledge of multiple variants including x86,
    x86_64, ARM, Arduino, et al.

-   Mobile device operating system structure and analysis techniques for
    evaluating applications

-   RF modulation and coding analysis experience.

-   Experience analyzing standards-based protocols including infrared,
    IEEE 802.15.4/ZigBee, IEEE 802.16/WiMAX, IEEE 802.11/Wi-Fi, LoRa and
    proprietary protocols.

-   Knowledge of wired and wireless network design and protocols, and
    detailed understanding of OSI model layers and their interactions.

-   Experience developing embedded technology including embedded
    software development programming and hardware engineering.

-   Ability to creatively evaluate technology for the goal of
    subversion.

-   Software vulnerability identification and analysis skills. Forensic
    capabilities may add value here.

-   Experience writing software-based exploits.

-   Analysis skills for evaluating cryptographic algorithms and
    associated functions.

-   Modern internet protocols and standards such as OAuth and HTTP/HTTPS

-   Web based technologies and languages, and varies methods for
    accessing data through these technologies, (Java, RESTful
    interfaces, etc.)

## Reproducible Findings

The attack team recognizes that any findings identifying security
threats or vulnerabilities in IoT-related technology will be subject to
significant scrutiny and analysis. Throughout the project, all
security-related findings will be documented such that other analysts
will have sufficient information to reproduce the findings for an
independent findings evaluation, where desired. Methodologies and
results should both be carefully documented to make results repeatable
by other researchers, the associated vendor(s), and IoT companies with
access to similar resources to the testing team.

## Risk Evaluation

The identification of risks to customers, utilities and vendors of
IoT-related technology will be one of the deliverables for the attack
team during the IoTA project. Where vendors and utilities have a finite
amount of resources to apply to the mitigation of risks, it is necessary
to provide an overall prioritization of identified threats such that
threats of the greatest urgency are resolved before threats with little
to no overall risk.

To support the prioritization and evaluation of risk, the attack team
should evaluate all threats and vulnerabilities with supporting
documentation as follows:

-   Risk Description: Information describing the risk, citing references
    where applicable.

-   Risk Probability: The probability of the risk happening to a vendor
    based on the understanding of potential adversaries that require
    defensive measures. This probability should be recorded on a scale
    of 1 to 10 with 1 being least probable and 10 being the most
    probable.

-   Risk Impact: The potential impact of the risk to the utility or
    vendor should the threat be realized. The impact should be recorded
    on a scale of 1 to 10 with 1 being least impact and 10 being the
    greatest impact.

-   Risk Data Quality Evaluation: Risk evaluation requires unbiased and
    accurate data for credibility. The risk documentation should include
    the following data points to describe the quality of the risk data:

    -   Risk Understanding: How well is the risk understood?

    -   Data Availability: How complete is the data pertaining to the
        risk?

    -   Data Quality: Is the data available relevant to the risk being
        described? Is the data current?

    -   Data Reliability: How objective is the data that has been
        supplied to describe the threat? The reliability of data will
        increase with multiple data points from different sources coming
        to similar conclusions, or decrease if the data is highly biased
        or widely disparate across multiple sources.

-   Risk Urgency Assessment: What is the urgency of the risk?

The completed risk data will be used to populate a quantitative risk
evaluation model, providing the necessary information to the utility or
vendor for prioritizing threat remediation, transfer or acceptance.

## Opportunities for Defensive Measures

While the focus of the attack team is to identify vulnerabilities
threatening IoT technology, such in-depth analysis will also reveal
defensive measures for mitigating the impact or probability of threats.
While not a definitive measure of the only strategy for defending
against identified vulnerabilities, the attack team should identify
strategies for the mitigation of vulnerabilities that may be adopted by
the utility or the vendor.

# Methodology

## **Recon**

### OSINT (fccid.io, patents, product documentation, etc.)

Public documentation and marketing material provide a wealth of
information pertinent to attack. New features in a product revision
announcement often indicate code that is newer and less thoroughly
reviewed for vulnerabilities. The team\'s understanding of an IoT
component\'s features and intended behavior will provide insight into
security weaknesses as well as context for binary code analysis.

Sources of public information pertinent to analysis include, but are not
limited to:

-   Marketing literature,

-   Component datasheets,

-   Component application notes,

-   Operating manuals,

-   User support forums and FAQs,

-   Radio block diagrams,

-   External and internal photographs,

-   Operational descriptions,

-   API documentation,

-   Schematic diagrams,

-   FCC test reports, and

-   Patent filings.

Available documentation will vary greatly between manufacturers. In some
cases, an adversary may resort to attacking publicly accessible systems
or collecting resources from trash receptacles for a target manufacturer
to obtain access to otherwise private information, including internal
support base access, firmware updates, unpublished press releases, and
similar sensitive information. Evaluation of the security of enterprise
computing resources for a particular vendor is beyond the scope of IoTA,
but no vendor should neglect to secure their enterprise environments
properly or to consider this threat to their products.

Throughout the document enumeration phase, the IoTA Red Team should
identify the sources of information that is collected. These sources may
include information retrieved from publicly accessible-sources, from
documentation released under NDA, through unauthorized information
access or from word-of-mouth and casual conversations with informed
personnel. By evaluating the sources of information leveraged for the
analysis of target devices, vendors and utilities can evaluate their
data prevention systems.

### PCB/Chip inspection 

The visual inspection of an IoT device will reveal additional
information about the configuration and use of the device. Items of
interest include out-of-band management interfaces, tamper-protection
measures, physical layout and connections, and antennas.

The following (incomplete) list provides a starting-point for analysis
during this phase:

-   Analyze the antenna size and shape, which reveals the intended band
    of operation

-   Document which ICs (particularly microcontrollers and EEPROMs) are
    connected to the board, what they are connected to, and which pins
    are used to connect them

-   Identify and document any interfaces to microcontrollers including
    JTAG, ICP/ISP, IR, RS232, Ethernet, SPI, I^2^C, etc.

-   Identify physical organization of circuit-boards; related components
    are most often grouped together, providing context for determining
    their purpose

-   Identify groupings of long traces on the circuit-boards; these are
    likely a bus of some sort, interesting for logic-analyzers and
    providing context to the relationship between sections of the
    circuit.

-   When possible, perform pin tracing to high value pin targets
    identified in data sheets. These pins may be accessible through
    unused surface mount or through-hole locations, vias and test
    points.

-   Basic inspection with a multimeter is also possible during this
    phase. For example, ground planes and pins can often be identified
    using continuity testing, pin voltages can be observed, resistance
    values can be obtained, etc.

-   Basic Oscilloscope and/or Logic Analyzer probing can be performed
    once appropriate grounds have been identified and system voltages
    have been determined to be in range of the inspection device.

### External Device Inspection for Network Connectivity

The external visual inspection of an IoT device will reveal additional
information about the connectivity options of the device. Items of
interest include out-of-band management interfaces, WAN, LAN and DMZ
ethernet ports.

Typically these ethernet ports will feature an external RJ-45 interface.
RJ-45 can also be used for other connectivity options, such as RS-485,
RS-232, power, or even a proprietary implementation. As a result of the
multiple uses, in cases where the connectors may be unlabeled it is
important to verify the use of these connectors through comparison to
the available documentation, schematics, or internal inspection.

### External Device RF Communications Inspection

Continued visual inspection of an IoT device will reveal additional
information about the connectivity options of the device. Items of
interest include antennas.

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

-   Remote tamper detection systems, where an IoT device can remotely
    > notify the manufacturer office that someone is tampering with a
    > device;

-   System integrity protection systems, where an IoT device can protect
    > the integrity of a system, including self-erasure of keys and
    > firmware;

-   Intended repair modes, used for authorized repair personnel from the
    > manufacturer, company or vendor;

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

### Insecure internal and external buses

Embedded systems commonly use peripheral devices such as radios or
EEPROM chips, interfacing with microcontrollers through SPI, I^2^C or
other types of serial bus interfaces. Although this is a convenient and
industry standard method for interfacing peripheral devices with each
other, it represents a security risk for a device with little or no
physical protection. For example, the use of two electrical probes
constructed from medical syringes connected to a protocol adapter to
extract the firmware from an EEPROM device over an I^2^C bus. The
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
devices to extend the attacker\'s access into the target network.

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
electric probes and a shared ground to dump an I^2^C EEPROM\'s
contents. The multi-master features of I^2^C allow this to be
performed while the device is still active. The two probes, modified
hypodermic syringes, are tapping the Serial Data (SDA) and Serial
Clock (SCL) lines.

SPI is more difficult to tap, yet not insurmountable. Because it lacks
the multi-master mode of I^2^C, any attempt to read or write the
memory chip will result in interference from the master
microcontroller. Three methods exist by which the memory chip may be
accessed without the microcontroller\'s interference.

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

#### ***Firmware tampering (no code signing, etc.)***

Once a device firmware file is obtained by dumping a devices memory or
by other means, the attack team should attempt to unpack a device
firmware, make alterations, and repack the firmware. File system
modification can sometimes be achieved using tools like dd, binwalk
and firmware-mod-kit or simply by changing a byte or two of
non-critical data with a hex-editor. One such example would be
changing a character in a text string from "Login" to "Logon". If the
resulting repacked firmware image can be uploaded to the device, this
can be the first step leading to the insertion of backdoors or
otherwise altering the devices expected operating behavior. For
example, replacing a binary called at device startup with a bind or
reverse shell could result in direct access to the device itself that
was not originally possible.

A word of caution for the IoTA team: Modifying a firmware image can be
tricky and can cause unintended consequences or even result in the
"bricking" of a device. Ensure that a backup device is available or
that the device has a firmware recovery feature.

In some cases firmware modification may require significant effort to
achieve and in other cases it may not be possible due to code signing
validation checks and hash based file verification.

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

####Frequency

Identifying the operating frequency is the first step in reverse engineering RF communications. This involves using a spectrum analyzer to detect the frequency bands utilized by the target device. Common frequency bands include industrial, scientific, and medical (ISM) bands such as 433 MHz, 868 MHz, and 2.4 GHz. Accurate frequency identification is crucial for tuning receivers and further analysis.

###Modulation

Modulation defines how data is represented over RF signals. Common modulation schemes include Amplitude Shift Keying (ASK), Frequency Shift Keying (FSK), and Phase Shift Keying (PSK). Determining the modulation type can be achieved by analyzing the signal's waveform using an oscilloscope or software-defined radio (SDR) tools. Understanding the modulation scheme is essential for demodulating the signal into a baseband data stream.

###Encoding

Encoding refers to how binary data is formatted for transmission. Protocols may use encoding schemes such as Non-Return to Zero (NRZ), Manchester encoding, or others. Identifying the encoding method requires examining the demodulated data stream to interpret the bit patterns correctly. This step is vital for accurately reconstructing the transmitted data.

###Packet Breakdown

RF communications are typically structured into packets comprising several components:

Sync Word: A predefined sequence that indicates the start of a packet, allowing receivers to synchronize with the incoming data stream.

Headers: Contain metadata such as address information, packet type, and length, which are essential for proper data interpretation and routing.

Data Payload: The actual user data transmitted.

Analyzing the packet structure involves capturing multiple packets and identifying consistent patterns corresponding to these components. Tools like SDRs combined with protocol analysis software such as RTL_433 and Universal Radio Hacker (URH) can facilitate this process. 



Upcoming additions

-   Web app testing (if applicable) Local device and "cloud" management
    services

    -   XSS

    -   SQLi

    -   XXE

    -   Session attacks

    -   IDOR

    -   etc.

-   iOS and Android app testing (if applicable)

    -   Hardcoded secrets in source

    -   Reversible protocols in source

    -   Reversible encryption in source

    -   Reversible native libs (Ghidra, IDA, etc.)

    -   Cert pinning attacks

    -   Root/jailbreak detection

    -   Vulnerable third party libs

    -   etc.

-   Network testing (if applicable)

    -   Exposed network services

    -   Cleartext services (telnet, http, ftp, etc.)

    -   Vulnerable services

    -   Sniffing traffic in transit, cleartext etc

    -   Etc.

-   API testing

    -   Availability of documentation

    -   Cleartext services

    -   Public passwords

    -   SOAP

    -   REST

    -   GraphQL
