[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Web Application Specific**

Many IoT devices expose web-based management interfaces, either hosted
locally on the device itself or provided through a cloud-based service
operated by the vendor. These interfaces frequently serve as the primary
means by which end users configure, monitor, and control IoT devices.
Both local and cloud-hosted web applications present attack surfaces
that the IoTA team should evaluate thoroughly. The same categories of
vulnerabilities found in traditional web applications apply here, often
compounded by the resource constraints and less rigorous development
practices common in embedded and IoT product development.

The IoTA team should evaluate web applications associated with IoT
devices in two contexts: the local device web interface (if present)
and any cloud or vendor-hosted management portals. Both should be
tested with the same rigor, as compromise of either can lead to full
device takeover, data exfiltration, or lateral movement into other
parts of the IoT ecosystem.

### Injection Vulnerabilities

#### ***Cross-Site Scripting (XSS)***

Cross-Site Scripting vulnerabilities occur when an application includes
untrusted data in web pages without proper validation or encoding. In
the context of IoT device management interfaces, XSS can be
particularly dangerous because administrative sessions often carry
elevated privileges, and device web interfaces may lack the security
headers and content security policies common in more mature web
applications. The IoTA team should test for reflected, stored, and
DOM-based XSS across all input fields, URL parameters, and any
location where user-supplied data is rendered back to the browser.
Stored XSS in device configuration fields (such as SSID names, device
hostnames, or user profile fields) is of special concern because it
can persist across sessions and affect multiple users of a shared
management portal.

#### ***SQL Injection (SQLi)***

SQL injection vulnerabilities allow an attacker to manipulate database
queries by injecting malicious SQL statements through application input
fields. While many IoT devices use lightweight databases such as SQLite
rather than full relational database management systems, the risk of
SQLi remains significant. Successful exploitation can lead to
unauthorized data access, authentication bypass, or in some cases
command execution on the underlying operating system. The IoTA team
should test all input points that interact with backend data stores,
including login forms, search functionality, device filtering, and
reporting features.

#### ***XML External Entity (XXE) Injection***

IoT devices and their associated services frequently process XML data
for configuration imports, firmware metadata parsing, SOAP-based API
calls, and inter-device communication. If the XML parser is configured
to resolve external entities, an attacker may be able to read arbitrary
files from the server, perform server-side request forgery (SSRF), or
cause denial of service through entity expansion attacks (sometimes
referred to as "billion laughs" attacks). The IoTA team should identify
all points where XML data is accepted and test for external entity
resolution, parameter entity injection, and out-of-band data
exfiltration through external DTD references.

#### ***Command Injection***

Embedded web applications on IoT devices frequently execute system
commands to perform network diagnostics, configuration changes, or
firmware operations. If user input is passed to these system commands
without proper sanitization, an attacker can inject additional commands
to execute arbitrary code on the device. This vulnerability class is
especially prevalent in IoT devices because embedded web applications
often invoke shell commands directly rather than using dedicated
libraries or APIs. The IoTA team should test all input fields that may
interact with underlying system commands, including network diagnostic
tools (ping, traceroute, DNS lookup), configuration utilities, and file
management functions.

### Session and Authentication Attacks

The IoTA team should evaluate the session management and authentication
mechanisms of IoT web interfaces for common weaknesses. Areas of concern
include the use of predictable or sequential session tokens, lack of
session expiration or timeout, failure to invalidate sessions on logout,
transmission of session identifiers over unencrypted channels, and
susceptibility to session fixation attacks. Many IoT web interfaces
implement custom authentication schemes rather than using established
frameworks, increasing the likelihood of implementation flaws.

The team should also evaluate whether the application enforces account
lockout policies after repeated failed authentication attempts, whether
multi-factor authentication is supported or available, and whether
password reset mechanisms can be abused to enumerate valid accounts or
bypass authentication entirely.

### Insecure Direct Object References (IDOR)

Insecure Direct Object Reference vulnerabilities arise when an
application exposes internal implementation objects, such as database
keys, file paths, or device identifiers, in a way that allows an
attacker to manipulate them to access unauthorized resources. In
multi-tenant IoT cloud platforms, IDOR vulnerabilities can allow one
user to access or modify another user's devices, configurations, or
data simply by changing a predictable identifier in a request. The
IoTA team should test all API endpoints and web application functions
that reference device identifiers, user accounts, configuration files,
or firmware images for unauthorized access through parameter
manipulation.

### Cross-Site Request Forgery (CSRF)

Cross-Site Request Forgery vulnerabilities allow an attacker to trick
an authenticated user into performing unintended actions on a web
application. For IoT device management interfaces, CSRF can be
leveraged to change device configurations, modify network settings,
update firmware, create new administrative accounts, or disable
security features, all without the user's knowledge. The IoTA team
should evaluate whether the application implements anti-CSRF tokens,
validates the origin and referrer headers of incoming requests, and
requires re-authentication for sensitive operations such as password
changes or firmware updates.

### Server-Side Request Forgery (SSRF)

Server-Side Request Forgery vulnerabilities allow an attacker to induce
the server-side application to make HTTP requests to an arbitrary domain
or internal resource. In the context of IoT cloud platforms, SSRF can
be leveraged to access internal services, cloud metadata endpoints
(such as AWS EC2 instance metadata at 169.254.169.254), or other
devices on the internal network. The IoTA team should test any
functionality that accepts URLs as input, including webhook
configurations, firmware update URLs, remote syslog server settings,
and integration endpoints.

### Information Disclosure

IoT web applications should be evaluated for information disclosure
vulnerabilities, including verbose error messages that reveal internal
paths, software versions, or stack traces; directory listing enabled on
the web server; exposure of configuration files, backup files, or
source code through predictable URLs; and inclusion of sensitive data
such as API keys, credentials, or internal IP addresses in client-side
JavaScript, HTML comments, or HTTP response headers. The IoTA team
should also evaluate whether the application properly restricts access
to administrative or debugging endpoints that may have been left
enabled from development.

### Transport Layer Security

The IoTA team should evaluate the TLS configuration of all web
interfaces, including supported protocol versions, cipher suites,
certificate validity, and implementation of HTTP Strict Transport
Security (HSTS). Many IoT device web interfaces use self-signed
certificates, outdated TLS versions, or weak cipher suites, any of
which can allow an attacker to intercept or manipulate communications
between the user and the device. The team should also evaluate whether
the application properly redirects HTTP requests to HTTPS and whether
sensitive data such as credentials or session tokens are ever
transmitted over unencrypted connections.

### WebSocket and Real-Time Communication Security

Many modern IoT management interfaces use WebSocket connections for
real-time device status updates and control. The IoTA team should test
for WebSocket hijacking, lack of origin validation on WebSocket
upgrade requests, missing authentication on WebSocket endpoints, and
whether sensitive commands can be injected through manipulated
WebSocket messages.

### Client-Side Storage Security

The IoTA team should evaluate whether IoT device management web
applications store sensitive data in browser-side storage mechanisms
such as localStorage, sessionStorage, or IndexedDB. Sensitive data
stored client-side, including API tokens, device credentials,
configuration data, and session identifiers, may be accessible to
cross-site scripting attacks, browser extensions, or other applications
running in the same browser context. The team should evaluate whether
sensitive data stored client-side is encrypted, whether it is properly
cleared on logout, and whether the application relies on client-side
storage for security-critical decisions that should be validated
server-side.

### OAuth and OpenID Connect Implementation

Many IoT cloud platforms use OAuth 2.0 or OpenID Connect (OIDC) for
user authentication and authorization. The IoTA team should evaluate
these implementations for: authorization code interception (especially
in mobile app redirect flows), token leakage through referrer headers
or browser history, PKCE (Proof Key for Code Exchange) bypass or
absence, improper scope validation (where a token granted for limited
access can be used for elevated operations), token lifetime and
refresh policies, and whether the implementation properly validates
redirect URIs to prevent open redirect attacks.
