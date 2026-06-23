[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

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
