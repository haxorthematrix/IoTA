[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Default Credentials and Authentication Hygiene**

Weak, guessable, or hardcoded passwords are ranked as the number one
IoT security risk by the OWASP IoT Top 10, and approximately 20% of
IoT devices still ship with default credentials. The IoTA team should
systematically evaluate credential security across all device
interfaces.

### Default Credential Enumeration

The IoTA team should test all exposed services (SSH, Telnet, HTTP
management, MQTT broker, FTP, TFTP, SNMP, UPnP, debug consoles,
serial console) against known default credential databases. Sources
for default credentials include vendor documentation, datasheets,
support forums, default credential databases (such as
cirt.net/passwords, default-password.info), and credentials discovered
during firmware analysis. The team should also test common weak
credentials (admin/admin, root/root, admin/password, and device-
specific patterns such as serial number-derived passwords).

### Credential Uniqueness Assessment

The IoTA team should determine whether credentials are unique per
device or shared across the product line. If the team has access to
multiple units of the same device, they should compare default
credentials across units. Shared credentials across a product line
represent a fleet-wide vulnerability: compromise of one device's
credentials immediately compromises every device of that model. ETSI
EN 303 645 Provision 1 explicitly prohibits universal default
passwords.

### Change Enforcement and Password Policy

The team should evaluate whether the device forces the user to change
default credentials during initial setup. A device that allows
continued operation with default credentials fails ETSI EN 303 645
Provision 1. The team should also evaluate password complexity
requirements, whether the device prevents the use of common or
trivially guessable passwords, and whether credentials are transmitted
securely during the change process.

### Factory Reset and Credential Recovery

The IoTA team should evaluate the security of the factory reset
mechanism itself. Does factory reset actually erase all user
credentials, configuration data, network keys, and cached data? The
team should perform a factory reset and then dump the device's flash
storage to check for residual data that survived the reset. The team
should also evaluate whether the factory reset mechanism can be
triggered by an attacker (physically or remotely) as a denial of
service attack or as a means to restore known default credentials.
