[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Physical Tamper Resistance**

The existing physical security and side-channel analysis sections
address sophisticated electronic attacks. This section covers the
foundational physical security evaluation that should precede any
advanced analysis.

### Tamper Evidence

The IoTA team should evaluate whether the device uses tamper-evident
mechanisms that make physical intrusion visible: security screws
(Torx with center pin, pentalobe, tri-wing), tamper-evident seals or
labels (holographic, frangible), ultrasonic welding of enclosure
halves (no visible screw access), or conformal coating on the PCB.
The team should attempt to open the device, access the PCB, and
reseal the enclosure without leaving visible evidence of entry. If
this is achievable, the tamper-evidence mechanism has failed.

### Tamper Detection and Response

The team should evaluate whether the device has active tamper
detection mechanisms: microswitches or magnetic switches that detect
enclosure opening, flex circuit wraps around security-critical
components that detect physical breach, voltage and clock monitoring
that detects glitch injection attempts, or temperature sensors that
detect localized heating from soldering or hot air rework. When tamper
is detected, the team should evaluate the response: Does the device
zeroize cryptographic keys? Does it alert the cloud backend? Does it
enter a lockdown mode? Or does it simply log the event (if it logs
at all)?

### Conformal Coating and Potting

The team should evaluate whether conformal coating is applied to the
PCB and whether it can be removed without damaging components.
Conformal coating can typically be removed with isopropyl alcohol,
acetone, or specialized conformal coating remover, followed by gentle
scraping. Potting compound (epoxy encapsulation over security-critical
components) is more difficult to defeat but can sometimes be removed
with heat, solvents, or careful mechanical work. The team should
document the type and coverage of any protective coatings encountered.

### Hardware Implant Feasibility

From the attacker's perspective, the IoTA team should evaluate
whether a hardware implant (UART tap, SPI interposer, wireless
bridge, keylogger) could be installed inside the device enclosure and
the enclosure resealed without obvious evidence of tampering. This
assessment considers the available physical space inside the
enclosure, access to relevant bus signals, and power supply
availability for the implant.

## **Logging, Monitoring, and Incident Detection**

A pentest report should include findings about the device's ability
(or inability) to detect that it is being attacked. The absence of
logging and monitoring capability is itself a significant finding.

### Security Event Logging

The IoTA team should evaluate whether the device logs security-
relevant events: authentication failures (and whether repeated
failures trigger lockout), configuration changes (who changed what,
when), firmware update events (success, failure, version changes),
debug interface access, network connection establishment and
termination, and anomalous conditions (unexpected reboots, watchdog
resets, storage errors). The team should evaluate where logs are
stored (on-device flash, RAM, external syslog), whether logs survive
reboot, and whether an attacker who has compromised the device can
tamper with or delete log entries.

### Log Forwarding and External Monitoring

The team should evaluate whether the device can forward security
events to a central monitoring system (syslog, MQTT telemetry channel,
cloud API endpoint, SIEM integration). Is the log forwarding channel
itself encrypted and authenticated? Can an attacker suppress log
forwarding to hide their activity?

### Forensic Readiness

After a suspected compromise, the team should evaluate what evidence
would be available for forensic analysis: Can firmware integrity be
verified against a known-good image? Are logs preserved with
sufficient detail to reconstruct the attacker's actions? Is there a
hardware-backed secure audit trail that cannot be modified by software
running on the device? ETSI EN 303 645 Provision 10 requires that
telemetry data be examined for security anomalies; the IoTA team
should verify that sufficient telemetry is generated and accessible
to satisfy this requirement.
