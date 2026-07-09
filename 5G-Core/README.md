# 5G Core (5GC)

The **5G Core (5GC)** is the cloud-native core network defined by the **3rd Generation Partnership Project (3GPP)** for Fifth Generation (5G) mobile communication systems. It represents a fundamental architectural evolution from the LTE Evolved Packet Core (EPC), replacing interface-centric network design with a **Service-Based Architecture (SBA)** in which Network Functions (NFs) communicate through standardized service-based interfaces using HTTP/2 and RESTful APIs.

Unlike previous generations, the 5G Core is designed as a fully software-defined, cloud-native platform that separates the **Control Plane (CP)** from the **User Plane (UP)**. This separation enables independent scaling, flexible service deployment, distributed user plane processing, network slicing, edge computing, and lifecycle automation using modern virtualization and container orchestration technologies.

The 5GC provides the foundation for supporting diverse service categories, including:

- **Enhanced Mobile Broadband (eMBB)** for high-speed mobile connectivity
- **Ultra-Reliable Low-Latency Communications (URLLC)** for mission-critical applications
- **Massive Machine-Type Communications (mMTC)** for large-scale IoT deployments

By introducing modular network functions, service discovery, policy-driven control, and standardized APIs, the 5G Core enables operators to build highly scalable, resilient, and programmable mobile networks capable of supporting both consumer and enterprise services.

This documentation provides a comprehensive and practical reference covering the architecture, network functions, service-based interfaces, mobility management, session management, QoS, network slicing, signaling procedures, troubleshooting methodologies, and operational best practices defined by 3GPP.

---

# Objectives

This documentation is intended to serve as a structured technical reference for telecom engineers, network architects, students, and mobile core professionals working with 5G Standalone (SA) and cloud-native mobile core networks.

The primary objectives are to:

- Explain the architecture and design principles of the 5G Core.
- Describe the responsibilities of every Network Function (NF).
- Illustrate Service-Based Architecture (SBA) interactions.
- Document end-to-end signaling procedures using professional call flow diagrams.
- Explain mobility management and session management procedures.
- Describe QoS architecture and policy control.
- Provide practical troubleshooting guidance based on real operational scenarios.
- Reference the relevant 3GPP specifications for every topic.
- Build a vendor-neutral knowledge base suitable for learning, design, deployment, and operations.

---

# Key Design Principles

The 5G Core introduces several architectural principles that distinguish it from previous generations of mobile core networks:

- Service-Based Architecture (SBA)
- Cloud-Native Network Functions
- Control and User Plane Separation (CUPS)
- Stateless Control Plane Design
- Service Discovery through the NRF
- RESTful API Communication using HTTP/2
- Distributed User Plane Architecture
- Network Slicing
- Policy-Based Network Control
- Edge Computing Integration
- Automation and Orchestration
- High Availability and Horizontal Scalability

---

# Core Network Functions

| Network Function | Primary Responsibility |
|------------------|------------------------|
| **AMF** | Access registration, mobility management, NAS signaling and connection management |
| **SMF** | PDU Session management, IP address allocation, UPF selection and user-plane control |
| **UPF** | User-plane packet forwarding, QoS enforcement, traffic steering and charging enforcement |
| **UDM** | Subscriber profile management, subscription data and identifier management |
| **AUSF** | Authentication of subscribers using 5G AKA or EAP-AKA' |
| **PCF** | Policy and QoS decision making |
| **NRF** | Registration and discovery of Network Functions |
| **NSSF** | Network Slice selection and management |
| **NEF** | Secure exposure of network capabilities to external applications |
| **AF** | Application-level policy influence and service interaction |
| **UDSF** | Storage of unstructured network function data |

---

# Documentation Roadmap

## Architecture

- 5G Core Overview
- Service-Based Architecture (SBA)
- Network Functions
- Service-Based Interfaces
- Reference Points
- QoS Architecture
- Network Slicing
- Control and User Plane Separation (CUPS)

## Mobility Management

- Registration
- Deregistration
- Service Request
- Paging
- UE Context Release
- AMF Relocation

## Session Management

- PDU Session Establishment
- PDU Session Modification
- PDU Session Release
- User Plane Management

## Security

- Authentication
- Security Mode Control
- NAS Security
- Key Derivation
- Subscription Authentication

## Mobility Procedures

- Xn Handover
- N2 Handover
- Inter-System Mobility
- Idle Mode Mobility

---

# Procedure Status

| Topic | Status |
|------|:------:|
| 5G Core Overview | ✅ Completed |
| Service-Based Architecture | ⏳ Planned |
| Network Functions | ⏳ Planned |
| Service-Based Interfaces | ⏳ Planned |
| Reference Points | ⏳ Planned |
| Registration | ⏳ Planned |
| Authentication | ⏳ Planned |
| Security Mode Control | ⏳ Planned |
| PDU Session Establishment | ⏳ Planned |
| PDU Session Modification | ⏳ Planned |
| PDU Session Release | ⏳ Planned |
| Service Request | ⏳ Planned |
| Paging | ⏳ Planned |
| UE Context Release | ⏳ Planned |
| Xn Handover | ⏳ Planned |
| N2 Handover | ⏳ Planned |

---

# Scope

This documentation focuses on **3GPP Release 15 and later**, covering standardized 5G Standalone (SA) architecture and signaling procedures. The content is intentionally **vendor-neutral**, allowing engineers to apply the concepts regardless of equipment vendor or deployment model.

Where applicable, explanations are supported by sequence diagrams, signaling message flows, operational considerations, troubleshooting guidance, and references to the corresponding 3GPP specifications.

---
---

# Documentation Structure

The 5G Core documentation is organized into the following sections:

## Architecture

- Service-Based Architecture (SBA)
- Network Functions
- Service-Based Interfaces
- Reference Points
- Network Slicing
- QoS Architecture

## Procedures

- Registration
- Authentication
- Security Mode Control
- Service Request
- Paging
- Deregistration
- PDU Session Establishment
- PDU Session Modification
- PDU Session Release
- Xn Handover
- N2 Handover
- UE Context Release

## Operational Guides

- Troubleshooting
- Best Practices
- KPI Analysis
- Common Failure Scenarios
# References

The following 3GPP Technical Specifications form the primary references for this documentation:

| Specification | Description |
|--------------|-------------|
| **3GPP TS 23.501** | System Architecture for the 5G System (5GS) |
| **3GPP TS 23.502** | Procedures for the 5G System (5GS) |
| **3GPP TS 24.501** | NAS Protocol for the 5G System |
| **3GPP TS 29.500** | Service-Based Architecture Framework |
| **3GPP TS 29.501** | Principles and Guidelines for SBA Services |
| **3GPP TS 29.502** | Access and Mobility Management Services |
| **3GPP TS 29.503** | Unified Data Management Services |
| **3GPP TS 29.504** | Policy Control Services |
| **3GPP TS 29.510** | Network Repository Function (NRF) Services |
| **3GPP TS 38.300** | NR and NG-RAN Overall Description |
| **3GPP TS 38.413** | NG Application Protocol (NGAP) |
| **3GPP TS 38.331** | NR Radio Resource Control (RRC) |
