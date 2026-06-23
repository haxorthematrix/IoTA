[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Denial of Service and Availability Testing**

For many IoT deployments, availability is the critical security
property. A smart lock that can be crashed remotely, a medical monitor
that can be taken offline, or an industrial controller that can be
forced into a fault state represent threats with immediate real-world
consequences. The IoTA team should evaluate the device's resilience to
denial of service attacks across all layers.

### Resource Exhaustion

Constrained IoT devices with limited memory (often 64KB-512KB RAM),
limited flash storage, and limited processing power are vulnerable to
resource exhaustion attacks. The IoTA team should test: memory
exhaustion through rapid connection establishment, large payload
delivery, or memory leak exploitation; CPU exhaustion through
computationally expensive operations (cryptographic operations,
complex query processing, deeply nested data structure parsing);
flash storage filling through excessive log generation, telemetry
accumulation, or configuration write attacks; and connection table
overflow through rapid establishment of connections that are never
properly closed.

### Network and Application Layer DoS

The IoTA team should test the device's resilience to network-layer
attacks including SYN floods against TCP services, UDP amplification,
MQTT message flooding (high-rate publish to subscribed topics), BLE
advertisement flooding (filling the device's advertisement scan
buffer), Zigbee network key rotation storms, and CAN bus flooding
(high-rate message transmission that forces the target ECU into
bus-off state). Application-layer DoS testing should include deeply
nested JSON or XML payloads, oversized firmware update metadata,
GraphQL query depth bombs, and algorithmic complexity attacks against
any search, sort, or parsing functionality.

### RF Layer Denial of Service

The IoTA team should evaluate the device's behavior when its primary
wireless communication channel is jammed. Using appropriate SDR
hardware, the team should generate targeted interference on the
device's operating frequency and evaluate: Does the device detect
the jamming condition? Does it fail safely (maintaining its current
state) or unsafely (defaulting to an unlocked, open, or uncontrolled
state)? Does it fall back to a secondary communication channel, and
if so, is that fallback channel less secure? Does it alert the user
or cloud backend? How quickly does it recover when jamming stops?

### Recovery and Resilience

After any DoS condition (whether induced through resource exhaustion,
network flooding, or RF jamming), the IoTA team should evaluate the
device's recovery behavior: Does it recover automatically without
manual intervention? Does it require a power cycle or factory reset?
Does it lose configuration, credential state, or accumulated data
during recovery? How long does recovery take? Is the device vulnerable
to repeated DoS attacks that prevent it from completing recovery?

### Botnet Recruitment Assessment

The IoTA team should evaluate whether a compromised device could be
co-opted into a DDoS botnet (the Aisuru/TurboMirai botnet achieved
20+ Tbps DDoS capability using IoT devices). Evaluation should cover:
Does the device have sufficient network bandwidth and processing
capacity to generate meaningful DDoS traffic? Are there access
controls or resource limitations that would prevent an attacker from
installing persistent malware? Can the device's outbound traffic be
monitored or restricted by network-level controls?
