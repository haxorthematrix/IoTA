[← Back to IoTA Index](../README.md)

# Principles of IoT Vulnerability Assessment

To maximize the effectiveness and applicability of the security
evaluation of IoT-related technology, the attack team should follow a
series of testing principles supporting consistent technology analysis.
Through these techniques, a team of analysts can produce a cohesive
assessment of one or more target devices or IoT deployments. The
resulting documentation will provide necessary information to remediate
the threats and vulnerabilities that would otherwise hinder the
successful marketing and sales of IoT-based technology.

## Practical and Pertinent Vulnerability Analysis 

As a critical component of each security evaluation exercise, the attack
team should work with vendors, the IoT security team, utilities and
applicable third-parties to identify an appropriate scope for the
analysis. The scope must take into account the resources of an adversary
that may attempt to compromise the security and integrity of IoT-related
technology. The resources accessible to a potential adversary will
significantly influence the threats that require defense and mitigation
strategies. For example, an attacker who has less than US\$10,000 in
accessible resources for the purpose of exploiting IoT technology
represents a different threat than a well-funded adversary whose goals
exceed that of common mischief or exposure of private data. The scope
for the analysis should address the highest level of attacker funding
feasible, while the team must keep in mind all of the different levels
of attack. For instance, the scope may include the resources available
to a nation-state, but the attack team should be cautious not to ignore
the attack vectors likely to be targeted by self-funded individuals.

Based on the scope, the attack team should apply scientific analysis
methods to enumerate and evaluate potential threats, collecting
supporting data through observation and experimentation, then design
further analysis test cases to evaluate through experimentation. Threats
that are deemed irrelevant or impractical based on the identified
adversary resources can be disregarded, focusing on the practical and
pertinent threats immediately affecting customers, utilities and
vendors. Noting these discarded threats, their likelihood and impact of
exploitation should be considered additional value to a final report.
Vendors and utilities must understand the contextual constraints the
analysis project scope places on any engagement and subsequent report.

Through this method, the attack team will focus on the areas of direct
value for affected parties, providing the greatest possible benefit in
the analysis and findings reporting.

## Testing Team Expertise

In order to be effective in the analysis of IoT technology, the attack
team will require expertise in multiple areas of information security,
mobile device, web application and API testing, electrical and radio
engineering and protocol analysis. Due to the varied expertise
requirements, it is anticipated that the attack team will be composed of
several individuals with diverse backgrounds, collaborating on the
analysis of IoT technology.

Fields of expertise required for effective analysis include:

-   Basic and advanced understanding of electronics and principles of
    electricity.

-   Safety training and experience working with high-voltage
    electronics.

-   Knowledge of the assembly language for processors used by IoT
    devices associated hardware. This may require, depending on the
    platform(s) involved, knowledge of multiple variants including x86,
    x86_64, ARM, Arduino, et al.

-   Mobile device operating system structure and analysis techniques for
    evaluating applications

-   RF modulation and coding analysis experience.

-   Experience analyzing standards-based protocols including infrared,
    IEEE 802.15.4/ZigBee, IEEE 802.11/Wi-Fi, BLE, LoRa/LoRaWAN,
    Matter/Thread, cellular IoT (NB-IoT, LTE-M, 5G), and proprietary
    protocols.

-   Knowledge of wired and wireless network design and protocols, and
    detailed understanding of OSI model layers and their interactions.

-   Experience developing embedded technology including embedded
    software development programming and hardware engineering.

-   Ability to creatively evaluate technology for the goal of
    subversion.

-   Software vulnerability identification and analysis skills. Forensic
    capabilities may add value here.

-   Experience writing software-based exploits.

-   Analysis skills for evaluating cryptographic algorithms and
    associated functions.

-   Modern internet protocols and standards such as OAuth and HTTP/HTTPS

-   Web based technologies and languages, and various methods for
    accessing data through these technologies, (Java, RESTful
    interfaces, etc.)

-   AI/ML security: Understanding of adversarial attacks against ML
    models deployed on edge devices, model extraction, data poisoning,
    and inference attacks.

-   Cloud-native and container security: Many IoT backends now run on
    Kubernetes, serverless functions, and containerized microservices.
    Experience with AWS IoT Core, Azure IoT Hub, Google Cloud IoT, and
    similar platforms is increasingly necessary.

-   Automotive protocols: CAN, CAN-FD, Automotive Ethernet
    (100BASE-T1/1000BASE-T1), UDS, DoIP, SOME/IP, and AUTOSAR-based
    systems.

