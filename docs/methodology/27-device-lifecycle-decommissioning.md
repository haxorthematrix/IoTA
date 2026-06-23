[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Device Lifecycle and Decommissioning Security**

IoT devices have lifespans measured in years or decades, and they pass
through multiple stages: manufacturing, provisioning, deployment,
operation, maintenance, ownership transfer, and end-of-life disposal.
Security vulnerabilities can emerge at any stage transition.

### Secure Factory Reset

The IoTA team should evaluate whether a factory reset actually erases
all user data, credentials, network keys, Wi-Fi passwords, cloud
account associations, and cached data. The team should perform a
factory reset and then dump the device's storage to check for
residual data. On flash-based storage, data that has been logically
deleted may still be physically present due to flash wear-leveling
algorithms. The team should also evaluate whether the factory reset
mechanism itself is secure: Can it be triggered remotely by an
attacker? Can it be protected by a PIN or password? Does it require
physical access?

### Ownership Transfer

When a device changes ownership (resale, return, redeployment), the
IoTA team should evaluate whether the previous owner's access is
fully revoked. Key questions include: Can the previous owner's cloud
account still see or control the device after transfer? Are the
device's Wi-Fi credentials, Bluetooth pairings, and Matter fabric
memberships cleared during transfer? Is there a formal ownership
transfer process, or must the new owner perform a factory reset and
hope it actually works?

### End-of-Life Security

When a vendor discontinues a product or ceases operations, the IoTA
team should evaluate the implications for deployed devices: What
happens to cloud-dependent functionality when the backend is shut
down? Does the device become nonfunctional, or does it continue to
operate in a degraded (and potentially less secure) mode? Are
firmware updates still available? Is the SBOM still monitored for new
CVEs? Who is responsible for security vulnerabilities discovered after
end-of-life?
