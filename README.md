# NEXUS-COMPLY

### AI-Driven Multi-Vendor Network Security Compliance Auditor

>
> Normalize heterogeneous network-device configurations into a canonical
> security model, evaluate explicit security controls deterministically,
> preserve evidence, and use AI only as an assistive layer for
> previously unseen syntax.

------------------------------------------------------------------------

## 1. Overview

NEXUS-COMPLY is a proposed vendor-agnostic network security compliance
and risk platform designed to analyze configurations from multiple
network-device vendors.

The central design principle is:

``` text
Configuration
      ↓
Vendor Detection
      ↓
Vendor-Specific Parser
      ↓
Canonical Security Model
      ↓
Deterministic Compliance Evaluation
      ↓
Findings + Evidence
      ↓
Risk / Drift / What-If
      ↓
Validated Remediation + Reporting
```

The platform is designed to address the problem that different vendors
represent similar security controls using different configuration
syntaxes.

The project documentation defines an adaptive AI workflow for unknown
syntax:

``` text
Unknown Syntax
      ↓
AI Suggestion + Retrieved Context
      ↓
Human Review
      ↓
Approved Mapping
      ↓
Reusable Normalization
      ↓
Deterministic Compliance Evaluation
```

**Important implementation note:** the supplied project material is
primarily architecture, work-package, proposal, and presentation
documentation. The actual application source tree, build manifests,
database migrations, and executable deployment configuration were not
supplied for verification. Therefore, this README distinguishes
documented design from implementation that can be verified from source.

------------------------------------------------------------------------

## 2. Problem Statement

Modern network environments commonly contain devices from different
vendors. Each vendor may use different command structures, configuration
hierarchies, terminology, and syntax.

This creates several problems:

-   Configuration review becomes manual and time-consuming.
-   Equivalent security settings may appear in completely different
    syntaxes.
-   A single compliance engine cannot directly reason over raw
    vendor-specific configuration.
-   Unknown or unsupported syntax can result in incomplete assessments.
-   Simple PASS/FAIL results do not provide sufficient evidence or
    security context.
-   Configuration changes may introduce security-relevant drift.
-   Remediation must be appropriate to the vendor and platform.
-   AI-generated interpretations cannot safely become authoritative
    compliance decisions without validation.

NEXUS-COMPLY addresses this by separating **syntax interpretation**,
**security normalization**, and **deterministic compliance evaluation**.

------------------------------------------------------------------------

## 3. Objectives

The documented objectives are:

1.  Detect the vendor and platform of an uploaded configuration.
2.  Parse vendor-specific configuration syntax.
3.  Convert vendor-specific syntax into a common canonical security
    model.
4.  Preserve source locations and evidence for explainability.
5.  Evaluate canonical security facts against explicit compliance rules.
6.  Support multiple compliance frameworks.
7.  Represent uncertain or unknown syntax safely.
8.  Use AI to assist with previously unseen syntax.
9.  Require human approval before AI mappings become reusable.
10. Maintain findings and evidence for auditability.
11. Support contextual risk analysis.
12. Identify security-relevant configuration drift.
13. Provide what-if analysis for proposed changes.
14. Produce validated vendor/platform-specific remediation guidance.
15. Generate audit/reporting outputs.

------------------------------------------------------------------------

## 4. Key Features

### Core Security Pipeline

-   Vendor detection
-   Vendor-specific parsing
-   Canonical security normalization
-   Applicability evaluation
-   Deterministic compliance evaluation
-   Findings generation
-   Evidence traceability

### Compliance

The documented architecture includes mappings for:

-   CIS
-   NIST
-   DISA STIG
-   ISO/IEC 27001 technical evidence

The exact number of implemented controls and rules is **not specified in
the current implementation**.

### Security Intelligence

The design includes:

-   Severity
-   Contextual risk
-   Configuration drift
-   What-if simulation
-   Validated remediation
-   Audit orchestration
-   Reporting

### AI-Assisted Unknown Syntax

The AI layer is intended to:

-   Interpret unknown configuration syntax.
-   Retrieve similar approved mappings.
-   Generate structured candidate mappings.
-   Provide confidence and rationale.
-   Allow administrator approval, editing, rejection, or retention of
    UNKNOWN.
