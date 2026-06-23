[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Physical Security and Side-Channel Attacks**

Modern IoT security increasingly relies on hardware security features
such as secure boot, secure elements, and trusted execution
environments. Evaluating these protections requires correspondingly
sophisticated physical attack techniques.

### Electromagnetic Side-Channel Analysis

The IoTA team should use near-field electromagnetic probes to capture
EM emissions from cryptographic operations on target devices. Captured
traces should be analyzed for key leakage through Simple Power Analysis
(SPA), Differential Power Analysis (DPA), and Correlation Power
Analysis (CPA). Tools such as the ChipWhisperer Husky provide
integrated hardware and software for side-channel capture and analysis.

### Fault Injection

Voltage glitching, clock glitching, electromagnetic fault injection
(EMFI using tools like PicoEMP), and laser fault injection (for
high-value targets) should be employed to test the robustness of
secure boot implementations, code read-out protection, and
authentication routines. Fault injection can bypass security features
that are otherwise resistant to software-based attacks.

### Timing Side-Channel Attacks

Cryptographic implementations should be evaluated for timing leakage
that could reveal key material or bypass authentication. Variable-time
comparison functions, branch-dependent timing variations, and
cache-timing attacks on devices with shared caches should be tested
where applicable.
