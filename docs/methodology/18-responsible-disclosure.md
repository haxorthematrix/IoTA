[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

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