-   Persist approved mappings for future deterministic reuse.

AI is **not** intended to make the final compliance decision.

------------------------------------------------------------------------

## 5. High-Level Architecture

``` mermaid
flowchart TD
    A[Raw Network Configuration] --> B[Vendor Detection]
    B --> C[Vendor / OS Parser]
    C --> D{Unknown Syntax?}

    D -- No --> E[Canonical Security Model]
    D -- Yes --> F[AI Analysis / Retrieval]
    F --> G[Human Validation]
    G --> E

    E --> H[Applicability Engine]
    H --> I[Deterministic Rule Engine]

    I --> J[PASS]
    I --> K[FAIL]
    I --> L[UNKNOWN]
    I --> M[NOT APPLICABLE]
    I --> N[ERROR]

    J --> O[Findings / Evidence]
    K --> O
    L --> O
    M --> O
    N --> O

    O --> P[Risk]
    O --> Q[Drift]
    O --> R[What-If]
    O --> S[Validated Remediation]
    O --> T[Audit / Report]
```

### Architecture Boundary

The documented architecture intentionally keeps the decision boundary
clear:

``` text
AI
 ↓
Suggest meaning / mapping
 ↓
Human approval
 ↓
Canonical security state
 ↓
Deterministic compliance engine
 ↓
Compliance result
```

AI therefore assists semantic interpretation but does not become the
authority for PASS/FAIL.

------------------------------------------------------------------------

## 6. Technology Stack

The supplied documentation describes the following technology choices.

  -----------------------------------------------------------------------
  Layer                               Documented Technology / Approach
  ----------------------------------- -----------------------------------
  Frontend                            React + TypeScript

  Main backend                        Java + Spring Boot

  Security                            Spring Security / role-based access
                                      control

  Structured persistence              PostgreSQL + Hibernate/JPA in the
                                      main proposal

  AI service                          Python + FastAPI

  AI persistence                      MongoDB + Spring Data MongoDB in
                                      the AI/Package-B documentation

  AI techniques                       Embeddings, similarity retrieval,
                                      optional LLM/RAG

  API documentation                   OpenAPI / Swagger

  Containerization                    Docker

  Version control                     Git / GitHub
  -----------------------------------------------------------------------

### Database Architecture Caveat

The supplied documents contain two database descriptions:

-   The main solution proposal specifies **PostgreSQL + Hibernate/JPA**
    for structured audit, rule, and configuration data.
-   The AI and Package-B work packages specify **MongoDB / Spring Data
    MongoDB** for AI-related data such as `ai_mappings` and `ai_jobs`.

The final runtime database topology cannot be verified without the
actual source repository.

------------------------------------------------------------------------

## 7. Project Structure

The exact repository structure was not supplied.

The following conceptual structure is derived from the documented
architecture and should **not** be interpreted as the verified source
tree:

``` text
NEXUS-COMPLY/
├── frontend/                 # React / TypeScript UI
├── backend/                  # Java / Spring Boot API and orchestration
├── cyber-engine/             # Parsing, normalization and compliance logic
├── ai-service/               # Python/FastAPI AI assistance layer
├── database/                 # Database scripts/migrations if implemented
├── docs/                     # Architecture and project documentation
├── tests/                    # Automated tests if implemented
├── docker/                   # Container/deployment configuration if implemented
└── README.md
```

**Actual source structure: Not specified in the current
implementation.**

------------------------------------------------------------------------

## 8. End-to-End Workflow

``` mermaid
sequenceDiagram
    participant U as Administrator
    participant API as Spring Boot API
    participant D as Vendor Detector
    participant P as Parser
    participant AI as AI Service
    participant H as Human Reviewer
    participant N as Normalizer
    participant C as Compliance Engine
    participant R as Risk / Reporting

    U->>API: Submit configuration
    API->>D: Detect vendor/platform
    D-->>API: Vendor + confidence
    API->>P: Parse configuration
    P-->>API: Parsed facts + unknowns

    alt Unknown syntax exists
        API->>AI: Analyze unknown syntax
        AI-->>API: Candidate mapping
        API->>H: Present suggestion
        H-->>API: Approve / edit / reject / keep UNKNOWN
    end

    API->>N: Normalize configuration
    N-->>API: Canonical security model
    API->>C: Evaluate applicable rules
    C-->>API: PASS / FAIL / UNKNOWN / N/A / ERROR
    API->>R: Evidence, findings, risk and reporting
    R-->>U: Assessment results
```

