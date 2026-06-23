[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

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
