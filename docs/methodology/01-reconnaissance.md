[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

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