------------------------------------------------------------------------

## 9. Vendor Support

The documented prototype target contains four representative vendors:

  Vendor      Documented Role
  ----------- ---------------------------------------------------
  Cisco       Router/switch CLI example
  Juniper     Different syntax / set-style configuration
  Fortinet    Firewall / hierarchical configuration example
  Palo Alto   Enterprise firewall / management-security mapping

The architecture is described as vendor-agnostic and extensible.

**Do not interpret the four-vendor target as universal vendor support.**

Actual implemented vendor parsers are **not verifiable from the supplied
source files**.

------------------------------------------------------------------------

## 10. Vendor Detection

The documented detection strategy combines:

-   Device metadata hints
-   Configuration syntax fingerprints
-   Vendor identification
-   Platform/OS family detection
-   Version detection where possible
-   Confidence
-   User correction
-   Detection metadata

A representative documented response is:

``` json
{
  "vendor": "Cisco",
  "platform": "IOS-XE",
  "confidence": 0.97,
  "detectionMethod": "SYNTAX_FINGERPRINT",
  "status": "DETECTED"
}
```

A core safety principle is:

> An incorrect confident classification is worse than an explicit
> UNKNOWN state.

The exact implementation of the detector is not available for
source-level verification.

------------------------------------------------------------------------

## 11. Parser Layer

Each supported vendor is intended to have an isolated parser/adapter.

Conceptually:

``` text
parsers/
├── cisco/
├── juniper/
├── fortinet/
└── paloalto/
```

A parser is responsible for:

-   Tokenizing configuration syntax.
-   Interpreting vendor-specific constructs.
-   Extracting security settings.
-   Handling hierarchy and sections.
-   Flagging unknown constructs.
-   Returning source locations.
-   Producing canonical facts or intermediate representations.

A parser should **not**:

-   Decide overall framework compliance.
-   Hard-code CIS/NIST/STIG/ISO outcomes.
-   Silently discard unknown security data.
-   Generate arbitrary production commands.

------------------------------------------------------------------------

## 12. Canonical Security Model

The canonical model is the central abstraction of NEXUS-COMPLY.

Vendor-specific syntax is converted into vendor-neutral security facts.

A documented example is:

``` json
{
  "deviceId": "DEV-001",
  "security": {
    "ssh": {
      "enabled": true,
      "version": 2
    },
    "telnet": {
      "enabled": false
    },
    "https": {
      "enabled": true
    },
    "snmp": {
      "enabled": true,
      "version": "3"
    }
  },
  "authentication": {
    "aaa": true
  },
  "logging": {
    "syslog": true,
    "localLogging": true
  },
  "ntp": {
    "configured": true
  },
  "managementAccess": {},
  "acl": {},
  "crypto": {},
  "services": {}
}
```

Documented initial security domains include:

-   SSH
-   Telnet
-   HTTPS
-   SNMP
-   AAA
-   Authentication
-   Logging
-   NTP
-   ACLs
-   Management access
-   Services
-   Cryptography

The canonical schema is intended to be versioned so parser/rule
compatibility can be made explicit.

------------------------------------------------------------------------

## 13. Unknown Syntax and AI-Assisted Learning

Unknown syntax is treated as an explicit state rather than being
silently ignored.

### Learning Loop

``` mermaid
flowchart LR
    A[Unknown Configuration Syntax]
    B[Build Context]
    C[Redact Sensitive Data]
    D[Retrieve Approved Mappings]
    E[Generate Candidate Mapping]
    F[Validate Structured Response]
    G[Human Review]
    H[Approved Mapping]
    I[Future Reuse]
    J[Deterministic Compliance]

    A --> B --> C --> D --> E --> F --> G
    G -->|Approve| H --> I --> J
    G -->|Reject / Edit / Keep Unknown| J
```

The documented AI workflow includes:

