[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Cross-Layer Attack Chain Methodology**

The IoTA methodology evaluates each protocol and layer independently.
In practice, the most impactful attacks chain vulnerabilities across
multiple layers. The IoTA team should explicitly identify and test
cross-layer attack paths.

### Attack Chain Identification

During threat modeling, the IoTA team should identify attack chains
that span multiple layers of the IoT ecosystem. Common patterns
include: BLE commissioning compromise leading to cloud account
takeover (extract device credentials during BLE setup, use them to
access the cloud backend, pivot to other devices on the same account);
firmware update interception leading to persistent backdoor
installation (MITM the update channel, deliver modified firmware,
use the backdoor for ongoing access); web interface exploitation
leading to physical harm (exploit a web vulnerability to change
physical device parameters such as thermostat setpoints, motor speed
limits, or valve positions); and supply chain compromise leading to
fleet-wide exploitation (a vulnerability in a shared SDK component
exploitable across all devices using that component).

### End-to-End Scenario Testing

The IoTA team should develop and execute end-to-end test scenarios
that exercise identified attack chains. Each scenario should begin
with an initial access technique (physical access, network proximity,
internet-facing attack surface) and proceed through exploitation,
lateral movement, and objective achievement. The results demonstrate
the real-world impact of individual vulnerabilities when combined,
which is often significantly greater than the sum of their individual
risk ratings.
