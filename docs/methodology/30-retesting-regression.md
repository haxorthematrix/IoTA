[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

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