1.  Parser emits `UNKNOWN_SYNTAX`.
2.  Build context from vendor, OS/version, hierarchy, raw line, nearby
    lines, source location, parser information, and candidate canonical
    fields.
3.  Redact credentials/secrets where possible.
4.  Retrieve similar human-approved mappings.
5.  Generate a candidate meaning when necessary.
6.  Validate the AI response against an allowed structure.
7.  Present the result to an administrator.
8.  Store approved mappings with provenance.
9.  Reuse approved mappings on future audits.
10. Run deterministic compliance evaluation on the resulting canonical
    observation.

**AI confidence is a suggestion-quality signal, not a compliance
score.**

------------------------------------------------------------------------

## 14. Compliance Engine

The compliance engine operates on the canonical security state rather
than directly on vendor syntax.

Conceptual evaluation:

``` text
Canonical Security State
        ↓
Applicability
        ↓
Control
        ↓
Rule
        ↓
Expected vs Actual
        ↓
Result
```

Documented result states are:

  State              Meaning
  ------------------ ------------------------------------------
  `PASS`             Required condition is satisfied
  `FAIL`             Required condition is not satisfied
  `UNKNOWN`          Available information is insufficient
  `NOT_APPLICABLE`   The control does not apply
  `ERROR`            Evaluation could not be safely completed

A documented safety rule is:

> Parser or rule errors must never become PASS.

------------------------------------------------------------------------

## 15. Compliance Frameworks

The project documentation identifies the following framework families:

-   CIS
-   NIST
-   DISA STIG
-   ISO/IEC 27001 technical evidence mappings

The architecture separates framework rules from vendor parsing.

This allows a common canonical security state to be assessed against
multiple frameworks without duplicating vendor-specific parsing logic
inside every framework.

Exact rule counts, control IDs, framework versions, and implementation
status are **not specified in the current implementation**.

------------------------------------------------------------------------

## 16. Findings and Evidence

A finding is intended to connect the compliance result to the control
and the underlying configuration evidence.

The documented finding model includes concepts such as:

-   Audit
-   Control
-   Rule
-   Status
-   Expected value
-   Actual value
-   Severity

The evidence model includes:

-   Source line/fact
-   Expected value
-   Actual value
-   Explanation

Conceptually:

``` text
Finding
  ├── Framework
  ├── Control
  ├── Rule
  ├── Status
  ├── Expected
  ├── Actual
  └── Evidence
        ├── Source location
        ├── Configuration fact
        └── Explanation
```

This is intended to make compliance results explainable and auditable.

------------------------------------------------------------------------

## 17. Risk Analysis

The documentation distinguishes **severity** from **contextual risk**.

Severity describes the importance of a finding.

Contextual risk additionally considers device/environment context such
as:

-   Asset criticality
-   Exposure
-   Security impact
-   Other documented risk factors

The exact risk formula and scoring implementation are **not specified in
the current implementation**.

------------------------------------------------------------------------

## 18. Configuration Drift

NEXUS-COMPLY is designed to identify security-relevant configuration
changes between versions.

The documented drift model includes:

``` text
From Version
      ↓
Semantic / Canonical Comparison
      ↓
Changed Security Facts
      ↓
Security Impact
      ↓
Drift Result
```

The architecture emphasizes **semantic configuration drift**, rather
than relying only on raw text differences.

Actual drift implementation is not verified from source code.

------------------------------------------------------------------------

## 19. What-If Simulation

What-if analysis is intended to answer:

> What happens to compliance and risk if a proposed configuration change
> is applied?

The documented simulation model contains:

-   Base configuration/version
-   Proposed change
-   Before state
-   After state
-   Affected controls
-   Compliance impact
-   Risk impact

A simulation must not mutate the real configuration history.

Actual simulation implementation is not verified from source code.

------------------------------------------------------------------------

## 20. Remediation

The documented remediation layer is intended to produce:

-   Vendor-specific guidance
-   Platform-specific steps
-   Ordered remediation actions
-   Expected validation state
-   Reference to the relevant control/finding
-   Approval/review requirements

The design explicitly avoids autonomous production command execution.

AI-generated commands or remediation must not be automatically executed
against production devices.

