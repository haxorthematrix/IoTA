[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

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
