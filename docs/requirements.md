# NEXUS — Software Requirements Specification

**Document Version:** 1.0  
**Project:** Network Engineering & Change Intelligence System  
**Status:** Initial Specification

## 1. Introduction

### 1.1 Purpose

This document defines the functional and non-functional requirements for NEXUS.

It establishes the initial system scope, expected behaviour, constraints, and requirements that will guide development.

### 1.2 Project Overview

NEXUS is designed to analyse network device configurations, reconstruct network relationships, and assess the potential impact of configuration changes.

The system aims to help network engineers understand the consequences of modifying network configurations before applying those changes to a live environment.

### 1.3 Problem Definition

Network configuration changes can introduce unintended consequences.

A modification to one device may affect connectivity, routing behaviour, or dependencies involving other devices.

Engineers often need to manually inspect multiple configurations and trace network relationships to understand these consequences.

NEXUS aims to automate parts of this engineering analysis through configuration parsing, topology reconstruction, change comparison, and network validation.

## 2. Project Objectives

The system shall aim to:

1. Interpret supported network device configurations.
2. Construct a structured representation of network infrastructure.
3. Identify relationships between network devices.
4. Compare different configuration versions.
5. Analyse potential consequences of configuration changes.
6. Validate selected network behaviour.
7. Produce understandable reports supported by analysis evidence.

## 3. Target Users

- Network engineers.
- Network administrators.
- Network infrastructure students.
- Network design and validation teams.

## 4. Functional Requirements

### 4.1 Configuration Management

| ID | Requirement | Priority |
|---|---|---|
| FR-01 | The system shall accept Cisco IOS-style configuration files. | High |
| FR-02 | The system shall validate whether an uploaded configuration can be processed. | High |
| FR-03 | The system shall extract device hostname information. | High |
| FR-04 | The system shall extract interface names, descriptions, and IP addressing information. | High |
| FR-05 | The system shall identify interface administrative status. | High |
| FR-06 | The system shall extract supported VLAN and static routing information. | Medium |

### 4.2 Internal Network Representation

| ID | Requirement | Priority |
|---|---|---|
| FR-07 | The system shall convert parsed configurations into a common internal data model. | High |
| FR-08 | The system shall associate configuration information with individual devices. | High |
| FR-09 | The system shall represent supported network relationships. | High |

### 4.3 Topology Reconstruction

| ID | Requirement | Priority |
|---|---|---|
| FR-10 | The system shall construct a graph representing network devices and their relationships. | High |
| FR-11 | The system shall identify potential connections using available configuration information. | High |
| FR-12 | The system shall distinguish confirmed relationships from inferred relationships where possible. | High |

### 4.4 Configuration Versioning

| ID | Requirement | Priority |
|---|---|---|
| FR-13 | The system shall store configuration snapshots. | High |
| FR-14 | The system shall associate snapshots with their respective devices. | High |
| FR-15 | The system shall compare two configuration snapshots. | High |
| FR-16 | The system shall identify meaningful configuration changes rather than relying only on raw text differences. | High |

### 4.5 Change Impact Analysis

| ID | Requirement | Priority |
|---|---|---|
| FR-17 | The system shall identify network elements directly affected by a configuration change. | High |
| FR-18 | The system shall analyse dependencies between affected network elements. | High |
| FR-19 | The system shall estimate the potential blast radius of a proposed change. | High |
| FR-20 | The system shall provide explanations for identified potential impacts. | High |

### 4.6 Network Validation

| ID | Requirement | Priority |
|---|---|---|
| FR-21 | The system shall support validation of selected network reachability scenarios. | High |
| FR-22 | The system shall identify selected configuration inconsistencies. | Medium |
| FR-23 | The system shall distinguish validated findings from heuristic predictions. | High |
| FR-24 | The system shall retain supporting evidence for validation results where available. | High |

### 4.7 Reporting

| ID | Requirement | Priority |
|---|---|---|
| FR-25 | The system shall generate an analysis report for a configuration change. | High |
| FR-26 | Reports shall identify relevant configuration changes. | High |
| FR-27 | Reports shall identify potentially affected devices and dependencies. | High |
| FR-28 | Reports shall explain the reasoning behind findings. | High |

## 5. Non-Functional Requirements

| ID | Requirement |
|---|---|
| NFR-01 | The system shall use a modular architecture. |
| NFR-02 | The system shall support automated unit and integration testing. |
| NFR-03 | The system shall provide understandable error messages. |
| NFR-04 | The system shall produce reproducible analysis results for identical inputs and analysis settings. |
| NFR-05 | The system shall maintain clear separation between parsing, data modelling, analysis, and reporting. |
| NFR-06 | The system shall document supported configuration syntax and known limitations. |
| NFR-07 | The system shall not silently interpret unsupported configuration commands as validated information. |
| NFR-08 | The system shall be designed so additional network vendors can be supported in the future. |

## 6. Initial Technical Scope

The first development version will support:

- Cisco IOS-style configuration syntax.
- IPv4 addressing.
- Interface configuration.
- Interface descriptions.
- Administrative interface status.
- VLAN information.
- Static routing.
- Small simulated network environments.

## 7. Out of Scope for Initial Version

The following are not part of the initial implementation:

- Complete multi-vendor configuration support.
- Full BGP and OSPF analysis.
- Production network configuration deployment.
- Automated modification of live network devices.
- Real-time infrastructure monitoring.
- AI-generated configuration changes.
- Enterprise-scale distributed deployment.

These may be considered for later development.

## 8. Assumptions and Constraints

### 8.1 Assumptions

- Users provide configuration files in supported formats.
- Initial testing uses controlled network configurations.
- The first implementation targets small network environments.
- Network relationships may need to be inferred when explicit information is unavailable.

### 8.2 Constraints

- Initial development is performed by one developer.
- The project uses a limited development budget.
- Initial implementation prioritises correctness and explainability over vendor coverage.
- External analysis tools must be evaluated for compatibility and resource requirements.

## 9. Initial Acceptance Criteria

The initial parser milestone will be considered complete when:

1. A supported Cisco IOS configuration can be loaded.
2. The parser can extract a hostname.
3. The parser can extract interface information.
4. IPv4 addresses and subnet masks are represented correctly.
5. Interface administrative status is represented correctly.
6. Invalid or unsupported input produces understandable errors or warnings.
7. Automated tests cover the supported parsing behaviour.

The initial topology milestone will be considered complete when a small sample network can be represented as a graph and its supported device relationships can be inspected.

## 10. Future Enhancements

Potential future capabilities include:

- Additional network vendors.
- Dynamic routing protocol analysis.
- More advanced policy and ACL validation.
- Network change risk classification.
- Historical network state analysis.
- Automated engineering recommendations.
- AI-assisted explanation of deterministic analysis results.

These enhancements are subject to technical evaluation and are not guaranteed features of the initial release.