Actual remediation templates and execution behavior are not verified
from source code.

------------------------------------------------------------------------

## 21. Data Flow

``` mermaid
flowchart TD
    A[Configuration File] --> B[Configuration Version]
    B --> C[Vendor Detection]
    C --> D[Vendor Parser]
    D --> E[Source Facts]
    E --> F[Canonical Security Model]
    F --> G[Applicable Controls]
    G --> H[Compliance Rules]
    H --> I[Compliance Results]
    I --> J[Findings]
    J --> K[Evidence]
    J --> L[Risk]
    F --> M[Drift]
    F --> N[What-If]
    J --> O[Remediation]
    J --> P[Reports]
```

------------------------------------------------------------------------

## 22. Database Model

The project documents several conceptual data objects.

  -----------------------------------------------------------------------
  Object                  Representative Fields   Purpose
  ----------------------- ----------------------- -----------------------
  Device Context          deviceId, vendor,       Asset context
                          platform, model, OS,    
                          criticality, exposure   

  Raw Configuration       configId, version,      Original evidence
                          hash, source            

  Normalized State        security,               Vendor-neutral facts
                          authentication,         
                          logging, network,       
                          crypto                  

  Rule                    framework, control,     Executable assessment
                          applicability, check,   
                          severity, evidence      

  Finding                 audit, control, rule,   Actionable result
                          status, expected,       
                          actual, severity        

  Evidence                source line/fact,       Traceability
                          expected, actual,       
                          explanation             

  Risk                    severity, criticality,  Prioritization
                          exposure, factors,      
                          score                   

  Drift                   fromVersion, toVersion, Historical security
                          changes, impact         change

  AI Mapping              raw syntax, suggestion, Unknown-syntax learning
                          confidence, approval,   
                          canonical field         

  Remediation             vendor, platform,       Corrective guidance
                          finding type, steps,    
                          validation              

  Simulation              base version, proposed  What-if analysis
                          change, before/after,   
                          affected controls       
  -----------------------------------------------------------------------

The AI work package specifically describes:

-   `ai_mappings`
-   `ai_jobs`

The exact production schema, migrations, indexes, constraints, and
relationships are **not available for verification**.

------------------------------------------------------------------------

## 23. API Documentation

The work-package documentation defines an intended versioned REST API
surface.

### Parser and Normalization

  Method   Endpoint
  -------- -----------------------------------------------------
  POST     `/api/v1/cyber/parse`
  GET      `/api/v1/cyber/parse-jobs/{jobId}`
  GET      `/api/v1/cyber/parse-jobs/{jobId}/errors`
  GET      `/api/v1/cyber/parse-jobs/{jobId}/unknowns`
  POST     `/api/v1/cyber/normalize`
  GET      `/api/v1/normalized-configurations/{id}`
  GET      `/api/v1/configurations/versions/{id}/normalized`
  GET      `/api/v1/normalized-configurations/{id}/source-map`

### Frameworks, Controls and Rules

  Method   Endpoint
  -------- ------------------------------------
  GET      `/api/v1/frameworks`
  GET      `/api/v1/frameworks/{id}`
  GET      `/api/v1/frameworks/{id}/controls`
  GET      `/api/v1/controls/{id}`
  GET      `/api/v1/controls/{id}/rules`
  GET      `/api/v1/rules/{id}`
  GET      `/api/v1/framework-mappings`
  GET      `/api/v1/framework-mappings/{id}`

### Audits

  Method   Endpoint
  -------- -----------------------------------------
  POST     `/api/v1/audits`
  GET      `/api/v1/audits`
  GET      `/api/v1/audits/{id}`
  GET      `/api/v1/audits/{id}/status`
  POST     `/api/v1/audits/{id}/cancel`
  POST     `/api/v1/audits/{id}/rerun`
  GET      `/api/v1/audits/{id}/summary`
  GET      `/api/v1/audits/{id}/framework-results`
  GET      `/api/v1/audits/{id}/findings`
  GET      `/api/v1/audits/{id}/risk`
  POST     `/api/v1/audits/{id}/report`

