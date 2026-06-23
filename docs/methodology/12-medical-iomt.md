[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

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
