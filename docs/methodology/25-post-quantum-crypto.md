[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Post-Quantum Cryptography Readiness**

IoT devices manufactured today will remain in the field for 5 to 15
years or more. NIST has finalized post-quantum cryptographic standards
(ML-KEM for key encapsulation, ML-DSA for digital signatures, SLH-DSA
for hash-based signatures). Devices with hardcoded cryptographic
algorithms that cannot be updated will become vulnerable within their
operational lifetime. The IoTA team should evaluate PQC readiness as
part of any assessment of devices intended for long-term deployment.

### Crypto-Agility Assessment

The team should evaluate whether the device's cryptographic algorithms
can be updated via firmware update or whether they are hardcoded in
hardware (ROM, ASIC, or hardwired in an FPGA bitstream). Devices
without crypto-agility, where the root CA certificate, signature
verification algorithm, or key exchange algorithm cannot be changed
without hardware replacement, face a ticking clock as quantum
computing matures.

### Resource Feasibility

PQC algorithms require significantly more memory and computational
resources than current algorithms. ML-KEM public keys are
approximately 800-1500 bytes (vs. 32-91 bytes for ECDH), and ML-DSA
signatures are approximately 2400-4600 bytes (vs. 64-132 bytes for
ECDSA). The IoTA team should evaluate whether the target device's
hardware (RAM, flash, CPU) has sufficient resources to support PQC
algorithms, and whether the communication channels have sufficient
bandwidth for the larger key and signature sizes.

### Harvest-Now-Decrypt-Later Risk

For devices transmitting long-lived sensitive data (medical records,
industrial trade secrets, financial transactions, government
communications), the team should evaluate the risk that an adversary
captures encrypted traffic today and decrypts it with a future quantum
computer. This risk is highest for devices using static ECDH or RSA
key exchange (where a single key compromise decrypts all captured
traffic) and lowest for devices using ephemeral key exchange with
perfect forward secrecy.