### Compliance

  Method   Endpoint
  -------- -------------------------------------------------
  POST     `/api/v1/compliance/evaluate`
  POST     `/api/v1/compliance/evaluate/control/{id}`
  POST     `/api/v1/compliance/evaluate/framework/{id}`
  GET      `/api/v1/compliance/results/{auditId}`
  GET      `/api/v1/compliance/results/{auditId}/summary`
  GET      `/api/v1/compliance/results/{auditId}/unknowns`

### Findings and Evidence

  Method   Endpoint
  -------- ---------------------------------------
  GET      `/api/v1/findings`
  GET      `/api/v1/findings/{id}`
  PATCH    `/api/v1/findings/{id}/status`
  PATCH    `/api/v1/findings/{id}/severity`
  GET      `/api/v1/findings/{id}/history`
  GET      `/api/v1/findings/{id}/related`
  POST     `/api/v1/findings/{id}/acknowledge`
  POST     `/api/v1/findings/{id}/resolve`
  GET      `/api/v1/findings/{id}/evidence`
  GET      `/api/v1/evidence/{id}`
  GET      `/api/v1/evidence/{id}/source`
  GET      `/api/v1/evidence/{id}/configuration`

### AI

  Method   Endpoint
  -------- ------------------------------------
  POST     `/api/v1/ai/analyze`
  GET      `/api/v1/ai/jobs/{id}`
  GET      `/api/v1/ai/jobs/{id}/suggestions`
  GET      `/api/v1/ai/mappings`
  GET      `/api/v1/ai/mappings/{id}`
  POST     `/api/v1/ai/mappings/{id}/approve`
  POST     `/api/v1/ai/mappings/{id}/reject`
  POST     `/api/v1/ai/mappings/{id}/test`
  GET      `/api/v1/ai/mappings/{id}/usage`

### API Implementation Status

These endpoints are documented contracts/work-package interfaces. Their
existence in the running application could not be verified because the
actual backend source repository was not supplied.

The Cyber Layer document also contains an earlier non-versioned
`/api/...` endpoint surface. The final runtime API path should be taken
from the actual backend implementation when available.

------------------------------------------------------------------------

## 24. Cybersecurity and Security Controls

The documented security requirements include:

-   Treat uploaded configuration as untrusted input.
-   Prevent path traversal.
-   Restrict upload size and parser resources.
-   Never execute commands contained in uploaded configuration.
-   Never automatically execute AI-generated commands.
-   Sanitize/contextualize configuration fragments sent to AI.
-   Defend against prompt injection originating from configuration text.
-   Keep AI/provider keys out of the React frontend.
-   Keep AI/API credentials on the server side.
-   Validate structured AI responses.
-   Require human approval for reusable AI mappings.
-   Preserve provenance for approved mappings.
-   Fail safely when parsing or AI processing fails.
-   Ensure unauthorized users cannot trigger protected cyber operations.

Authentication implementation details, password hashing algorithms, JWT
configuration, encryption-at-rest, TLS configuration, secret-management
platform, and exact RBAC roles are **not specified in the supplied
implementation source**.

------------------------------------------------------------------------

## 25. Installation

### Current Repository Limitation

A complete installation procedure cannot be stated accurately because
the actual repository build files and deployment configuration were not
supplied for verification.

The documentation identifies a stack involving:

-   React / TypeScript
-   Java / Spring Boot
-   Python / FastAPI
-   PostgreSQL and/or MongoDB depending on component
-   Docker

However, exact dependency versions, environment variables, ports,
commands, database initialization steps, and service startup order are
**not specified in the current implementation**.

### Recommended Repository Documentation

Once the source repository is available, this section should contain
verified commands such as:

``` bash
git clone <repository-url>
cd <repository-directory>
```

followed by the actual project-specific build and startup commands.

Do not copy unverified commands into production documentation.

------------------------------------------------------------------------

## 26. Configuration

The supplied work packages indicate configuration areas such as:

-   Database connection
-   Spring Boot application configuration
-   AI service configuration
-   Embedding model configuration
-   Optional LLM provider configuration
-   API credentials
-   Logging
-   Security configuration

Exact environment-variable names and configuration files are **not
specified in the current implementation**.

Secrets should never be committed to Git.

------------------------------------------------------------------------

## 27. Running the Application

