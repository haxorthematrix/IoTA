# Internet of Things Attack (IoTA) Methodology

A comprehensive, vendor-neutral methodology for the security testing of
Internet of Things (IoT) devices and their supporting ecosystems —
hardware, firmware, radio interfaces, mobile and web applications, APIs,
cloud backends, and the program-level processes that surround an engagement.

IoTA describes the **IoTA Red Team** approach to evaluating IoT
technologies end to end. It is written both for security teams and
penetration testers who perform the testing, and for the utilities and
vendors who commission it and need to understand what to expect from a
qualified attack team.

> **Scope in one sentence:** equipment placed in the hands of consumers
> (residential or enterprise) using embedded architectures not physically
> protected by on-premise security — the device and its firmware, the data
> streams and upstream network gear, the backend storage and services, the
> companion mobile apps, the wired and wireless communication media, and the
> software supply chain underpinning it all.

The methodology aligns with the IoT definitions in **NIST SP 800-183**
(Networks of Things) and is mindful of the evolving regulatory landscape,
including the EU Cyber Resilience Act (CRA), US EO 14028, NIST CSF 2.0,
ETSI EN 303 645, the FCC Cyber Trust Mark, UNECE WP.29 R155/R156, IEC 62443,
and FDA premarket cybersecurity guidance.

## How this repository is organized

This document was previously a single monolithic file. It has been broken
out into focused, individually readable sections under [`docs/`](docs/).
Start here and follow the links, or jump straight to a testing domain in the
[Methodology index](docs/methodology/README.md).

### Front matter

1. [Introduction](docs/01-introduction.md) — purpose, scope, the NoT
   primitives, and an executive summary of the IoT threat landscape.
2. [Principles of IoT Vulnerability Assessment](docs/02-principles.md) —
   practical and pertinent analysis, team expertise, reproducible findings,
   risk evaluation, and opportunities for defensive measures.

### Methodology

3. **[Methodology](docs/methodology/README.md)** — the core catalogue of
   testing domains and techniques, organized into:
   - *Reconnaissance & Hardware* — OSINT, PCB/chip inspection, debug
     interfaces, glitching, firmware dumping, static & dynamic analysis
   - *Wireless & RF* — RF characterization, sniffing/replay/jamming,
     proprietary RF reverse engineering, BLE, LoRa/Meshtastic, Matter/Thread,
     Cellular IoT
   - *Software, App & Network Surfaces* — web, mobile, network, API, software
     supply chain, cloud/backend, and AI/ML
   - *Domain-Specific Verticals* — Automotive, Medical (IoMT), Industrial
     (IIoT) & OT
   - *Cross-Cutting Techniques* — traffic capture, PKI & device identity,
     fuzzing, DoS, default credentials, data protection, post-quantum crypto,
     tamper resistance, device lifecycle, cross-layer attack chains
   - *Compliance, Process & Program* — regulatory compliance, lab
     construction, threat modeling, responsible disclosure, engagement
     scoping, retesting & regression

### Back matter

4. [Conclusions](docs/04-conclusions.md) — recommended recurring security
   exercises and the case for security designed in at every stage.
5. [Appendix A: Tool Reference](docs/05-appendix-a-tool-reference.md) —
   hardware and software tooling by category, plus suggested starter and
   professional lab kits.

## Who should read this

- **IoT security teams and penetration testers** — as a working testing
  methodology and reference.
- **IoT vendors** — to understand the vulnerabilities and attacks against
  their products so they can build security in.
- **Utilities and operators** — to know which skill sets to enlist when
  testing their own deployments and to keep testing consistent across
  different vendor architectures.

## License

This work is licensed under the [Creative Commons Attribution 4.0 International License](LICENSE) (CC BY 4.0).
You are free to share and adapt it for any purpose, including commercial use, provided you give appropriate credit.

## Contributing

The IoT threat landscape changes constantly as new protocols, devices, and
techniques emerge. Contributions from the security research community are
welcomed and encouraged. Each section under [`docs/`](docs/) is a standalone
Markdown file; edits and additions can be made section by section.

---

*Note: the original single-file version of this document remains available
in the project's git history prior to the section split.*
