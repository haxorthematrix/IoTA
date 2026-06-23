[← Back to IoTA Index](../../README.md)

# Methodology

This is the core of the IoTA methodology: a catalogue of testing domains,
techniques, and vulnerability classes spanning the entire IoT ecosystem —
from physical hardware and radio interfaces up through firmware, network
services, cloud backends, and the program-level processes that surround an
engagement.

Each section below is a self-contained document. Sections do not need to be
read in order; scope a given engagement to the device classes and surfaces
that are relevant (see [Engagement Scoping](29-engagement-scoping.md)).

## Reconnaissance & Hardware

- [01 · Reconnaissance](01-reconnaissance.md) — OSINT, PCB/chip inspection, external device inspection, RF survey
- [02 · Hardware-Specific Testing](02-hardware.md) — tampering, potting removal, cryptography, debug interfaces, buses, glitching, firmware dumping, static & dynamic firmware analysis

## Wireless & RF

- [03 · RF / Wireless](03-rf-wireless.md) — RF characterization, sniffing, replay, jamming, reverse engineering, BLE, Meshtastic/LoRa, Matter/Thread, Cellular IoT

## Software, App & Network Surfaces

- [04 · Web Application](04-web-application.md)
- [05 · Mobile Application](05-mobile-application.md)
- [06 · Network](06-network.md)
- [07 · API](07-api.md)
- [08 · Software Supply Chain Analysis](08-software-supply-chain.md)
- [09 · Cloud & Backend Infrastructure](09-cloud-backend.md)
- [10 · AI & Machine Learning in IoT](10-ai-ml.md)

## Domain-Specific Verticals

- [11 · Automotive IoT](11-automotive.md)
- [12 · Medical IoT (IoMT)](12-medical-iomt.md)
- [13 · Industrial IoT (IIoT) & OT Convergence](13-industrial-iiot-ot.md)

## Cross-Cutting Techniques

- [14 · Physical Security & Side-Channel Attacks](14-physical-side-channel.md)
- [19 · Traffic Capture, Protocol Sniffing & Analysis](19-traffic-capture-analysis.md)
- [20 · Device Identity, PKI & Certificate Lifecycle](20-device-identity-pki.md)
- [21 · Fuzzing](21-fuzzing.md)
- [22 · Denial of Service & Availability Testing](22-denial-of-service.md)
- [23 · Default Credentials & Authentication Hygiene](23-default-credentials.md)
- [24 · Data Protection](24-data-protection.md)
- [25 · Post-Quantum Cryptography Readiness](25-post-quantum-crypto.md)
- [26 · Physical Tamper Resistance](26-physical-tamper-resistance.md)
- [27 · Device Lifecycle & Decommissioning Security](27-device-lifecycle-decommissioning.md)
- [28 · Cross-Layer Attack Chain Methodology](28-cross-layer-attack-chains.md)

## Compliance, Process & Program

- [15 · Regulatory Compliance Testing](15-regulatory-compliance.md)
- [16 · Constructing a Lab](16-constructing-a-lab.md)
- [17 · Threat Modeling](17-threat-modeling.md)
- [18 · Responsible Disclosure & Coordination](18-responsible-disclosure.md)
- [29 · Engagement Scoping & Methodology](29-engagement-scoping.md)
- [30 · Retesting & Regression](30-retesting-regression.md)