The exact commands for running NEXUS-COMPLY are not verifiable from the
supplied project material.

A documented target architecture is:

``` text
React UI
   ↓
Spring Boot REST API
   ↓
Cyber Engine
   ↓
Python/FastAPI AI Service
   ↓
AI retrieval / embedding / optional LLM
```

The actual service ports, startup commands, Docker Compose
configuration, health checks, and production deployment process remain
**Not specified in the current implementation.**

------------------------------------------------------------------------

## 28. Testing

The Cyber Layer documentation identifies the following testing
expectations:

-   Parser tests
-   Compliance rule tests
-   AI tests
-   Security/adversarial tests
-   End-to-end tests
-   Four-vendor scenarios

The acceptance criteria include:

-   Correct vendor detection or safe uncertainty.
-   Supported syntax becoming canonical security facts.
-   Unknown syntax never silently becoming a secure/valid state.
-   AI mappings requiring human approval.
-   Deterministic and reproducible compliance.
-   Traceable framework/control/rule context.
-   Traceable evidence for important findings.
-   Separation of severity and contextual risk.
-   Security-relevant historical drift.
-   Non-mutating what-if simulation.
-   Vendor/platform-appropriate remediation.
-   Safe failure for malformed/unsupported input.
-   AI failure not breaking deterministic auditing.
-   Authorization around protected cyber operations.
-   No autonomous production command execution.
-   Four-vendor end-to-end testing for demonstrated controls.

Actual test files, coverage, execution results, and benchmark numbers
are **not specified in the current implementation**.

------------------------------------------------------------------------

## 29. Error Handling

The documented error model is:

  Condition                 Required Behavior
  ------------------------- -------------------------------------------
  Unsupported vendor        UNKNOWN/unsupported state + safe message
  Parser error              Audit FAILED/PARTIAL with safe diagnostic
  Unknown syntax            UNKNOWN + AI review path
  Rule execution error      ERROR; never PASS
  Missing evidence          UNKNOWN or NOT_ASSESSABLE
  AI failure                Continue deterministic audit path
  Malformed configuration   Reject/flag before compliance
  Timeout                   Controlled retry/error

The key principle is **fail-safe assessment rather than optimistic
compliance**.

------------------------------------------------------------------------

## 30. Limitations

Based on the supplied project material, the following limitations must
be stated explicitly:

1.  The actual application source repository was not supplied for this
    documentation pass.
2.  No implementation can be claimed solely from architecture documents.
3.  Exact dependency versions are not available.
4.  Exact database migrations/schema are not available.
5.  Exact environment-variable names are not available.
6.  Exact authentication/RBAC implementation is not available.
7.  Exact parser implementation is not available.
8.  Exact vendor support status is not source-verified.
9.  Exact compliance rule counts and framework versions are not
    specified.
10. Exact risk-scoring formula is not specified.
11. Exact drift algorithm is not specified.
12. Exact what-if implementation is not specified.
13. Exact remediation templates are not specified.
14. Exact report-generation implementation is not specified.
15. Actual performance benchmarks are not provided.
16. Actual automated test results are not provided.

These limitations should be removed or updated after the source
repository is available.

------------------------------------------------------------------------

## 31. Future Work

The Cyber Layer work report identifies the following implementation
priorities:

-   Persist findings and evidence.
-   Implement contextual risk.
-   Implement remaining vendor parsers.
-   Implement validated remediation templates.
-   Implement drift.
-   Implement AI unknown-syntax suggestion and approval.
-   Implement what-if simulation.
-   Expand controls and vendor coverage.
-   Integrate reporting and dashboard metrics.
-   Run adversarial, security, and end-to-end testing.

The AI work package additionally identifies future refinement around:

-   Retrieval of approved mappings.
-   Embedding-based similarity search.
-   Optional LLM/RAG inference.
-   Structured response validation.
-   Provenance and mapping reuse.
-   Safe failure and human approval.

------------------------------------------------------------------------

## 32. Contribution Guidelines

When contributing to the project:

### Parser Contributions

-   Keep vendor-specific parsing isolated.
-   Preserve source locations.
-   Never silently discard unknown security-relevant syntax.
-   Do not embed framework compliance decisions inside parsers.

