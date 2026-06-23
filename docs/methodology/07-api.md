[← Methodology Index](README.md) · [← IoTA Index](../../README.md)

## **API Specific**

IoT ecosystems commonly rely on application programming interfaces
(APIs) to facilitate communication between devices, mobile
applications, cloud services, and third-party integrations. These APIs
often serve as the primary mechanism for device control, data
collection, user management, and firmware distribution. The IoTA team
should evaluate APIs associated with IoT devices and their supporting
infrastructure for vulnerabilities that could lead to unauthorized
access, data exposure, or device compromise.

### API Discovery and Documentation

The IoTA team should begin API assessment by identifying all API
endpoints associated with the IoT ecosystem. This includes APIs
documented in vendor-provided materials, APIs discovered through
analysis of mobile application binaries and web interface traffic, and
undocumented or "hidden" APIs identified through traffic analysis,
directory brute-forcing, or examination of JavaScript source code. The
team should evaluate whether API documentation (such as OpenAPI/Swagger
specifications) is publicly accessible without authentication and
whether such documentation reveals information about internal
endpoints, data models, or authentication mechanisms that could aid
an attacker.

### Authentication and Authorization

The IoTA team should evaluate the authentication mechanisms used to
protect API endpoints. Common weaknesses in IoT API authentication
include the use of static API keys that are shared across all devices
or users, lack of authentication on endpoints that perform sensitive
operations, use of basic authentication without TLS, weak or
predictable token generation, and failure to properly expire or rotate
authentication tokens. The team should also evaluate whether the API
enforces proper authorization checks, ensuring that authenticated
users can only access resources and perform actions appropriate to
their privilege level. Broken object-level authorization (BOLA), where
an authenticated user can access another user's devices or data by
manipulating resource identifiers, is a common and high-impact
vulnerability in multi-tenant IoT platforms.

### Input Validation and Injection

API endpoints should be tested for proper input validation across all
parameters, including URL path parameters, query string parameters,
request headers, and request body content. The IoTA team should test
for injection vulnerabilities including SQL injection, NoSQL injection,
command injection, and LDAP injection across all endpoints that accept
user-supplied input. APIs that process structured data formats such as
JSON, XML, or YAML should be tested for deserialization vulnerabilities
and format-specific injection attacks.

### Rate Limiting and Abuse Prevention

The IoTA team should evaluate whether API endpoints implement rate
limiting to prevent brute-force attacks, credential stuffing, denial
of service, and resource exhaustion. APIs that lack rate limiting on
authentication endpoints are particularly concerning, as they may allow
an attacker to enumerate valid credentials or perform offline attacks
against weak password policies. The team should also evaluate whether
the API implements protections against automated abuse, such as
progressive delays or account lockout mechanisms.

### Data Exposure

API responses should be evaluated for excessive data exposure, where
the API returns more information than is necessary for the requested
operation. Common examples in IoT APIs include device listing endpoints
that return detailed hardware information, firmware versions, network
configuration, and geographic location data for all devices associated
with an account; user profile endpoints that expose email addresses,
phone numbers, or physical addresses; and error responses that reveal
internal system details, database schemas, or stack traces. The IoTA
team should evaluate each API response to determine whether the data
returned is appropriate for the requesting user's privilege level and
whether sensitive data is properly redacted or omitted.

### SOAP, REST, GraphQL, and gRPC Considerations

IoT APIs may be implemented using various architectural styles and
protocols. REST APIs, the most common in modern IoT implementations,
should be evaluated for proper use of HTTP methods, status codes, and
resource naming conventions. SOAP-based APIs, sometimes found in
industrial and enterprise IoT systems, should be evaluated for
XML-specific vulnerabilities including XXE injection, WSDL
enumeration, and SOAPAction manipulation.

GraphQL APIs, increasingly adopted in IoT cloud platforms, present
unique security considerations. The IoTA team should evaluate GraphQL
implementations for introspection query exposure (which can reveal
the entire API schema), deeply nested query attacks that can cause
denial of service through resource exhaustion, batching attacks that
can bypass rate limiting, and field-level authorization bypasses
where a user can access restricted data through alternative query
paths. The team should also evaluate whether the GraphQL
implementation properly validates and sanitizes variables and
arguments to prevent injection attacks.

gRPC/Protobuf APIs are used by some modern IoT platforms for
high-performance device communication. The IoTA team should evaluate
gRPC implementations for service enumeration through reflection,
protobuf schema extraction, authentication bypass, and input
validation on protobuf message fields.

### Webhook and FOTA API Security

Many IoT platforms support webhooks or callback URLs for event
notifications. The IoTA team should evaluate whether webhook
configurations are properly authenticated, whether the platform
validates destination URLs to prevent SSRF attacks, and whether
webhook payloads contain sensitive data. FOTA (Firmware Over The Air)
API endpoints responsible for firmware distribution, version
checking, and update delivery should be specifically evaluated for
authentication bypass, unsigned firmware acceptance, version rollback
facilitation, and metadata injection.
