# NEXUS — System Architecture

**Document Version:** 1.0  
**Project:** Network Engineering & Change Intelligence System  
**Status:** Proposed Architecture

## 1. Architecture Overview

NEXUS follows a modular architecture in which network configurations are transformed into structured data before being processed by topology reconstruction, change analysis, and network validation components.

The architecture separates configuration parsing from network analysis.

This separation is important because different network vendors may use different configuration syntaxes while sharing common network concepts such as devices, interfaces, IP addresses, routes, and VLANs.

The long-term objective is to allow additional vendor parsers to use the same internal network representation without rewriting the analysis engine.

## 2. High-Level Architecture

```text
             NETWORK CONFIGURATIONS
                       |
                       v
             CONFIGURATION INPUT
                       |
                       v
              CONFIGURATION PARSER
                       |
                       v
             INTERNAL DATA MODEL
                       |
              +--------+--------+
              |                 |
              v                 v
       TOPOLOGY ENGINE    SNAPSHOT STORAGE
              |                 |
              |                 v
              |          CONFIGURATION DIFF
              |                 |
              +--------+--------+
                       |
                       v
              CHANGE ANALYSIS ENGINE
                       |
                       v
               IMPACT ASSESSMENT
                       |
                       v
               VALIDATION ENGINE
                       |
                       v
               REPORT GENERATOR
                       |
                       v
                 USER INTERFACE
```

## 3. Main Components

### 3.1 Configuration Input

Responsible for accepting network configuration files.

Initial responsibilities:

- Accept supported configuration files.
- Identify input files and associated device information.
- Perform basic input validation.
- Pass configuration content to the appropriate parser.

### 3.2 Configuration Parser

Responsible for interpreting network configuration syntax.

Initial target: Cisco IOS-style configurations.

Responsibilities:

- Extract device hostname.
- Identify interfaces.
- Extract interface descriptions.
- Extract IPv4 addresses and subnet masks.
- Identify administrative interface status.
- Extract supported VLAN and static route information.
- Report unsupported or malformed configuration sections.

The parser must not make assumptions about network connectivity simply because two devices contain related configuration values.

### 3.3 Internal Network Data Model

Provides a consistent representation of network information.

Proposed initial entities:

**Device**
- Device identifier.
- Hostname.
- Vendor.
- Operating system.
- Interfaces.

**Interface**
- Interface identifier.
- Interface name.
- Description.
- IPv4 address.
- Subnet mask or prefix length.
- Administrative status.
- VLAN information.

**Route**
- Destination network.
- Next-hop address.
- Outgoing interface.
- Route type.

The data model acts as the boundary between vendor-specific parsing and vendor-independent analysis.

### 3.4 Topology Engine

Responsible for constructing a network graph.

NetworkX will initially be used to represent network relationships.

Proposed graph concepts:

- Nodes represent network devices.
- Edges represent identified or inferred relationships.
- Edge attributes contain relevant connection information.
- Relationship confidence and evidence should be retained where available.

A shared IP subnet alone is not sufficient proof that two devices are physically connected. Topology reconstruction must distinguish confirmed, inferred, and unknown relationships.

### 3.5 Snapshot Storage

Responsible for retaining configuration versions.

Each snapshot should include:

- Device identifier.
- Configuration content or reference.
- Timestamp.
- Content hash.
- Relevant metadata.

This allows historical configuration states to be compared.

PostgreSQL is a proposed future storage technology. Early development may use local files and simple persistence mechanisms.

### 3.6 Configuration Diff Engine

Responsible for comparing two versions of a device configuration.

The engine should identify meaningful changes such as:

- Interface address modifications.
- Interface status changes.
- Route additions and removals.
- VLAN changes.
- Interface configuration changes.

The goal is semantic comparison rather than simply reporting that two text files differ.

### 3.7 Change Analysis Engine

The central intelligence component of NEXUS.

It combines configuration changes with network relationships to identify potential consequences.

Responsibilities:

- Identify directly modified network elements.
- Determine relevant dependencies.
- Trace affected network relationships.
- Estimate potential blast radius.
- Explain why an element was identified as potentially affected.

The engine should distinguish between direct effects, indirect potential effects, and uncertain inferences.

### 3.8 Validation Engine

Responsible for evaluating network behaviour using supported analysis methods.

Batfish is the proposed analysis integration.

Potential validation capabilities:

- Network reachability checks.
- Routing behaviour analysis.
- Configuration consistency checks.
- Comparison of network behaviour across configuration snapshots.

Validation findings must include their supporting evidence and analysis context.

### 3.9 Report Generator

Responsible for producing engineering-oriented results.

Reports should include:

- Analysed configuration changes.
- Affected devices and network elements.
- Potential consequences.
- Validation findings.
- Supporting evidence.
- Limitations and uncertainties.

### 3.10 User Interface

The initial development will prioritise the backend and analysis engine.

A React and TypeScript interface may be introduced later to present:

- Network topology.
- Configuration changes.
- Impact analysis.
- Validation results.
- Engineering reports.

## 4. Proposed Technology Stack

| Component | Technology | Reason |
|---|---|---|
| Core application | Python | Main implementation language |
| Graph analysis | NetworkX | Network graph representation |
| Network validation | Batfish | Configuration and behaviour analysis |
| API | FastAPI | Backend API |
| Database | PostgreSQL | Persistent storage |
| Frontend | React + TypeScript | Interactive interface |
| Deployment | Docker | Reproducible deployment |
| Network simulation | Containerlab | Controlled network lab |
| Routing | FRRouting | Simulated routing environments |

Technologies will be introduced only when required by the relevant development milestone.

## 5. Initial Development Architecture

The first implementation will be a local Python application.

The initial development stages will avoid unnecessary infrastructure.

Proposed initial source structure:

```text
src/
└── nexus/
    ├── __init__.py
    ├── parser/
    │   ├── __init__.py
    │   └── cisco_ios.py
    ├── models/
    │   ├── __init__.py
    │   └── network.py
    ├── topology/
    │   └── __init__.py
    ├── analysis/
    │   └── __init__.py
    └── main.py
```

This structure is a starting point and may evolve as the project grows.

## 6. Data Processing Flow

The intended processing sequence is:

1. The user provides network configuration files.
2. NEXUS validates the input.
3. The appropriate parser interprets each configuration.
4. Parsed information is converted into the internal data model.
5. The topology engine constructs network relationships.
6. Configuration snapshots are stored.
7. The diff engine identifies meaningful changes between snapshots.
8. The change analysis engine identifies potentially affected network elements.
9. The validation engine evaluates supported network behaviour.
10. The report generator produces an evidence-backed engineering report.

## 7. Architectural Principles

### 7.1 Modularity

Each component should have a clear responsibility and be independently testable.

### 7.2 Vendor Independence

Vendor-specific configuration parsing should be separated from the common network data model.

### 7.3 Explainability

Every significant analysis finding should have an understandable explanation.

### 7.4 Evidence-Based Analysis

NEXUS should distinguish between facts extracted from configuration, relationships inferred from available information, and results validated by an analysis engine.

### 7.5 Testability

Core logic should be testable without requiring a complete network lab or production infrastructure.

### 7.6 Incremental Development

The project should introduce new technologies only when they solve a concrete engineering requirement.

## 8. Security and Operational Considerations

- Configuration files may contain sensitive information.
- Credentials and secrets must not be committed to GitHub.
- Sample configurations should use fictional addresses and credentials.
- Uploaded configuration files should be treated as untrusted input.
- Analysis must not automatically deploy changes to network devices.
- Reports should clearly state analysis limitations.

## 9. Future Architecture Extensions

Possible future extensions include:

- Multi-vendor parser modules.
- Advanced routing analysis.
- More sophisticated network dependency modelling.
- Policy and access-control validation.
- Historical network state comparison.
- AI-assisted explanations of verified analysis findings.
- More comprehensive lab automation.

These extensions will be considered after the initial analysis engine is implemented and tested.