-   Matter/Thread protocol stack analysis and commissioning security,
    including PASE/CASE cryptographic evaluation and Device Attestation
    Certificate (DAC) analysis.

-   SBOM (Software Bill of Materials) analysis and software composition
    analysis (SCA) tooling. Familiarity with CycloneDX and SPDX formats,
    binary-level composition analysis platforms, and vulnerability
    database cross-referencing workflows.

-   Regulatory framework literacy: Ability to map findings to EU CRA
    essential requirements, ETSI EN 303 645 provisions, NIST CSF 2.0
    functions, and sector-specific standards (IEC 62443 for industrial,
    IEC 81001-5-1 for medical, ISO/SAE 21434 for automotive).

## Reproducible Findings

The attack team recognizes that any findings identifying security
threats or vulnerabilities in IoT-related technology will be subject to
significant scrutiny and analysis. Throughout the project, all
security-related findings will be documented such that other analysts
will have sufficient information to reproduce the findings for an
independent findings evaluation, where desired. Methodologies and
results should both be carefully documented to make results repeatable
by other researchers, the associated vendor(s), and IoT companies with
access to similar resources to the testing team.

## Risk Evaluation

The identification of risks to customers, utilities and vendors of
IoT-related technology will be one of the deliverables for the attack
team during the IoTA project. Where vendors and utilities have a finite
amount of resources to apply to the mitigation of risks, it is necessary
to provide an overall prioritization of identified threats such that
threats of the greatest urgency are resolved before threats with little
to no overall risk.

To support the prioritization and evaluation of risk, the attack team
should evaluate all threats and vulnerabilities with supporting
documentation as follows:

-   Risk Description: Information describing the risk, citing references
    where applicable.

-   Risk Probability: The probability of the risk happening to a vendor
    based on the understanding of potential adversaries that require
    defensive measures. This probability should be recorded on a scale
    of 1 to 10 with 1 being least probable and 10 being the most
    probable.

-   Risk Impact: The potential impact of the risk to the utility or
    vendor should the threat be realized. The impact should be recorded
    on a scale of 1 to 10 with 1 being least impact and 10 being the
    greatest impact.

-   Risk Data Quality Evaluation: Risk evaluation requires unbiased and
    accurate data for credibility. The risk documentation should include
    the following data points to describe the quality of the risk data:

    -   Risk Understanding: How well is the risk understood?

    -   Data Availability: How complete is the data pertaining to the
        risk?

    -   Data Quality: Is the data available relevant to the risk being
        described? Is the data current?

    -   Data Reliability: How objective is the data that has been
        supplied to describe the threat? The reliability of data will
        increase with multiple data points from different sources coming
        to similar conclusions, or decrease if the data is highly biased
        or widely disparate across multiple sources.

-   Risk Urgency Assessment: What is the urgency of the risk?

The completed risk data will be used to populate a quantitative risk
evaluation model, providing the necessary information to the utility or
vendor for prioritizing threat remediation, transfer or acceptance.

In addition to the probability/impact scoring above, the IoTA team
should align findings with industry-standard frameworks to improve
compatibility with vulnerability management workflows. CVSS 4.0 scoring
should be applied to all findings to provide a standardized severity
metric. The Stakeholder-Specific Vulnerability Categorization (SSVC)
framework should be used where appropriate to provide decision-oriented
prioritization that accounts for the exploitation status, automatable
nature, and mission impact of a vulnerability, rather than relying on
CVSS base scores alone.

Two additional risk dimensions are particularly relevant for IoT
assessments:

Supply chain risk: A vulnerability in a widely deployed SDK component
or open-source library has fundamentally different blast radius
characteristics than a vulnerability in custom firmware code. A single
vulnerable version of a TLS library or RTOS kernel may affect millions
of devices across multiple vendors. Supply chain vulnerabilities should
be evaluated with consideration of the component's prevalence across
the broader IoT ecosystem, not only the single device under test.

Regulatory risk: A finding that would trigger EU Cyber Resilience Act
reporting obligations, block market access under CE RED, or violate
ETSI EN 303 645 provisions carries urgency beyond its purely technical
impact. Findings that implicate regulatory non-compliance should be
flagged with appropriate regulatory references so that the vendor can
prioritize remediation accordingly.

## Opportunities for Defensive Measures

While the focus of the attack team is to identify vulnerabilities
threatening IoT technology, such in-depth analysis will also reveal
defensive measures for mitigating the impact or probability of threats.
While not a definitive measure of the only strategy for defending
against identified vulnerabilities, the attack team should identify
strategies for the mitigation of vulnerabilities that may be adopted by
the utility or the vendor.
