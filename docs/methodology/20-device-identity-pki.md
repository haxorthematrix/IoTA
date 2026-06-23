[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Device Identity, PKI, and Certificate Lifecycle**

Device identity is the foundation of modern IoT security architecture.
Every access control decision, every firmware update authorization,
and every cloud API call ultimately depends on the device being able
to prove that it is what it claims to be. The IoTA team should
evaluate the entire identity lifecycle from provisioning through
operation to revocation.

### Provisioning and Enrollment

The IoTA team should evaluate how device identity is established
during manufacturing and first use. Key questions include: Are
credentials (certificates, keys, tokens) provisioned during
manufacturing, during first-time setup, or through a just-in-time
enrollment process? Is the provisioning process itself secure (is the
manufacturing environment trusted, or could credentials be intercepted
or cloned during provisioning)? Are credentials unique per device, or
are they shared across a product line, a manufacturing batch, or an
entire fleet? The Meshtastic CVE-2025-52464 (duplicated private keys
across vendor-cloned devices) is a real-world example of provisioning
failure. The team should evaluate the entropy of generated key
material on the target platform, as constrained devices with limited
entropy sources may produce weak or predictable keys.

### Certificate Lifecycle

For devices using X.509 certificates (TLS mutual authentication,
Matter DAC, cloud IoT platform certificates), the IoTA team should
evaluate the full certificate lifecycle: What is the certificate
validity period? Can certificates be renewed over the air? What
happens when a certificate expires and the device cannot reach the
renewal service (does it fail open, fail closed, or enter a degraded
mode)? Is there an automated certificate management protocol (such as
EST, CMP, or ACME) or are certificates manually managed? Is the root
CA certificate updatable, or is it hardcoded (this is a crypto-agility
concern for post-quantum migration)?

### Credential Rotation and Revocation

The team should evaluate whether symmetric keys, API tokens, and
session credentials are rotated on a defined schedule. What happens to
active sessions during rotation? Can an administrator force rotation
across the fleet? For revocation, the team should evaluate whether a
compromised device's identity can be revoked from the cloud backend
and how quickly revocation propagates to the rest of the ecosystem.
Is there a Certificate Revocation List (CRL) or OCSP responder, and
does the device check it? What happens if the revocation infrastructure
is unreachable?

### Identity Spoofing and Cloning

The IoTA team should test whether an attacker with physical access to
one device can extract credentials sufficient to impersonate it or
create clones that are accepted as legitimate by the cloud backend and
other devices. This directly connects to the firmware extraction,
secure element analysis, and fault injection work described elsewhere
in this methodology. The team should evaluate the blast radius of a
single device identity compromise: does it provide access only to that
device's data, or does it grant access to fleet-wide resources?