### Compliance Contributions

-   Keep rules deterministic.
-   Define expected and actual values clearly.
-   Preserve evidence.
-   Handle UNKNOWN and ERROR explicitly.
-   Do not convert processing errors into PASS.

### AI Contributions

-   Treat configuration as untrusted.
-   Redact secrets where possible.
-   Validate model output against an explicit schema.
-   Do not allow AI output to directly determine compliance.
-   Require human approval for reusable mappings.
-   Preserve mapping provenance.

### Security Contributions

-   Do not commit credentials.
-   Do not add autonomous production command execution.
-   Validate uploaded files and input paths.
-   Add security tests for newly introduced attack surfaces.

------------------------------------------------------------------------

## 33. Troubleshooting

### Configuration is classified as UNKNOWN

Check:

1.  Vendor detection result.
2.  Platform/OS identification.
3.  Parser support.
4.  Unknown syntax records.
5.  Whether an approved AI mapping exists.
6.  Whether the mapping has been human-approved.

### Compliance result is UNKNOWN

Check:

1.  Whether the relevant security fact was normalized.
2.  Whether the control is applicable.
3.  Whether required evidence exists.
4.  Whether the parser reported an unknown construct.

### Compliance result is ERROR

An ERROR must not be treated as PASS.

Inspect:

1.  Parser diagnostics.
2.  Rule execution errors.
3.  Missing or malformed data.
4.  Service/API errors.
5.  Audit job status.

### AI analysis fails

The deterministic compliance path should remain available wherever
sufficient canonical information already exists.

### Vendor detection is uncertain

Prefer explicit uncertainty over incorrect high-confidence
classification.

------------------------------------------------------------------------

## 34. Project Status

  Component                           Status from Supplied Material
  ----------------------------------- -------------------------------
  Overall architecture                Documented
  Vendor detection design             Documented
  Four-vendor strategy                Documented
  Parser architecture                 Documented
  Canonical security model            Documented
  Deterministic compliance design     Documented
  CIS/NIST/STIG/ISO mapping concept   Documented
  Evidence model                      Documented
  Risk concept                        Documented
  Drift concept                       Documented
  What-if concept                     Documented
  Remediation concept                 Documented
  AI unknown-syntax workflow          Documented
  AI human-approval boundary          Documented
  API contract                        Documented
  Actual source implementation        Not verified
  Actual database schema              Not verified
  Actual automated tests              Not verified
  Actual deployment configuration     Not verified
  Production readiness                Not established

### Implementation Principle

The strongest end-to-end demonstration described by the project
documentation is:

``` text
Four Representative Vendors
        ↓
Different Configuration Syntax
        ↓
One Canonical Security Model
        ↓
One Deterministic Compliance Engine
        ↓
Explainable Findings + Evidence
        ↓
Risk / Drift / What-If
        ↓
Validated Action
```

------------------------------------------------------------------------

## 35. License

A project license was not specified in the supplied project material.

Until an official license is selected and added to the repository, do
not claim an open-source license.

Recommended repository action:

``` text
Choose an appropriate license with the project owner
and add the corresponding LICENSE file.
```

------------------------------------------------------------------------

## 36. Final Architecture Summary

NEXUS-COMPLY is centered on a separation-of-concerns model:

``` text
Vendor Syntax
     │
     ▼
Vendor Detection
     │
     ▼
Vendor Parser
     │
     ▼
Canonical Security Model
     │
     ├──────────────► AI Assistance for Unknown Syntax
     │                       │
     │                       ▼
     │                 Human Approval
     │
     ▼
Applicability
     │
     ▼
Deterministic Compliance Rules
     │
     ▼
PASS / FAIL / UNKNOWN / N/A / ERROR
     │
     ├──► Evidence
     ├──► Findings
     ├──► Risk
     ├──► Drift
     ├──► What-If
     ├──► Remediation
     └──► Reporting
```

The core engineering principle is:

> **AI suggests meaning and mappings; humans approve reusable mappings;
> deterministic rules remain authoritative for compliance decisions.**

This architecture avoids duplicating vendor-specific logic across every
compliance framework and provides a clear boundary between probabilistic
AI assistance and deterministic security assessment.
