[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Engagement Scoping and Methodology**

The IoTA methodology defines what to test. This section provides
guidance on how to scope an engagement, estimate effort, and structure
the testing approach.

### Scoping Questionnaire

Before an engagement, the IoTA team should gather the following
information from the client: device architecture overview (hardware
platform, RTOS or OS, communication protocols, cloud backend, mobile
app), number of device variants and firmware versions in scope,
whether physical access to the device is permitted (and the level of
physical destructiveness allowed: non-invasive only, PCB probing,
chip desoldering, fault injection), whether the cloud backend and
mobile application are in scope, any regulatory frameworks the device
must comply with (CRA, ETSI EN 303 645, FDA, IEC 62443), and
available documentation (schematics, source code, SDKs, prior
assessment reports).

### Effort Estimation

Testing effort varies dramatically by device complexity. A simple BLE
sensor with no cloud connectivity may require 3-5 days. A multi-
protocol gateway with a cloud backend, mobile app, and web management
interface may require 15-25 days. A connected vehicle ECU with CAN-FD,
Automotive Ethernet, cellular connectivity, and OTA update capability
may require 20-40 days. The presence of secure boot, secure elements,
and hardware tamper protection increases effort due to the time
required for advanced physical attacks.

### Rules of Engagement

The IoTA team should establish clear rules of engagement covering:
whether testing against production cloud infrastructure is permitted
(or whether a staging environment will be provided), notification
procedures for critical findings that require immediate vendor
action, handling of vulnerabilities discovered in third-party
components (disclosure to the component maintainer in addition to
the client), physical device return condition expectations, and data
handling requirements for any sensitive data encountered during
testing.
