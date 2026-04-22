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
    802.15.4, Zigbee 3.0, Thread, Matter/CHIP, 6LoWPAN, IEEE 802.11
    (Wi-Fi 4/5/6/6E/7), Bluetooth Classic, BLE 5.x, Z-Wave, LoRa/LoRaWAN,
    Meshtastic, NB-IoT, LTE-M, 4G/5G NR, UWB, DECT ULE, Amazon Sidewalk,
    Wi-SUN, satellite IoT, and proprietary systems) and wired
    communication media such as Ethernet, USB, CAN/CAN-FD, Automotive
    Ethernet, and HDMI.

-   The software supply chain underpinning the IoT device firmware,
    including open-source components, vendor SDKs, RTOS kernels,
    third-party libraries, and their associated dependency trees. Each
    component carries its own vulnerability surface and must be
    inventoried, tracked, and evaluated throughout the product lifecycle.

The IoT landscape is also shaped by an evolving regulatory environment
that the IoTA team should be aware of when scoping engagements and
reporting findings. Relevant frameworks include the EU Cyber Resilience
Act (CRA), US Executive Order 14028, NIST Cybersecurity Framework 2.0,
ETSI EN 303 645, the FCC Cyber Trust Mark program, and sector-specific
standards such as UNECE WP.29 R155/R156 for automotive, IEC 62443 for
industrial systems, and FDA premarket cybersecurity guidance for medical
devices.

In addition to consumer and enterprise IoT, this methodology applies
to Industrial IoT (IIoT) and OT convergence environments, Internet of
Medical Things (IoMT), connected vehicles and EV charging
infrastructure, smart city and critical infrastructure deployments, and
AI/ML-enabled edge devices. The scope of a given engagement should be
defined to encompass the relevant device classes and their supporting
ecosystems.

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

