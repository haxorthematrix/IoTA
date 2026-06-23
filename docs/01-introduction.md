[← Back to IoTA Index](../README.md)

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
