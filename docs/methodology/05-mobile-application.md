[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **Mobile Application Specific**

Many IoT devices rely on companion mobile applications for initial
device setup, ongoing configuration, data visualization, and remote
control. These applications, developed for iOS and Android platforms,
represent a significant attack surface within the IoT ecosystem. A
vulnerability in a companion application can provide an attacker with
access to user credentials, device control capabilities, or sensitive
data collected by the IoT device. The IoTA team should evaluate
companion mobile applications with the same rigor applied to the
device firmware and web interfaces.

### Static Analysis of Application Binaries

#### ***Hardcoded Secrets in Application Source***

The IoTA team should decompile or disassemble the mobile application
binary to search for hardcoded secrets embedded in the application
code, resources, or configuration files. Secrets of interest include
API keys, authentication tokens, encryption keys, backend server URLs,
database credentials, and vendor-specific identifiers. Android
applications can be decompiled using tools such as jadx, apktool, or
dex2jar, while iOS applications can be analyzed using tools such as
class-dump, Hopper, or Ghidra. Many modern IoT companion apps are
built with cross-platform frameworks such as Flutter (requiring Dart
AOT compilation analysis) or React Native (requiring JavaScript bundle
analysis), which demand different decompilation approaches than native
Android or iOS development. Hardcoded secrets in mobile applications
are particularly concerning because application binaries are
distributed to end users and can be trivially extracted and analyzed
by an adversary.

#### ***Reversible Protocols and Encryption***

Analysis of the decompiled application source can reveal the
communication protocols used between the mobile application and the
IoT device or cloud backend. The IoTA team should identify whether
proprietary protocols are in use, whether standard protocols are
implemented correctly, and whether encryption is applied to data in
transit. Custom or proprietary protocol implementations discovered in
the application source should be evaluated for weaknesses including
lack of authentication, missing integrity checks, susceptibility to
replay attacks, and use of weak or hardcoded encryption keys.

#### ***Reversible Native Libraries***

Many mobile applications include native libraries (compiled C/C++ code)
for performance-critical operations such as cryptographic functions,
protocol handling, or media processing. These libraries can be
extracted from the application package and analyzed using tools such as
Ghidra, IDA Pro, or Binary Ninja. The IoTA team should evaluate native
libraries for the same classes of vulnerabilities found in firmware
binaries, including hardcoded credentials, buffer overflows, format
string vulnerabilities, and improper cryptographic implementations.

#### ***Vulnerable Third-Party Libraries***

Mobile applications commonly include third-party libraries and SDKs
for analytics, crash reporting, advertising, networking, and other
functions. The IoTA team should inventory all third-party dependencies
included in the application and evaluate them against known
vulnerability databases such as the National Vulnerability Database
(NVD) and vendor-specific security advisories. Outdated or vulnerable
third-party libraries can introduce exploitable weaknesses into an
otherwise secure application.

### Runtime Analysis

#### ***Certificate Pinning Bypass***

Certificate pinning is a security mechanism that restricts which
certificates an application will accept when establishing TLS
connections, preventing man-in-the-middle attacks even when an
attacker has installed a trusted root certificate on the device. The
IoTA team should evaluate whether the application implements
certificate pinning and, if so, attempt to bypass it using tools such
as Frida, Objection, or SSL Kill Switch. Successful bypass of
certificate pinning allows the team to intercept and analyze all
network traffic between the application and backend services using
an intercepting proxy such as Burp Suite or mitmproxy.

#### ***Root and Jailbreak Detection***

Some mobile applications implement checks to detect whether the device
has been rooted (Android) or jailbroken (iOS), refusing to operate or
limiting functionality on modified devices. While these checks can
provide a layer of defense against tampering, they are often
implemented poorly and can be bypassed through hooking frameworks such
as Frida or Xposed. The IoTA team should evaluate whether root/jailbreak
detection is implemented, the robustness of the detection mechanism,
and whether bypassing it exposes additional attack surface or reveals
behaviors the application hides on unmodified devices.

#### ***Insecure Data Storage***

The IoTA team should evaluate how the mobile application stores data
on the device, including credentials, authentication tokens, device
configuration data, user personal information, and cached
communications. Data stored in plaintext in shared preferences
(Android), NSUserDefaults (iOS), SQLite databases, or application log
files is accessible to other applications on a rooted or jailbroken
device and may persist even after the application is uninstalled.
The team should evaluate whether sensitive data is encrypted at rest
using platform-provided secure storage mechanisms such as the Android
Keystore or iOS Keychain.

#### ***Inter-Process Communication***

Mobile applications may expose functionality through inter-process
communication (IPC) mechanisms such as Android Intents, Content
Providers, Broadcast Receivers, or iOS URL Schemes and Universal Links.
Improperly secured IPC endpoints can allow other applications on the
device to invoke privileged functionality, access sensitive data, or
manipulate the IoT device through the companion application. The IoTA
team should enumerate all exported IPC endpoints and evaluate their
access controls and input validation.

#### ***Cloud API Key and Credential Embedding***

The IoTA team should evaluate whether the mobile application embeds
cloud API keys, Firebase configurations, AWS access keys, or other
backend credentials that could provide unauthorized access to the
vendor's cloud infrastructure. Firebase configuration files
(google-services.json for Android, GoogleService-Info.plist for iOS)
often contain project identifiers, API keys, and database URLs that
can be used to enumerate or access backend resources. The team should
test whether embedded credentials provide access beyond the intended
scope of the mobile application.

#### ***Push Notification Security***

The IoTA team should evaluate whether push notification channels
(Firebase Cloud Messaging for Android, Apple Push Notification service
for iOS) can be manipulated to deliver false alerts, trigger
unauthorized device actions, or exfiltrate data. The team should test
whether the application properly validates the source and integrity of
push notification payloads, whether notification content includes
sensitive data that could be exposed on the device lock screen, and
whether the push notification registration token can be hijacked to
redirect notifications to an attacker-controlled device.

### Bluetooth and Local Communication

Many IoT companion applications communicate directly with the device
over Bluetooth Low Energy (BLE), Wi-Fi Direct, or other local
communication channels during setup and ongoing operation. The IoTA
team should evaluate the security of these local communication
channels, including whether pairing is authenticated, whether data
exchanged over these channels is encrypted, and whether the application
validates the identity of the device it is communicating with.
Weaknesses in local communication can allow an attacker in physical
proximity to intercept sensitive data, inject commands, or
impersonate a legitimate device.