The scale of the IoT threat landscape continues to grow. With over 21
billion connected IoT devices globally, the attack surface is vast and
expanding. Supply chain compromise has emerged as one of the most
impactful attack vectors, as demonstrated by incidents such as BadBox
2.0 (pre-installed malware affecting over 10 million smart devices) and
the Mars Hydro data exposure (2.7 billion records leaked from an IoT
device manufacturer's unprotected database). The rise of AI-augmented
attack tooling further increases the velocity and sophistication of
attacks against IoT ecosystems.

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
    IEEE 802.15.4/ZigBee, IEEE 802.11/Wi-Fi, BLE, LoRa/LoRaWAN,
    Matter/Thread, cellular IoT (NB-IoT, LTE-M, 5G), and proprietary
    protocols.

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

-   Web based technologies and languages, and various methods for
    accessing data through these technologies, (Java, RESTful
    interfaces, etc.)

-   AI/ML security: Understanding of adversarial attacks against ML
    models deployed on edge devices, model extraction, data poisoning,
    and inference attacks.

-   Cloud-native and container security: Many IoT backends now run on
    Kubernetes, serverless functions, and containerized microservices.
    Experience with AWS IoT Core, Azure IoT Hub, Google Cloud IoT, and
    similar platforms is increasingly necessary.

-   Automotive protocols: CAN, CAN-FD, Automotive Ethernet
    (100BASE-T1/1000BASE-T1), UDS, DoIP, SOME/IP, and AUTOSAR-based
    systems.

-   Matter/Thread protocol stack analysis and commissioning security,
    including PASE/CASE cryptographic evaluation and Device Attestation
    Certificate (DAC) analysis.

-   SBOM (Software Bill of Materials) analysis and software composition
    analysis (SCA) tooling. Familiarity with CycloneDX and SPDX formats,
    binary-level composition analysis platforms, and vulnerability
    database cross-referencing workflows.

-   Regulatory framework literacy: Ability to map findings to EU CRA
    essential requirements, ETSI EN 303 645 provisions, NIST CSF 2.0
    functions, and sector-specific standards (IEC 62443 for industrial,
    IEC 81001-5-1 for medical, ISO/SAE 21434 for automotive).

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

In addition to the probability/impact scoring above, the IoTA team
should align findings with industry-standard frameworks to improve
compatibility with vulnerability management workflows. CVSS 4.0 scoring
should be applied to all findings to provide a standardized severity
metric. The Stakeholder-Specific Vulnerability Categorization (SSVC)
framework should be used where appropriate to provide decision-oriented
prioritization that accounts for the exploitation status, automatable
nature, and mission impact of a vulnerability, rather than relying on
CVSS base scores alone.

Two additional risk dimensions are particularly relevant for IoT
assessments:

Supply chain risk: A vulnerability in a widely deployed SDK component
or open-source library has fundamentally different blast radius
characteristics than a vulnerability in custom firmware code. A single
vulnerable version of a TLS library or RTOS kernel may affect millions
of devices across multiple vendors. Supply chain vulnerabilities should
be evaluated with consideration of the component's prevalence across
the broader IoT ecosystem, not only the single device under test.

Regulatory risk: A finding that would trigger EU Cyber Resilience Act
reporting obligations, block market access under CE RED, or violate
ETSI EN 303 645 provisions carries urgency beyond its purely technical
impact. Findings that implicate regulatory non-compliance should be
flagged with appropriate regulatory references so that the vendor can
prioritize remediation accordingly.

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
reviewed for vulnerabilities. The team's understanding of an IoT
component's features and intended behavior will provide insight into
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

-   International regulatory filings beyond FCC, including IC (Canada),
    CE (EU), ARIB (Japan), and other bodies. These filings often contain
    detailed RF test reports, block diagrams, and internal device
    photographs not available from other sources.

-   Software Bill of Materials (SBOM) analysis: Where available, obtain
    or generate an SBOM for the target firmware using binary analysis
    tools. Cross-reference identified components against NVD, OSV, CISA
    KEV, and vendor-specific advisory databases.

-   CVE and advisory mining: Search for known CVEs against identified
    chipsets, RTOS platforms (FreeRTOS, Zephyr, ThreadX/Azure RTOS,
    Mbed OS, NuttX, QNX), and SDK versions used by the target device.

-   Cloud infrastructure reconnaissance: Identify cloud backend
    providers, API endpoints, and CDN configurations through DNS
    enumeration, certificate transparency logs (crt.sh), and application
    traffic analysis.

-   Source code repository analysis: Many IoT vendors maintain public
    or accidentally exposed repositories on GitHub, GitLab, or similar
    platforms. Search for leaked credentials, development branches with
    debug features enabled, CI/CD configurations, and internal
    documentation using tools such as GitDorker and TruffleHog.

-   Internet-wide scanning databases: Services such as Shodan, Censys,
    and ZoomEye index internet-connected devices and can be used to
    identify deployed populations of a target device, exposed services,
    common misconfigurations, and firmware version distributions across
    the installed base.

-   Job posting intelligence: Vendor job listings often reveal
    technology stacks, development tools, cloud providers, and
    architectural decisions that inform testing strategy.

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

-   Identify Trusted Platform Modules (TPMs), Hardware Security Modules
    (HSMs), Secure Elements (e.g., Microchip ATECC608, Infineon OPTIGA
    Trust, NXP SE050), and Trusted Execution Environments (TEEs, e.g.,
    ARM TrustZone, RISC-V PMP). The presence or absence of these
    components significantly influences the threat model and the
    attack paths available.

-   Document SoC identification with emphasis on modern platforms
    commonly found in IoT devices: Espressif ESP32-S3/C6/H2, Nordic
    nRF52840/nRF5340/nRF9160, Silicon Labs EFR32, Qualcomm QCS/QCM
    series, Realtek Ameba, MediaTek MT76xx/Genio, and TI CC13xx/CC26xx.
    Identifying the exact SoC variant informs the available debug
    interfaces, boot security features, and known vulnerabilities.

-   Identify and document any AI/ML accelerator hardware (NPUs, TPUs,
    DSPs used for on-device inference). These components introduce
    additional attack surface related to model extraction, adversarial
    input processing, and side-channel leakage during inference
    operations.

-   X-ray inspection may be necessary for multi-layer PCBs and BGA
    packages where visual inspection alone is insufficient to determine
    trace routing and component interconnections.

-   Thermal imaging during device operation can identify active
    components, power regulation circuits, and heat dissipation patterns
    that inform functional analysis and potential side-channel attack
    points.

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

## **Web Application Specific**

Many IoT devices expose web-based management interfaces, either hosted
locally on the device itself or provided through a cloud-based service
operated by the vendor. These interfaces frequently serve as the primary
means by which end users configure, monitor, and control IoT devices.
Both local and cloud-hosted web applications present attack surfaces
that the IoTA team should evaluate thoroughly. The same categories of
vulnerabilities found in traditional web applications apply here, often
compounded by the resource constraints and less rigorous development
practices common in embedded and IoT product development.

The IoTA team should evaluate web applications associated with IoT
devices in two contexts: the local device web interface (if present)
and any cloud or vendor-hosted management portals. Both should be
tested with the same rigor, as compromise of either can lead to full
device takeover, data exfiltration, or lateral movement into other
parts of the IoT ecosystem.

### Injection Vulnerabilities

#### ***Cross-Site Scripting (XSS)***

Cross-Site Scripting vulnerabilities occur when an application includes
untrusted data in web pages without proper validation or encoding. In
the context of IoT device management interfaces, XSS can be
particularly dangerous because administrative sessions often carry
elevated privileges, and device web interfaces may lack the security
headers and content security policies common in more mature web
applications. The IoTA team should test for reflected, stored, and
DOM-based XSS across all input fields, URL parameters, and any
location where user-supplied data is rendered back to the browser.
Stored XSS in device configuration fields (such as SSID names, device
hostnames, or user profile fields) is of special concern because it
can persist across sessions and affect multiple users of a shared
management portal.

#### ***SQL Injection (SQLi)***

SQL injection vulnerabilities allow an attacker to manipulate database
queries by injecting malicious SQL statements through application input
fields. While many IoT devices use lightweight databases such as SQLite
rather than full relational database management systems, the risk of
SQLi remains significant. Successful exploitation can lead to
unauthorized data access, authentication bypass, or in some cases
command execution on the underlying operating system. The IoTA team
should test all input points that interact with backend data stores,
including login forms, search functionality, device filtering, and
reporting features.

#### ***XML External Entity (XXE) Injection***

IoT devices and their associated services frequently process XML data
for configuration imports, firmware metadata parsing, SOAP-based API
calls, and inter-device communication. If the XML parser is configured
to resolve external entities, an attacker may be able to read arbitrary
files from the server, perform server-side request forgery (SSRF), or
cause denial of service through entity expansion attacks (sometimes
referred to as "billion laughs" attacks). The IoTA team should identify
all points where XML data is accepted and test for external entity
resolution, parameter entity injection, and out-of-band data
exfiltration through external DTD references.

#### ***Command Injection***

Embedded web applications on IoT devices frequently execute system
commands to perform network diagnostics, configuration changes, or
firmware operations. If user input is passed to these system commands
without proper sanitization, an attacker can inject additional commands
to execute arbitrary code on the device. This vulnerability class is
especially prevalent in IoT devices because embedded web applications
often invoke shell commands directly rather than using dedicated
libraries or APIs. The IoTA team should test all input fields that may
interact with underlying system commands, including network diagnostic
tools (ping, traceroute, DNS lookup), configuration utilities, and file
management functions.

### Session and Authentication Attacks

The IoTA team should evaluate the session management and authentication
mechanisms of IoT web interfaces for common weaknesses. Areas of concern
include the use of predictable or sequential session tokens, lack of
session expiration or timeout, failure to invalidate sessions on logout,
transmission of session identifiers over unencrypted channels, and
susceptibility to session fixation attacks. Many IoT web interfaces
implement custom authentication schemes rather than using established
frameworks, increasing the likelihood of implementation flaws.

The team should also evaluate whether the application enforces account
lockout policies after repeated failed authentication attempts, whether
multi-factor authentication is supported or available, and whether
password reset mechanisms can be abused to enumerate valid accounts or
bypass authentication entirely.

### Insecure Direct Object References (IDOR)

Insecure Direct Object Reference vulnerabilities arise when an
application exposes internal implementation objects, such as database
keys, file paths, or device identifiers, in a way that allows an
attacker to manipulate them to access unauthorized resources. In
multi-tenant IoT cloud platforms, IDOR vulnerabilities can allow one
user to access or modify another user's devices, configurations, or
data simply by changing a predictable identifier in a request. The
IoTA team should test all API endpoints and web application functions
that reference device identifiers, user accounts, configuration files,
or firmware images for unauthorized access through parameter
manipulation.

### Cross-Site Request Forgery (CSRF)

Cross-Site Request Forgery vulnerabilities allow an attacker to trick
an authenticated user into performing unintended actions on a web
application. For IoT device management interfaces, CSRF can be
leveraged to change device configurations, modify network settings,
update firmware, create new administrative accounts, or disable
security features, all without the user's knowledge. The IoTA team
should evaluate whether the application implements anti-CSRF tokens,
validates the origin and referrer headers of incoming requests, and
requires re-authentication for sensitive operations such as password
changes or firmware updates.

### Server-Side Request Forgery (SSRF)

Server-Side Request Forgery vulnerabilities allow an attacker to induce
the server-side application to make HTTP requests to an arbitrary domain
or internal resource. In the context of IoT cloud platforms, SSRF can
be leveraged to access internal services, cloud metadata endpoints
(such as AWS EC2 instance metadata at 169.254.169.254), or other
devices on the internal network. The IoTA team should test any
functionality that accepts URLs as input, including webhook
configurations, firmware update URLs, remote syslog server settings,
and integration endpoints.

### Information Disclosure

IoT web applications should be evaluated for information disclosure
vulnerabilities, including verbose error messages that reveal internal
paths, software versions, or stack traces; directory listing enabled on
the web server; exposure of configuration files, backup files, or
source code through predictable URLs; and inclusion of sensitive data
such as API keys, credentials, or internal IP addresses in client-side
JavaScript, HTML comments, or HTTP response headers. The IoTA team
should also evaluate whether the application properly restricts access
to administrative or debugging endpoints that may have been left
enabled from development.

### Transport Layer Security

The IoTA team should evaluate the TLS configuration of all web
interfaces, including supported protocol versions, cipher suites,
certificate validity, and implementation of HTTP Strict Transport
Security (HSTS). Many IoT device web interfaces use self-signed
certificates, outdated TLS versions, or weak cipher suites, any of
which can allow an attacker to intercept or manipulate communications
between the user and the device. The team should also evaluate whether
the application properly redirects HTTP requests to HTTPS and whether
sensitive data such as credentials or session tokens are ever
transmitted over unencrypted connections.

### WebSocket and Real-Time Communication Security

Many modern IoT management interfaces use WebSocket connections for
real-time device status updates and control. The IoTA team should test
for WebSocket hijacking, lack of origin validation on WebSocket
upgrade requests, missing authentication on WebSocket endpoints, and
whether sensitive commands can be injected through manipulated
WebSocket messages.

### Client-Side Storage Security

The IoTA team should evaluate whether IoT device management web
applications store sensitive data in browser-side storage mechanisms
such as localStorage, sessionStorage, or IndexedDB. Sensitive data
stored client-side, including API tokens, device credentials,
configuration data, and session identifiers, may be accessible to
cross-site scripting attacks, browser extensions, or other applications
running in the same browser context. The team should evaluate whether
sensitive data stored client-side is encrypted, whether it is properly
cleared on logout, and whether the application relies on client-side
storage for security-critical decisions that should be validated
server-side.

### OAuth and OpenID Connect Implementation

Many IoT cloud platforms use OAuth 2.0 or OpenID Connect (OIDC) for
user authentication and authorization. The IoTA team should evaluate
these implementations for: authorization code interception (especially
in mobile app redirect flows), token leakage through referrer headers
or browser history, PKCE (Proof Key for Code Exchange) bypass or
absence, improper scope validation (where a token granted for limited
access can be used for elevated operations), token lifetime and
refresh policies, and whether the implementation properly validates
redirect URIs to prevent open redirect attacks.

## **Mobile Application Specific**

Many IoT devices rely on companion mobile applications for initial
device setup, ongoing configuration, data visualization, and remote
control. These applications, developed for iOS and Android platforms,
represent a significant attack surface within the IoT ecosystem. A
vulnerability in a companion application can provide an attacker with
access to user credentials, device control capabilities, or sensitive
data collected by the IoT device. The IoTA team should evaluate
companion mobile applications with the same rigor applied to the
device firmware and web interfaces.

### Static Analysis of Application Binaries

#### ***Hardcoded Secrets in Application Source***

The IoTA team should decompile or disassemble the mobile application
binary to search for hardcoded secrets embedded in the application
code, resources, or configuration files. Secrets of interest include
API keys, authentication tokens, encryption keys, backend server URLs,
database credentials, and vendor-specific identifiers. Android
applications can be decompiled using tools such as jadx, apktool, or
dex2jar, while iOS applications can be analyzed using tools such as
class-dump, Hopper, or Ghidra. Many modern IoT companion apps are
built with cross-platform frameworks such as Flutter (requiring Dart
AOT compilation analysis) or React Native (requiring JavaScript bundle
analysis), which demand different decompilation approaches than native
Android or iOS development. Hardcoded secrets in mobile applications
are particularly concerning because application binaries are
distributed to end users and can be trivially extracted and analyzed
by an adversary.

#### ***Reversible Protocols and Encryption***

Analysis of the decompiled application source can reveal the
communication protocols used between the mobile application and the
IoT device or cloud backend. The IoTA team should identify whether
proprietary protocols are in use, whether standard protocols are
implemented correctly, and whether encryption is applied to data in
transit. Custom or proprietary protocol implementations discovered in
the application source should be evaluated for weaknesses including
lack of authentication, missing integrity checks, susceptibility to
replay attacks, and use of weak or hardcoded encryption keys.

#### ***Reversible Native Libraries***

Many mobile applications include native libraries (compiled C/C++ code)
for performance-critical operations such as cryptographic functions,
protocol handling, or media processing. These libraries can be
extracted from the application package and analyzed using tools such as
Ghidra, IDA Pro, or Binary Ninja. The IoTA team should evaluate native
libraries for the same classes of vulnerabilities found in firmware
binaries, including hardcoded credentials, buffer overflows, format
string vulnerabilities, and improper cryptographic implementations.

#### ***Vulnerable Third-Party Libraries***

Mobile applications commonly include third-party libraries and SDKs
for analytics, crash reporting, advertising, networking, and other
functions. The IoTA team should inventory all third-party dependencies
included in the application and evaluate them against known
vulnerability databases such as the National Vulnerability Database
(NVD) and vendor-specific security advisories. Outdated or vulnerable
third-party libraries can introduce exploitable weaknesses into an
otherwise secure application.

### Runtime Analysis

#### ***Certificate Pinning Bypass***

Certificate pinning is a security mechanism that restricts which
certificates an application will accept when establishing TLS
connections, preventing man-in-the-middle attacks even when an
attacker has installed a trusted root certificate on the device. The
IoTA team should evaluate whether the application implements
certificate pinning and, if so, attempt to bypass it using tools such
as Frida, Objection, or SSL Kill Switch. Successful bypass of
certificate pinning allows the team to intercept and analyze all
network traffic between the application and backend services using
an intercepting proxy such as Burp Suite or mitmproxy.

#### ***Root and Jailbreak Detection***

Some mobile applications implement checks to detect whether the device
has been rooted (Android) or jailbroken (iOS), refusing to operate or
limiting functionality on modified devices. While these checks can
provide a layer of defense against tampering, they are often
implemented poorly and can be bypassed through hooking frameworks such
as Frida or Xposed. The IoTA team should evaluate whether root/jailbreak
detection is implemented, the robustness of the detection mechanism,
and whether bypassing it exposes additional attack surface or reveals
behaviors the application hides on unmodified devices.

#### ***Insecure Data Storage***

The IoTA team should evaluate how the mobile application stores data
on the device, including credentials, authentication tokens, device
configuration data, user personal information, and cached
communications. Data stored in plaintext in shared preferences
(Android), NSUserDefaults (iOS), SQLite databases, or application log
files is accessible to other applications on a rooted or jailbroken
device and may persist even after the application is uninstalled.
The team should evaluate whether sensitive data is encrypted at rest
using platform-provided secure storage mechanisms such as the Android
Keystore or iOS Keychain.

#### ***Inter-Process Communication***

Mobile applications may expose functionality through inter-process
communication (IPC) mechanisms such as Android Intents, Content
Providers, Broadcast Receivers, or iOS URL Schemes and Universal Links.
Improperly secured IPC endpoints can allow other applications on the
device to invoke privileged functionality, access sensitive data, or
manipulate the IoT device through the companion application. The IoTA
team should enumerate all exported IPC endpoints and evaluate their
access controls and input validation.

#### ***Cloud API Key and Credential Embedding***

The IoTA team should evaluate whether the mobile application embeds
cloud API keys, Firebase configurations, AWS access keys, or other
backend credentials that could provide unauthorized access to the
vendor's cloud infrastructure. Firebase configuration files
(google-services.json for Android, GoogleService-Info.plist for iOS)
often contain project identifiers, API keys, and database URLs that
can be used to enumerate or access backend resources. The team should
test whether embedded credentials provide access beyond the intended
scope of the mobile application.

#### ***Push Notification Security***

The IoTA team should evaluate whether push notification channels
(Firebase Cloud Messaging for Android, Apple Push Notification service
for iOS) can be manipulated to deliver false alerts, trigger
unauthorized device actions, or exfiltrate data. The team should test
whether the application properly validates the source and integrity of
push notification payloads, whether notification content includes
sensitive data that could be exposed on the device lock screen, and
whether the push notification registration token can be hijacked to
redirect notifications to an attacker-controlled device.

### Bluetooth and Local Communication

Many IoT companion applications communicate directly with the device
over Bluetooth Low Energy (BLE), Wi-Fi Direct, or other local
communication channels during setup and ongoing operation. The IoTA
team should evaluate the security of these local communication
channels, including whether pairing is authenticated, whether data
exchanged over these channels is encrypted, and whether the application
validates the identity of the device it is communicating with.
Weaknesses in local communication can allow an attacker in physical
proximity to intercept sensitive data, inject commands, or
impersonate a legitimate device.

## **Network Specific**

IoT devices communicate over various network protocols and services,
both on the local network and over the internet to cloud-based
backends. The network attack surface of an IoT device encompasses all
listening services, data transmitted in transit, and the interactions
between the device and other systems on the network. The IoTA team
should evaluate the network posture of IoT devices with an approach
that accounts for the unique characteristics of embedded systems,
including limited processing resources, constrained update mechanisms,
and the tendency to deploy devices on networks alongside other
sensitive systems.

### Service Enumeration and Exposed Network Services

The IoTA team should perform comprehensive port scanning and service
enumeration against each IoT device under evaluation. Tools such as
Nmap, Masscan, and service-specific probes should be used to identify
all listening TCP and UDP services. Many IoT devices expose services
that are unnecessary for their intended function or that were left
enabled from development and debugging. Common examples include
Telnet, SSH, FTP, TFTP, UPnP/SSDP, mDNS, MQTT, CoAP, HTTP, and
custom services on non-standard ports. Each identified service should
be evaluated for its necessity, configuration security, and
vulnerability to known exploits.

### Cleartext Communication Protocols

The use of cleartext communication protocols such as Telnet, HTTP,
FTP, MQTT (without TLS), and CoAP (without DTLS) represents a
significant risk for IoT devices. An attacker with access to the
network path between the IoT device and its communication peers can
passively capture credentials, configuration data, sensor readings,
control commands, and firmware updates. The IoTA team should capture
and analyze network traffic generated by the IoT device during
normal operation, initial setup, firmware update processes, and error
conditions to identify any data transmitted without encryption. Tools
such as Wireshark, tcpdump, and protocol-specific analyzers should
be used for traffic capture and analysis.

### Vulnerable Network Services

Each network service identified during enumeration should be evaluated
for known vulnerabilities. This includes checking the software version
against public vulnerability databases, testing for default or weak
credentials, evaluating the service configuration for
misconfigurations, and performing authenticated and unauthenticated
vulnerability scanning. IoT devices frequently run outdated versions
of common network services (such as dropbear SSH, lighttpd, busybox,
or mosquitto MQTT) that contain known, publicly documented
vulnerabilities. The IoTA team should also evaluate whether the
device's firmware update mechanism can address identified
vulnerabilities in a timely manner.

### Man-in-the-Middle (MITM) Attacks

The IoTA team should evaluate the susceptibility of IoT device
communications to man-in-the-middle attacks. This includes testing
whether the device validates TLS certificates when connecting to cloud
backends, whether the device is vulnerable to ARP spoofing or DNS
poisoning on the local network, and whether an attacker who can
position themselves in the network path can intercept, modify, or
inject data into communications between the device and its peers.
Tools such as Bettercap, mitmproxy, and Ettercap can be used to
perform and evaluate MITM attack scenarios.

### Wi-Fi Client Isolation Bypass

Recent research has demonstrated techniques for bypassing Wi-Fi client
isolation in both enterprise and consumer access points. IoT devices
deployed on shared networks may be accessible to other clients despite
isolation policies being configured on the access point. The IoTA team
should evaluate whether target IoT devices are affected by known
isolation bypass techniques, such as those documented in the AirSnitch
research (NDSS 2026), and should test whether the device's management
interface and services are accessible from other clients on the same
network segment when isolation is expected to be enforced.

### Network Segmentation and Lateral Movement

IoT devices are frequently deployed on the same network segments as
other enterprise or consumer systems. The IoTA team should evaluate
whether a compromised IoT device can be used as a pivot point to
attack other systems on the network. This includes evaluating the
device's ability to perform network scanning, whether it can be used
to relay traffic, and whether compromise of the device provides access
to credentials or network information that could facilitate lateral
movement. The team should also evaluate whether the device properly
isolates its management interfaces from its data interfaces and
whether it supports deployment in segmented network architectures.

### IoT Protocol Specific Testing (MQTT, CoAP, LwM2M)

MQTT and CoAP are foundational protocols for IoT device communication
and deserve dedicated testing beyond generic network service
evaluation. For MQTT implementations, the IoTA team should test broker
authentication mechanisms, topic enumeration and access control list
(ACL) bypass, QoS level manipulation, retained message abuse, and
whether the $SYS topic tree exposes sensitive broker information. Tools
such as MQTT Explorer, MQTT-PWN, and mosquitto_sub/pub facilitate
MQTT-specific testing. For CoAP implementations, the team should test
observe/subscribe functionality for abuse, block-wise transfer
manipulation, DTLS configuration weaknesses, and whether the .well-known/core
resource discovery endpoint exposes unnecessary information. LwM2M
(Lightweight M2M), widely used for cellular IoT device management,
should be evaluated for bootstrap security, object model enumeration,
and firmware update delivery security.

### DNS and Network Service Discovery

IoT devices often rely on DNS for resolving backend service endpoints,
and many use multicast DNS (mDNS) or Simple Service Discovery Protocol
(SSDP) for local network discovery. The IoTA team should evaluate
whether DNS communications are secured (through DNS over TLS or DNSSEC
validation), whether the device is susceptible to DNS rebinding
attacks, and whether mDNS or SSDP responses expose unnecessary
information about the device or its services. DNS rebinding attacks
are of particular concern for IoT devices because they can allow an
attacker to bypass browser same-origin policies and interact with
device management interfaces from a remote web page.

## **API Specific**

IoT ecosystems commonly rely on application programming interfaces
(APIs) to facilitate communication between devices, mobile
applications, cloud services, and third-party integrations. These APIs
often serve as the primary mechanism for device control, data
collection, user management, and firmware distribution. The IoTA team
should evaluate APIs associated with IoT devices and their supporting
infrastructure for vulnerabilities that could lead to unauthorized
access, data exposure, or device compromise.

### API Discovery and Documentation

The IoTA team should begin API assessment by identifying all API
endpoints associated with the IoT ecosystem. This includes APIs
documented in vendor-provided materials, APIs discovered through
analysis of mobile application binaries and web interface traffic, and
undocumented or "hidden" APIs identified through traffic analysis,
directory brute-forcing, or examination of JavaScript source code. The
team should evaluate whether API documentation (such as OpenAPI/Swagger
specifications) is publicly accessible without authentication and
whether such documentation reveals information about internal
endpoints, data models, or authentication mechanisms that could aid
an attacker.

### Authentication and Authorization

The IoTA team should evaluate the authentication mechanisms used to
protect API endpoints. Common weaknesses in IoT API authentication
include the use of static API keys that are shared across all devices
or users, lack of authentication on endpoints that perform sensitive
operations, use of basic authentication without TLS, weak or
predictable token generation, and failure to properly expire or rotate
authentication tokens. The team should also evaluate whether the API
enforces proper authorization checks, ensuring that authenticated
users can only access resources and perform actions appropriate to
their privilege level. Broken object-level authorization (BOLA), where
an authenticated user can access another user's devices or data by
manipulating resource identifiers, is a common and high-impact
vulnerability in multi-tenant IoT platforms.

### Input Validation and Injection

API endpoints should be tested for proper input validation across all
parameters, including URL path parameters, query string parameters,
request headers, and request body content. The IoTA team should test
for injection vulnerabilities including SQL injection, NoSQL injection,
command injection, and LDAP injection across all endpoints that accept
user-supplied input. APIs that process structured data formats such as
JSON, XML, or YAML should be tested for deserialization vulnerabilities
and format-specific injection attacks.

### Rate Limiting and Abuse Prevention

The IoTA team should evaluate whether API endpoints implement rate
limiting to prevent brute-force attacks, credential stuffing, denial
of service, and resource exhaustion. APIs that lack rate limiting on
authentication endpoints are particularly concerning, as they may allow
an attacker to enumerate valid credentials or perform offline attacks
against weak password policies. The team should also evaluate whether
the API implements protections against automated abuse, such as
progressive delays or account lockout mechanisms.

### Data Exposure

API responses should be evaluated for excessive data exposure, where
the API returns more information than is necessary for the requested
operation. Common examples in IoT APIs include device listing endpoints
that return detailed hardware information, firmware versions, network
configuration, and geographic location data for all devices associated
with an account; user profile endpoints that expose email addresses,
phone numbers, or physical addresses; and error responses that reveal
internal system details, database schemas, or stack traces. The IoTA
team should evaluate each API response to determine whether the data
returned is appropriate for the requesting user's privilege level and
whether sensitive data is properly redacted or omitted.

### SOAP, REST, GraphQL, and gRPC Considerations

IoT APIs may be implemented using various architectural styles and
protocols. REST APIs, the most common in modern IoT implementations,
should be evaluated for proper use of HTTP methods, status codes, and
resource naming conventions. SOAP-based APIs, sometimes found in
industrial and enterprise IoT systems, should be evaluated for
XML-specific vulnerabilities including XXE injection, WSDL
enumeration, and SOAPAction manipulation.

GraphQL APIs, increasingly adopted in IoT cloud platforms, present
unique security considerations. The IoTA team should evaluate GraphQL
implementations for introspection query exposure (which can reveal
the entire API schema), deeply nested query attacks that can cause
denial of service through resource exhaustion, batching attacks that
can bypass rate limiting, and field-level authorization bypasses
where a user can access restricted data through alternative query
paths. The team should also evaluate whether the GraphQL
implementation properly validates and sanitizes variables and
arguments to prevent injection attacks.

gRPC/Protobuf APIs are used by some modern IoT platforms for
high-performance device communication. The IoTA team should evaluate
gRPC implementations for service enumeration through reflection,
protobuf schema extraction, authentication bypass, and input
validation on protobuf message fields.

### Webhook and FOTA API Security

Many IoT platforms support webhooks or callback URLs for event
notifications. The IoTA team should evaluate whether webhook
configurations are properly authenticated, whether the platform
validates destination URLs to prevent SSRF attacks, and whether
webhook payloads contain sensitive data. FOTA (Firmware Over The Air)
API endpoints responsible for firmware distribution, version
checking, and update delivery should be specifically evaluated for
authentication bypass, unsigned firmware acceptance, version rollback
facilitation, and metadata injection.

## **Software Supply Chain Analysis**

Modern IoT firmware is assembled from dozens to hundreds of open-source
and commercial components. Supply chain compromise has become one of
the most impactful attack vectors against IoT ecosystems, as
demonstrated by incidents such as the BadBox 2.0 pre-installed malware
campaign affecting over 10 million smart devices. The EU Cyber
Resilience Act mandates SBOM creation and vulnerability reporting
starting September 2026, making supply chain analysis both a security
imperative and a regulatory requirement.

### SBOM Generation and Component Identification

For every target device, the IoTA team should generate a comprehensive
Software Bill of Materials from the extracted firmware using binary
analysis tools such as EMBA, FACT, or commercial platforms like Finite
State. The SBOM should document all identified components including
name, version, supplier or origin, license, and known CVE
associations, and should be produced in CycloneDX or SPDX format. The
complete dependency tree should be mapped, as transitive dependencies
can introduce vulnerabilities not apparent from top-level analysis.

### Vulnerability Mapping and Prioritization

All identified components should be cross-referenced against multiple
vulnerability databases including NVD, OSV, vendor-specific advisories,
and the CISA Known Exploited Vulnerabilities (KEV) catalog. Findings
should be prioritized based on exploitability in the context of the
target device (a vulnerable library may not be reachable through any
exposed interface) rather than raw CVSS score alone.

### Third-Party SDK and RTOS Evaluation

Chipset vendor SDKs (from Espressif, Nordic, Qualcomm, Silicon Labs,
TI, and others) carry their own vulnerability surface independent of
the application code. The IoTA team should identify the SDK version
in use, cross-reference it against known vulnerabilities, and evaluate
whether the vendor's SDK configuration introduces security weaknesses
(such as debug features left enabled, insecure default configurations,
or outdated bundled libraries). RTOS components should be similarly
evaluated.

### Build Environment Artifacts

Residual build environment artifacts in the firmware (compiler strings,
build paths, debug symbols, CI/CD configuration fragments, Git commit
hashes) should be documented as they may reveal information about the
vendor's development environment, internal network structure, and
supply chain practices.

## **Cloud and Backend Infrastructure**

Cloud backends are integral to modern IoT device operation. Compromise
of the backend often provides access to the entire deployed device
fleet, making cloud infrastructure a high-value target. The IoTA
methodology no longer defers cloud testing; it is a required component
of any comprehensive IoT security assessment.

### Cloud Service Identification and Testing

The IoTA team should identify the cloud provider(s) and services used
by the IoT vendor (AWS IoT Core, Azure IoT Hub, Google Cloud IoT, or
self-hosted alternatives). Device provisioning and registration
workflows, device shadow/twin manipulation, message broker (MQTT/AMQP)
authentication and authorization, and rule engine/event processing
security should all be evaluated. Tools such as ScoutSuite, Prowler,
and CloudSploit provide automated assessment of cloud security
configurations.

### Device-to-Cloud Authentication

The IoTA team should evaluate the authentication mechanisms used
between devices and cloud backends, including certificate-based mutual
TLS, token-based authentication, and SAS key mechanisms. Testing should
assess whether credentials are shared across device fleets (a single
compromised device yielding access to the entire population), whether
certificate or key rotation is implemented, and whether revocation
mechanisms function effectively.

### Data Storage and Multi-Tenant Isolation

The security of device telemetry storage, user data, and device
credentials at rest should be evaluated. The team should test for
unauthorized access through API manipulation, direct storage access
(such as misconfigured S3 buckets, Azure Blob containers, or GCS
buckets), and database injection. In multi-tenant IoT platforms,
tenant isolation should be rigorously tested to verify that one
customer's devices and data cannot be accessed by another.

## **AI and Machine Learning in IoT**

AI and ML models are increasingly deployed on IoT edge devices for
anomaly detection, predictive maintenance, voice and image recognition,
and autonomous decision-making. This introduces attack surfaces not
covered by traditional IoT testing.

### Model Extraction and Analysis

The IoTA team should attempt to extract ML models from device firmware
or runtime memory. Extracted models may reveal sensitive training data
(through model inversion attacks), proprietary algorithms, or
classification boundaries that can be exploited.

### Adversarial Input Testing

For devices that use ML for sensor data interpretation (image
recognition cameras, voice assistants, predictive maintenance sensors),
the IoTA team should evaluate susceptibility to adversarial inputs
designed to cause misclassification or incorrect decisions. For devices
where AI/ML models make safety-critical decisions (medical devices,
autonomous systems, industrial controllers), the robustness of the
decision pipeline against adversarial conditions should be evaluated
with particular rigor.

### AI-Assisted Analysis

The IoTA team should leverage AI/ML tools to augment their own testing
capabilities, including LLM-assisted firmware reverse engineering (for
decompiled code analysis and vulnerability pattern recognition),
AI-driven protocol fuzzing and test case generation, and automated
threat modeling using the device's architecture documentation and SBOM
as inputs. These tools are force multipliers for human analysts, not
replacements.

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

## **Medical IoT (IoMT) Specific**

Internet of Medical Things devices present unique risk profiles where
security vulnerabilities can directly impact patient safety. Regulatory
requirements from the FDA, IEC 81001-5-1, and EU MDR mandate security
testing as part of the product lifecycle.

### Patient Data and Safety

The IoTA team should evaluate compliance with HIPAA (US), GDPR (EU),
and equivalent regulations for handling of Protected Health Information
(PHI). Safety-critical function isolation should be evaluated to
determine whether vulnerabilities in non-critical device functions
(networking, UI, telemetry) can propagate to affect safety-critical
functions (drug delivery, physiological monitoring, life support).

### Medical Device Protocols and Lifecycle

Proprietary and standards-based wireless protocols used in medical
device ecosystems should be tested, including Medical Body Area
Networks (MBAN) and IEEE 802.15.6. The security of healthcare
interoperability protocol implementations such as HL7 FHIR and DICOM
should be evaluated. Device decommissioning procedures should be
assessed to verify that patient data and credentials are properly
sanitized before disposal or redistribution.

## **Industrial IoT (IIoT) and OT Convergence**

The convergence of IT and OT networks means that IoT devices
increasingly bridge into industrial control system environments. The
IoTA team should evaluate industrial protocol security, network
segmentation, and the potential for IoT devices to serve as entry
points into critical infrastructure.

### Industrial Protocol Testing

The security of industrial protocols should be evaluated, including
Modbus TCP/RTU, OPC UA, EtherNet/IP (CIP), PROFINET, BACnet (building
automation), and DNP3. For IoT devices that interact with or act as
programmable logic controllers (PLCs) or remote terminal units (RTUs),
the team should evaluate default credentials, firmware update security,
logic manipulation, and unauthorized read/write of process values.

### Safety and Segmentation

Any finding that could impact Safety Instrumented System (SIS)
integrity should be treated as critical regardless of other risk
factors. Adherence to the Purdue Model (ISA-95) for network
segmentation should be evaluated, testing whether IoT devices properly
reside within their designated network zones and whether they can be
used to traverse zone boundaries. Zero trust microsegmentation
principles should be assessed, evaluating whether compromised IoT
devices can access resources beyond their minimum required scope.

## **Physical Security and Side-Channel Attacks**

Modern IoT security increasingly relies on hardware security features
such as secure boot, secure elements, and trusted execution
environments. Evaluating these protections requires correspondingly
sophisticated physical attack techniques.

### Electromagnetic Side-Channel Analysis

The IoTA team should use near-field electromagnetic probes to capture
EM emissions from cryptographic operations on target devices. Captured
traces should be analyzed for key leakage through Simple Power Analysis
(SPA), Differential Power Analysis (DPA), and Correlation Power
Analysis (CPA). Tools such as the ChipWhisperer Husky provide
integrated hardware and software for side-channel capture and analysis.

### Fault Injection

Voltage glitching, clock glitching, electromagnetic fault injection
(EMFI using tools like PicoEMP), and laser fault injection (for
high-value targets) should be employed to test the robustness of
secure boot implementations, code read-out protection, and
authentication routines. Fault injection can bypass security features
that are otherwise resistant to software-based attacks.

### Timing Side-Channel Attacks

Cryptographic implementations should be evaluated for timing leakage
that could reveal key material or bypass authentication. Variable-time
comparison functions, branch-dependent timing variations, and
cache-timing attacks on devices with shared caches should be tested
where applicable.

## **Regulatory Compliance Testing**

The regulatory landscape for IoT security has matured significantly.
The IoTA team should map findings to relevant regulatory frameworks to
support compliance documentation, market access, and risk management
decisions.

### EU Cyber Resilience Act (CRA)

Findings should be mapped to CRA Annex I essential requirements.
Findings that constitute actively exploited vulnerabilities trigger
reporting obligations to ENISA within 24 hours (effective September
2026). SBOM completeness and accuracy should be evaluated against CRA
requirements.

### ETSI EN 303 645

Findings should be mapped to the 13 provisions of the European standard
for consumer IoT security: no universal default passwords, implement a
means to manage vulnerability reports, keep software updated, securely
store sensitive security parameters, communicate securely, minimize
exposed attack surfaces, ensure software integrity, ensure personal
data is secure, make systems resilient to outages, examine system
telemetry data, make it easy for users to delete user data, make
installation and maintenance easy, and validate input data.

### Additional Frameworks

Findings should be mappable to NIST Cybersecurity Framework 2.0
functions (Govern, Identify, Protect, Detect, Respond, Recover), the
OWASP IoT Top 10 for high-level categorization, CWE for vulnerability
classification, CVSS 4.0 for severity scoring, and MITRE ATT&CK for
Enterprise/ICS/Mobile for TTP mapping. Sector-specific mappings should
be applied where relevant: FDA premarket cybersecurity guidance and IEC
81001-5-1 for medical devices, ISO/SAE 21434 and UNECE R155/R156 for
automotive, IEC 62443 for industrial systems, and the FCC Cyber Trust
Mark for US consumer IoT.

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

## **Threat Modeling**

The IoTA team should incorporate structured threat modeling as a
precursor to active testing. Threat modeling performed before hands-on
testing focuses the team's effort on the highest-risk areas and ensures
that the limited time available for active testing is spent on the
attack paths most likely to yield impactful findings.

### Approach

The threat modeling process should begin with architectural
decomposition of the target IoT ecosystem, identifying all components,
trust boundaries, data flows, and external dependencies. The team
should use one or more structured methodologies to enumerate threats
systematically:

STRIDE (Spoofing, Tampering, Repudiation, Information Disclosure,
Denial of Service, Elevation of Privilege) is recommended for
system-level threat enumeration. Each component and data flow in the
architecture should be evaluated against all six STRIDE categories.

Attack trees are recommended for focused analysis of specific
high-value targets within the system (such as "extract firmware
encryption keys" or "gain unauthorized device control"). Attack trees
enumerate the paths an adversary could take to achieve a specific
objective, helping the team identify which paths are most feasible
and which security controls are most critical.

LINDDUN (Linkability, Identifiability, Non-repudiation, Detectability,
Disclosure of information, Unawareness, Non-compliance) should be
applied when privacy threats are a significant concern, such as for
devices that collect health data, location data, or behavioral
telemetry.

### AI-Assisted Threat Modeling

The IoTA team may leverage large language models and AI-assisted tools
to accelerate the threat modeling process. A device's architecture
documentation, SBOM, and protocol stack description can be provided as
input to an LLM to generate an initial threat model that the team then
refines based on their expertise and hands-on reconnaissance findings.
AI-assisted threat modeling is a force multiplier for experienced
analysts, not a replacement for domain expertise.

### Integration with Testing

The completed threat model should directly inform the testing plan.
Each identified threat should be mapped to a specific test case or
set of test cases. Threats that are assessed as high-risk but not
validated during active testing should be documented as untested
assumptions with a recommendation for future evaluation. Findings
from active testing should be traced back to the threat model to
verify that the model accurately predicted the discovered
vulnerabilities and to identify gaps in the model for future
improvement.

## **Responsible Disclosure and Coordination**

IoT security testing frequently identifies vulnerabilities that affect
deployed device populations. The IoTA team should follow responsible
disclosure practices that balance the urgency of protecting users with
the practical reality that IoT firmware updates are significantly more
difficult to deploy than software patches.

### Disclosure Timelines

IoT vulnerability disclosure timelines should account for the
challenges of embedded firmware update deployment. A 90-day disclosure
window, standard for software vulnerabilities, may be insufficient for
IoT devices that require coordinated firmware releases across multiple
hardware variants, carrier certification (for cellular devices),
regulatory re-testing, or physical site visits for devices that cannot
receive OTA updates. The IoTA team should negotiate disclosure
timelines on a case-by-case basis, considering the severity of the
vulnerability, the size of the affected device population, and the
vendor's demonstrated capacity to produce and deploy patches.

### Coordination with CERTs and PSIRTs

The IoTA team should engage with CERT/CC, ICS-CERT (for industrial
IoT), and the vendor's Product Security Incident Response Team (PSIRT)
where one exists. For vulnerabilities in third-party components (such
as a vulnerable version of an open-source library found in the device
firmware), the team should report the vulnerability to the component
maintainer, the device vendor, and the relevant vulnerability database
(NVD, OSV) independently.

### EU CRA Reporting Obligations

Under the EU Cyber Resilience Act (reporting obligations effective
September 2026), manufacturers are required to report actively
exploited vulnerabilities to ENISA within 24 hours. When the IoTA team
discovers a vulnerability during testing that they have reason to
believe is being actively exploited in the wild (for example, by
observing exploitation artifacts in the device firmware or finding the
vulnerability already documented in underground forums), they should
notify the vendor immediately and advise the vendor of their CRA
reporting obligation.

### Vulnerability Documentation

All reported vulnerabilities should include sufficient information for
the vendor to reproduce the finding, including: a clear description of
the vulnerability, the affected firmware versions and hardware
variants, step-by-step reproduction instructions, proof-of-concept
code or captured evidence, an assessment of the potential impact, and
recommended remediation approaches. The vulnerability documentation
should be sufficient for the vendor to develop and test a patch without
requiring further interaction with the testing team.

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

## **Device Identity, PKI, and Certificate Lifecycle**

Device identity is the foundation of modern IoT security architecture.
Every access control decision, every firmware update authorization,
and every cloud API call ultimately depends on the device being able
to prove that it is what it claims to be. The IoTA team should
evaluate the entire identity lifecycle from provisioning through
operation to revocation.

### Provisioning and Enrollment

The IoTA team should evaluate how device identity is established
during manufacturing and first use. Key questions include: Are
credentials (certificates, keys, tokens) provisioned during
manufacturing, during first-time setup, or through a just-in-time
enrollment process? Is the provisioning process itself secure (is the
manufacturing environment trusted, or could credentials be intercepted
or cloned during provisioning)? Are credentials unique per device, or
are they shared across a product line, a manufacturing batch, or an
entire fleet? The Meshtastic CVE-2025-52464 (duplicated private keys
across vendor-cloned devices) is a real-world example of provisioning
failure. The team should evaluate the entropy of generated key
material on the target platform, as constrained devices with limited
entropy sources may produce weak or predictable keys.

### Certificate Lifecycle

For devices using X.509 certificates (TLS mutual authentication,
Matter DAC, cloud IoT platform certificates), the IoTA team should
evaluate the full certificate lifecycle: What is the certificate
validity period? Can certificates be renewed over the air? What
happens when a certificate expires and the device cannot reach the
renewal service (does it fail open, fail closed, or enter a degraded
mode)? Is there an automated certificate management protocol (such as
EST, CMP, or ACME) or are certificates manually managed? Is the root
CA certificate updatable, or is it hardcoded (this is a crypto-agility
concern for post-quantum migration)?

### Credential Rotation and Revocation

The team should evaluate whether symmetric keys, API tokens, and
session credentials are rotated on a defined schedule. What happens to
active sessions during rotation? Can an administrator force rotation
across the fleet? For revocation, the team should evaluate whether a
compromised device's identity can be revoked from the cloud backend
and how quickly revocation propagates to the rest of the ecosystem.
Is there a Certificate Revocation List (CRL) or OCSP responder, and
does the device check it? What happens if the revocation infrastructure
is unreachable?

### Identity Spoofing and Cloning

The IoTA team should test whether an attacker with physical access to
one device can extract credentials sufficient to impersonate it or
create clones that are accepted as legitimate by the cloud backend and
other devices. This directly connects to the firmware extraction,
secure element analysis, and fault injection work described elsewhere
in this methodology. The team should evaluate the blast radius of a
single device identity compromise: does it provide access only to that
device's data, or does it grant access to fleet-wide resources?

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

## **Denial of Service and Availability Testing**

For many IoT deployments, availability is the critical security
property. A smart lock that can be crashed remotely, a medical monitor
that can be taken offline, or an industrial controller that can be
forced into a fault state represent threats with immediate real-world
consequences. The IoTA team should evaluate the device's resilience to
denial of service attacks across all layers.

### Resource Exhaustion

Constrained IoT devices with limited memory (often 64KB-512KB RAM),
limited flash storage, and limited processing power are vulnerable to
resource exhaustion attacks. The IoTA team should test: memory
exhaustion through rapid connection establishment, large payload
delivery, or memory leak exploitation; CPU exhaustion through
computationally expensive operations (cryptographic operations,
complex query processing, deeply nested data structure parsing);
flash storage filling through excessive log generation, telemetry
accumulation, or configuration write attacks; and connection table
overflow through rapid establishment of connections that are never
properly closed.

### Network and Application Layer DoS

The IoTA team should test the device's resilience to network-layer
attacks including SYN floods against TCP services, UDP amplification,
MQTT message flooding (high-rate publish to subscribed topics), BLE
advertisement flooding (filling the device's advertisement scan
buffer), Zigbee network key rotation storms, and CAN bus flooding
(high-rate message transmission that forces the target ECU into
bus-off state). Application-layer DoS testing should include deeply
nested JSON or XML payloads, oversized firmware update metadata,
GraphQL query depth bombs, and algorithmic complexity attacks against
any search, sort, or parsing functionality.

### RF Layer Denial of Service

The IoTA team should evaluate the device's behavior when its primary
wireless communication channel is jammed. Using appropriate SDR
hardware, the team should generate targeted interference on the
device's operating frequency and evaluate: Does the device detect
the jamming condition? Does it fail safely (maintaining its current
state) or unsafely (defaulting to an unlocked, open, or uncontrolled
state)? Does it fall back to a secondary communication channel, and
if so, is that fallback channel less secure? Does it alert the user
or cloud backend? How quickly does it recover when jamming stops?

### Recovery and Resilience

After any DoS condition (whether induced through resource exhaustion,
network flooding, or RF jamming), the IoTA team should evaluate the
device's recovery behavior: Does it recover automatically without
manual intervention? Does it require a power cycle or factory reset?
Does it lose configuration, credential state, or accumulated data
during recovery? How long does recovery take? Is the device vulnerable
to repeated DoS attacks that prevent it from completing recovery?

### Botnet Recruitment Assessment

The IoTA team should evaluate whether a compromised device could be
co-opted into a DDoS botnet (the Aisuru/TurboMirai botnet achieved
20+ Tbps DDoS capability using IoT devices). Evaluation should cover:
Does the device have sufficient network bandwidth and processing
capacity to generate meaningful DDoS traffic? Are there access
controls or resource limitations that would prevent an attacker from
installing persistent malware? Can the device's outbound traffic be
monitored or restricted by network-level controls?

## **Default Credentials and Authentication Hygiene**

Weak, guessable, or hardcoded passwords are ranked as the number one
IoT security risk by the OWASP IoT Top 10, and approximately 20% of
IoT devices still ship with default credentials. The IoTA team should
systematically evaluate credential security across all device
interfaces.

### Default Credential Enumeration

The IoTA team should test all exposed services (SSH, Telnet, HTTP
management, MQTT broker, FTP, TFTP, SNMP, UPnP, debug consoles,
serial console) against known default credential databases. Sources
for default credentials include vendor documentation, datasheets,
support forums, default credential databases (such as
cirt.net/passwords, default-password.info), and credentials discovered
during firmware analysis. The team should also test common weak
credentials (admin/admin, root/root, admin/password, and device-
specific patterns such as serial number-derived passwords).

### Credential Uniqueness Assessment

The IoTA team should determine whether credentials are unique per
device or shared across the product line. If the team has access to
multiple units of the same device, they should compare default
credentials across units. Shared credentials across a product line
represent a fleet-wide vulnerability: compromise of one device's
credentials immediately compromises every device of that model. ETSI
EN 303 645 Provision 1 explicitly prohibits universal default
passwords.

### Change Enforcement and Password Policy

The team should evaluate whether the device forces the user to change
default credentials during initial setup. A device that allows
continued operation with default credentials fails ETSI EN 303 645
Provision 1. The team should also evaluate password complexity
requirements, whether the device prevents the use of common or
trivially guessable passwords, and whether credentials are transmitted
securely during the change process.

### Factory Reset and Credential Recovery

The IoTA team should evaluate the security of the factory reset
mechanism itself. Does factory reset actually erase all user
credentials, configuration data, network keys, and cached data? The
team should perform a factory reset and then dump the device's flash
storage to check for residual data that survived the reset. The team
should also evaluate whether the factory reset mechanism can be
triggered by an attacker (physically or remotely) as a denial of
service attack or as a means to restore known default credentials.

## **Data Protection**

Data protection encompasses the security of all data collected,
processed, stored, and transmitted by the IoT device throughout its
lifecycle. The IoTA team should evaluate data protection as a holistic
property rather than addressing transport security and storage
security in isolation.

### Data Inventory and Classification

Before evaluating data protection mechanisms, the IoTA team should
inventory all data the device handles. Categories include: sensor and
telemetry data (environmental readings, usage patterns, behavioral
data), user personally identifiable information (names, email
addresses, account credentials, physical addresses), authentication
credentials (device certificates, API keys, session tokens, network
keys), device configuration (network settings, operational parameters,
firmware version), and operational logs (access logs, error logs, event
history). Each data category should be classified by sensitivity and
evaluated against the protection mechanisms applied to it.

### Data at Rest

The IoTA team should evaluate whether sensitive data is encrypted on
the device's storage medium. The team should extract the device's
storage (via SPI flash dump, eMMC extraction, or filesystem access)
and determine whether data can be read in plaintext. If encryption is
applied, the team should evaluate the encryption algorithm, key
management scheme (where is the encryption key stored? can it be
extracted?), and whether the encryption covers all sensitive data or
only a subset. Flash wear-leveling and bad block management on NAND-
based storage can leave copies of sensitive data in areas that are
not covered by application-level encryption.

### Data Minimization and Retention

The IoTA team should evaluate whether the device collects more data
than necessary for its stated function (over-collection creates
unnecessary risk and may violate GDPR data minimization requirements).
The team should also evaluate data retention policies: How long is
data stored on the device? How long is data retained in the cloud
backend? Can the user request deletion of their data, and is deletion
actually effective (is data wiped from storage, or can it be recovered
through forensic analysis)?

### Data Deletion Verification

When a user deletes data (through the management interface, factory
reset, or account deletion in the cloud), the IoTA team should verify
that the data is actually removed. For on-device storage, dump the
flash after deletion and search for residual data. For cloud storage,
verify that API endpoints no longer return the deleted data and that
it is not recoverable through alternative API calls, backup systems,
or cached responses.

## **Post-Quantum Cryptography Readiness**

IoT devices manufactured today will remain in the field for 5 to 15
years or more. NIST has finalized post-quantum cryptographic standards
(ML-KEM for key encapsulation, ML-DSA for digital signatures, SLH-DSA
for hash-based signatures). Devices with hardcoded cryptographic
algorithms that cannot be updated will become vulnerable within their
operational lifetime. The IoTA team should evaluate PQC readiness as
part of any assessment of devices intended for long-term deployment.

### Crypto-Agility Assessment

The team should evaluate whether the device's cryptographic algorithms
can be updated via firmware update or whether they are hardcoded in
hardware (ROM, ASIC, or hardwired in an FPGA bitstream). Devices
without crypto-agility, where the root CA certificate, signature
verification algorithm, or key exchange algorithm cannot be changed
without hardware replacement, face a ticking clock as quantum
computing matures.

### Resource Feasibility

PQC algorithms require significantly more memory and computational
resources than current algorithms. ML-KEM public keys are
approximately 800-1500 bytes (vs. 32-91 bytes for ECDH), and ML-DSA
signatures are approximately 2400-4600 bytes (vs. 64-132 bytes for
ECDSA). The IoTA team should evaluate whether the target device's
hardware (RAM, flash, CPU) has sufficient resources to support PQC
algorithms, and whether the communication channels have sufficient
bandwidth for the larger key and signature sizes.

### Harvest-Now-Decrypt-Later Risk

For devices transmitting long-lived sensitive data (medical records,
industrial trade secrets, financial transactions, government
communications), the team should evaluate the risk that an adversary
captures encrypted traffic today and decrypts it with a future quantum
computer. This risk is highest for devices using static ECDH or RSA
key exchange (where a single key compromise decrypts all captured
traffic) and lowest for devices using ephemeral key exchange with
perfect forward secrecy.

## **Physical Tamper Resistance**

The existing physical security and side-channel analysis sections
address sophisticated electronic attacks. This section covers the
foundational physical security evaluation that should precede any
advanced analysis.

### Tamper Evidence

The IoTA team should evaluate whether the device uses tamper-evident
mechanisms that make physical intrusion visible: security screws
(Torx with center pin, pentalobe, tri-wing), tamper-evident seals or
labels (holographic, frangible), ultrasonic welding of enclosure
halves (no visible screw access), or conformal coating on the PCB.
The team should attempt to open the device, access the PCB, and
reseal the enclosure without leaving visible evidence of entry. If
this is achievable, the tamper-evidence mechanism has failed.

### Tamper Detection and Response

The team should evaluate whether the device has active tamper
detection mechanisms: microswitches or magnetic switches that detect
enclosure opening, flex circuit wraps around security-critical
components that detect physical breach, voltage and clock monitoring
that detects glitch injection attempts, or temperature sensors that
detect localized heating from soldering or hot air rework. When tamper
is detected, the team should evaluate the response: Does the device
zeroize cryptographic keys? Does it alert the cloud backend? Does it
enter a lockdown mode? Or does it simply log the event (if it logs
at all)?

### Conformal Coating and Potting

The team should evaluate whether conformal coating is applied to the
PCB and whether it can be removed without damaging components.
Conformal coating can typically be removed with isopropyl alcohol,
acetone, or specialized conformal coating remover, followed by gentle
scraping. Potting compound (epoxy encapsulation over security-critical
components) is more difficult to defeat but can sometimes be removed
with heat, solvents, or careful mechanical work. The team should
document the type and coverage of any protective coatings encountered.

### Hardware Implant Feasibility

From the attacker's perspective, the IoTA team should evaluate
whether a hardware implant (UART tap, SPI interposer, wireless
bridge, keylogger) could be installed inside the device enclosure and
the enclosure resealed without obvious evidence of tampering. This
assessment considers the available physical space inside the
enclosure, access to relevant bus signals, and power supply
availability for the implant.

## **Logging, Monitoring, and Incident Detection**

A pentest report should include findings about the device's ability
(or inability) to detect that it is being attacked. The absence of
logging and monitoring capability is itself a significant finding.

### Security Event Logging

The IoTA team should evaluate whether the device logs security-
relevant events: authentication failures (and whether repeated
failures trigger lockout), configuration changes (who changed what,
when), firmware update events (success, failure, version changes),
debug interface access, network connection establishment and
termination, and anomalous conditions (unexpected reboots, watchdog
resets, storage errors). The team should evaluate where logs are
stored (on-device flash, RAM, external syslog), whether logs survive
reboot, and whether an attacker who has compromised the device can
tamper with or delete log entries.

### Log Forwarding and External Monitoring

The team should evaluate whether the device can forward security
events to a central monitoring system (syslog, MQTT telemetry channel,
cloud API endpoint, SIEM integration). Is the log forwarding channel
itself encrypted and authenticated? Can an attacker suppress log
forwarding to hide their activity?

### Forensic Readiness

After a suspected compromise, the team should evaluate what evidence
would be available for forensic analysis: Can firmware integrity be
verified against a known-good image? Are logs preserved with
sufficient detail to reconstruct the attacker's actions? Is there a
hardware-backed secure audit trail that cannot be modified by software
running on the device? ETSI EN 303 645 Provision 10 requires that
telemetry data be examined for security anomalies; the IoTA team
should verify that sufficient telemetry is generated and accessible
to satisfy this requirement.

## **Device Lifecycle and Decommissioning Security**

IoT devices have lifespans measured in years or decades, and they pass
through multiple stages: manufacturing, provisioning, deployment,
operation, maintenance, ownership transfer, and end-of-life disposal.
Security vulnerabilities can emerge at any stage transition.

### Secure Factory Reset

The IoTA team should evaluate whether a factory reset actually erases
all user data, credentials, network keys, Wi-Fi passwords, cloud
account associations, and cached data. The team should perform a
factory reset and then dump the device's storage to check for
residual data. On flash-based storage, data that has been logically
deleted may still be physically present due to flash wear-leveling
algorithms. The team should also evaluate whether the factory reset
mechanism itself is secure: Can it be triggered remotely by an
attacker? Can it be protected by a PIN or password? Does it require
physical access?

### Ownership Transfer

When a device changes ownership (resale, return, redeployment), the
IoTA team should evaluate whether the previous owner's access is
fully revoked. Key questions include: Can the previous owner's cloud
account still see or control the device after transfer? Are the
device's Wi-Fi credentials, Bluetooth pairings, and Matter fabric
memberships cleared during transfer? Is there a formal ownership
transfer process, or must the new owner perform a factory reset and
hope it actually works?

### End-of-Life Security

When a vendor discontinues a product or ceases operations, the IoTA
team should evaluate the implications for deployed devices: What
happens to cloud-dependent functionality when the backend is shut
down? Does the device become nonfunctional, or does it continue to
operate in a degraded (and potentially less secure) mode? Are
firmware updates still available? Is the SBOM still monitored for new
CVEs? Who is responsible for security vulnerabilities discovered after
end-of-life?

## **Cross-Layer Attack Chain Methodology**

The IoTA methodology evaluates each protocol and layer independently.
In practice, the most impactful attacks chain vulnerabilities across
multiple layers. The IoTA team should explicitly identify and test
cross-layer attack paths.

### Attack Chain Identification

During threat modeling, the IoTA team should identify attack chains
that span multiple layers of the IoT ecosystem. Common patterns
include: BLE commissioning compromise leading to cloud account
takeover (extract device credentials during BLE setup, use them to
access the cloud backend, pivot to other devices on the same account);
firmware update interception leading to persistent backdoor
installation (MITM the update channel, deliver modified firmware,
use the backdoor for ongoing access); web interface exploitation
leading to physical harm (exploit a web vulnerability to change
physical device parameters such as thermostat setpoints, motor speed
limits, or valve positions); and supply chain compromise leading to
fleet-wide exploitation (a vulnerability in a shared SDK component
exploitable across all devices using that component).

### End-to-End Scenario Testing

The IoTA team should develop and execute end-to-end test scenarios
that exercise identified attack chains. Each scenario should begin
with an initial access technique (physical access, network proximity,
internet-facing attack surface) and proceed through exploitation,
lateral movement, and objective achievement. The results demonstrate
the real-world impact of individual vulnerabilities when combined,
which is often significantly greater than the sum of their individual
risk ratings.

## **Engagement Scoping and Methodology**

The IoTA methodology defines what to test. This section provides
guidance on how to scope an engagement, estimate effort, and structure
the testing approach.

### Scoping Questionnaire

Before an engagement, the IoTA team should gather the following
information from the client: device architecture overview (hardware
platform, RTOS or OS, communication protocols, cloud backend, mobile
app), number of device variants and firmware versions in scope,
whether physical access to the device is permitted (and the level of
physical destructiveness allowed: non-invasive only, PCB probing,
chip desoldering, fault injection), whether the cloud backend and
mobile application are in scope, any regulatory frameworks the device
must comply with (CRA, ETSI EN 303 645, FDA, IEC 62443), and
available documentation (schematics, source code, SDKs, prior
assessment reports).

### Effort Estimation

Testing effort varies dramatically by device complexity. A simple BLE
sensor with no cloud connectivity may require 3-5 days. A multi-
protocol gateway with a cloud backend, mobile app, and web management
interface may require 15-25 days. A connected vehicle ECU with CAN-FD,
Automotive Ethernet, cellular connectivity, and OTA update capability
may require 20-40 days. The presence of secure boot, secure elements,
and hardware tamper protection increases effort due to the time
required for advanced physical attacks.

### Rules of Engagement

The IoTA team should establish clear rules of engagement covering:
whether testing against production cloud infrastructure is permitted
(or whether a staging environment will be provided), notification
procedures for critical findings that require immediate vendor
action, handling of vulnerabilities discovered in third-party
components (disclosure to the component maintainer in addition to
the client), physical device return condition expectations, and data
handling requirements for any sensitive data encountered during
testing.

## **Retesting and Regression**

Many clients require retesting after remediation of identified
vulnerabilities. The IoTA team should have a structured approach to
retesting that verifies remediation without requiring a full repeat
of the original assessment.

### Retest Methodology

For each previously identified finding, the team should attempt to
reproduce the original vulnerability using the same technique
documented in the original report. If the vulnerability is no longer
reproducible, the team should evaluate the remediation approach to
verify that the root cause has been addressed (rather than just the
specific reproduction steps). The team should also evaluate whether
the remediation has introduced any new attack surface or changed the
behavior of adjacent functionality.

### Firmware Diff Analysis

When comparing a remediated firmware image to the original, the IoTA
team should use SBOM diff tools (EMBA, FACT, or binary diff
utilities) to identify all changes between versions. Each change
should be evaluated for security relevance: Has the vulnerable
component been updated? Have new components been introduced? Have
any security controls been weakened as a side effect of the
remediation?

## **Conclusions**

The Internet of Things represents a convergence of disciplines
including embedded systems engineering, wireless communications, web
application development, mobile development, cloud infrastructure, and
API design. The security of any IoT ecosystem depends on the security
of each of these components individually as well as their interactions
with one another. A vulnerability in any single component can cascade
through the ecosystem, affecting device integrity, user privacy, and
the broader networks to which these devices are connected.

The IoTA Red Team recommends that IoT vendors and the organizations
that deploy their products adopt a comprehensive, ongoing approach to
security testing that addresses each layer of the IoT ecosystem. The
following security exercises should be performed regularly by qualified
teams:

-   Physical Security Penetration Testing: Evaluation of
    tamper-protection mechanisms, physical access controls, and the
    security of debug and programming interfaces against an attacker
    with physical possession of the device.

-   Embedded Device Security Analysis: Comprehensive firmware
    extraction, static and dynamic analysis, cryptographic evaluation,
    SBOM generation, and vulnerability assessment of the device hardware
    and software.

-   RF and Wireless Security Assessment: Characterization and security
    evaluation of all radio frequency communications, including protocol
    analysis, encryption evaluation, and testing for replay,
    impersonation, and jamming attacks across all wireless protocols in
    use (BLE, Zigbee, Thread, Matter, LoRa/Meshtastic, Wi-Fi, cellular,
    and proprietary).

-   Web Application Penetration Testing: Security assessment of both
    local device management interfaces and cloud-hosted management
    portals for injection vulnerabilities, authentication weaknesses,
    session management flaws, and other web application security issues.

-   Mobile Application Security Assessment: Static and dynamic analysis
    of companion mobile applications, including evaluation of data
    storage practices, communication security, third-party library
    vulnerabilities, and certificate pinning implementations.

-   Network Security Assessment: Evaluation of all network services
    exposed by IoT devices, transport security, susceptibility to
    man-in-the-middle attacks, and the potential for compromised devices
    to facilitate lateral movement.

-   API Security Assessment: Comprehensive testing of all APIs within
    the IoT ecosystem for authentication and authorization weaknesses,
    injection vulnerabilities, data exposure, and abuse prevention
    mechanisms.

-   Software Supply Chain Evaluation: Assessment of third-party
    components, libraries, and SDKs used throughout the IoT ecosystem
    for known vulnerabilities and risks associated with software supply
    chain compromise. SBOM generation and ongoing vulnerability
    monitoring should be standard practice.

-   Cloud and Backend Infrastructure Assessment: Evaluation of cloud
    service configurations, device-to-cloud authentication, data
    storage security, and multi-tenant isolation for IoT backend
    systems.

-   Regulatory Compliance Mapping: Mapping of findings to applicable
    regulatory frameworks (EU CRA, ETSI EN 303 645, NIST CSF 2.0,
    sector-specific standards) to support compliance documentation and
    market access decisions.

-   Device Identity and Credential Lifecycle Assessment: Evaluation of
    device provisioning, certificate management, credential rotation,
    and revocation mechanisms across the entire device lifecycle from
    manufacturing through decommissioning.

-   Availability and Resilience Testing: Evaluation of device behavior
    under denial of service conditions including resource exhaustion,
    network flooding, RF jamming, and recovery testing to verify that
    devices fail safely and recover gracefully.

-   Fuzzing and Input Validation Assessment: Systematic fuzzing of all
    identified protocols, file formats, hardware interfaces, and state
    machines to discover memory corruption, crash, and logic
    vulnerabilities.

-   Data Protection Assessment: Evaluation of data handling practices
    including data at rest encryption, data minimization, retention
    policies, deletion effectiveness, and post-quantum cryptography
    readiness for long-lived deployments.

Security must be designed into IoT products from the earliest stages
of development and validated through testing at every stage of the
product lifecycle, including design, manufacturing, distribution,
deployment, operation, and end-of-life. The cost of addressing
security vulnerabilities increases dramatically the later they are
discovered, and vulnerabilities in deployed IoT devices may be
difficult or impossible to remediate if the device lacks a secure and
reliable update mechanism.

The IoTA methodology is intended to be a living document, updated as
the IoT landscape evolves, new vulnerability classes emerge, new
protocols and technologies are adopted, and new tools and techniques
become available to both attackers and defenders. Contributions from
the security research community are welcomed and encouraged.

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
