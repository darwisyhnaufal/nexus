# NEXUS
### Network Engineering & Change Intelligence System

**An engineering platform for understanding network dependencies and predicting the potential impact of configuration changes.**

## Overview

NEXUS is a network engineering platform designed to analyse network device configurations, reconstruct network topology, and assess the potential impact of proposed configuration changes before they are deployed.

In traditional network management, engineers often need to understand how a configuration change on one device might affect other devices, network paths, and dependent services.

NEXUS aims to address this challenge by transforming network configurations into structured information that can be analysed to identify dependencies, detect inconsistencies, and evaluate potential network impact.

The central question behind NEXUS is:

> If I change something in my network, what else could break?

## Problem Statement

Network infrastructure consists of interconnected devices, configurations, routing policies, interfaces, and network dependencies.

A seemingly small configuration change can have consequences beyond the device being modified. For example, changing an interface address, modifying a route, or altering a VLAN configuration could affect connectivity elsewhere in the network.

Understanding these relationships manually can be difficult, especially as network complexity increases.

NEXUS aims to provide an engineering-oriented approach to understanding network configurations and evaluating the consequences of proposed changes before deployment.

## Objectives

- Parse and interpret network device configurations.
- Convert configuration files into a structured internal representation.
- Reconstruct network topology and device relationships.
- Store and compare historical configuration snapshots.
- Identify configuration changes and their potential consequences.
- Analyse network dependencies and potential blast radius.
- Validate network reachability and behaviour.
- Generate evidence-backed engineering reports.

## Core Features

| Feature | Description |
|---|---|
| Configuration Parser | Extracts structured information from network configurations. |
| Topology Reconstruction | Builds a representation of network devices and their connections. |
| Configuration Snapshots | Stores versions of network configurations. |
| Semantic Configuration Diff | Identifies meaningful configuration changes. |
| Blast-Radius Analysis | Identifies devices and network dependencies potentially affected by a change. |
| Network Validation | Evaluates network behaviour and reachability. |
| Engineering Reports | Presents analysis results and supporting evidence. |

## Technology Stack

| Technology | Purpose |
|---|---|
| Python | Core application and analysis logic |
| NetworkX | Network graph and dependency analysis |
| Batfish | Network configuration and behaviour validation |
| FastAPI | Backend API |
| PostgreSQL | Persistent data storage |
| React + TypeScript | User interface |
| Docker | Application deployment |
| Containerlab | Network lab environment |
| FRRouting | Simulated network routing |

## Initial Project Scope

The initial version will focus on:

- Cisco IOS-style configurations.
- IPv4 addressing.
- Network interfaces.
- VLAN information.
- Static routing.
- Small simulated networks consisting of approximately 3–5 devices.

Additional protocols, vendors, and advanced analysis capabilities may be introduced after the initial system is working reliably.

## Development Philosophy

NEXUS prioritises deterministic network analysis, explainable results, and engineering evidence.

Artificial intelligence may eventually assist with explaining findings, but it will not replace the underlying network analysis engine or invent validation results.

The project will be developed incrementally, with automated testing, documentation, and reproducible examples.

## Project Status

**Current stage:** Initial project foundation.

NEXUS is under active development. Features described above represent the intended project direction and should not be interpreted as already implemented.

## Repository Structure

```text
nexus/
├── docs/
│   ├── requirements.md
│   └── architecture.md
├── src/
│   └── nexus/
├── tests/
├── examples/
├── scripts/
├── README.md
├── .gitignore
└── LICENSE
```

## Long-Term Vision

The long-term goal is to develop NEXUS into a network engineering intelligence platform that helps engineers understand network state, evaluate proposed changes, and make better-informed infrastructure decisions.

---

**Project:** NEXUS  
**Full Name:** Network Engineering & Change Intelligence System  
**Language:** Python  
**License:** MIT