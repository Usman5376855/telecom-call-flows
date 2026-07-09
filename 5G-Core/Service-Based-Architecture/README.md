
# Service-Based Architecture (SBA)

## Overview

The **Service-Based Architecture (SBA)** is the architectural foundation of the 5G Core (5GC), introduced by the 3rd Generation Partnership Project (3GPP) to replace the traditional point-to-point interface model used in previous generations of mobile core networks.

Unlike the LTE Evolved Packet Core (EPC), where network functions communicate through dedicated interfaces such as S1-MME, S11, S5/S8, and Gx, the 5G Core adopts a service-oriented communication model in which Network Functions (NFs) expose standardized services that can be discovered and consumed by other authorized Network Functions.

Communication between Network Functions is performed over the **Service-Based Interface (SBI)** using HTTP/2 and RESTful APIs with JSON data encoding. This enables dynamic service discovery, independent scaling, cloud-native deployment, and greater operational flexibility.

The Service-Based Architecture simplifies network evolution by decoupling Network Functions, allowing services to be added, upgraded, or scaled independently without impacting the overall architecture. It also provides the foundation for network slicing, automation, service orchestration, and modern cloud-native mobile core deployments.

As the fundamental architectural model of the 5G Core, the SBA enables interoperable, programmable, and highly scalable mobile networks capable of supporting a wide range of consumer, enterprise, and mission-critical services.

---

# Objectives

This document provides a comprehensive overview of the Service-Based Architecture (SBA) within the 5G Core (5GC), explaining its architectural principles, communication model, and operational benefits as defined by the 3GPP.

The objectives of this document are to:

- Explain the evolution from interface-based architecture in LTE EPC to the service-oriented architecture of the 5G Core.
- Describe the role of the Service-Based Interface (SBI) in enabling communication between Network Functions (NFs).
- Explain how Network Functions register, discover, and consume services through the Network Repository Function (NRF).
- Illustrate the producer–consumer communication model used by the 5G Core.
- Describe the protocols, security mechanisms, and technologies used within the SBA.
- Highlight the operational advantages of cloud-native and service-based network design.
- Provide the architectural foundation required to understand subsequent 5G Core signaling procedures and Network Functions.

  ---

# Key Characteristics

The Service-Based Architecture introduces a modern communication framework that enables modular, scalable, and interoperable Network Functions within the 5G Core.

Its key characteristics include:

- Service-oriented communication between Network Functions.
- Standardized Service-Based Interfaces (SBIs).
- HTTP/2 and RESTful API-based messaging.
- JSON-based data representation.
- Dynamic service registration and discovery through the NRF.
- Producer–Consumer communication model.
- Stateless Control Plane Network Functions.
- Independent scaling of individual services.
- Cloud-native deployment using virtualization and container technologies.
- Secure communication using TLS and OAuth 2.0 authorization.
- High availability through redundant and distributed Network Functions.
- Vendor-neutral interoperability based on 3GPP standards.

- ---

# SBA Components

The Service-Based Architecture (SBA) is composed of multiple functional components that work together to provide standardized, service-oriented communication within the 5G Core. Each component has a specific responsibility, enabling Network Functions (NFs) to discover, authenticate, and consume services in a secure and scalable manner.

Unlike traditional interface-based architectures, where communication paths are statically defined, the SBA enables dynamic interaction between Network Functions through standardized Service-Based Interfaces (SBIs). This modular approach improves flexibility, scalability, resiliency, and interoperability across the mobile core network.

The principal components of the Service-Based Architecture are described below.

| Component | Description |
|-----------|-------------|
| **Network Functions (NFs)** | Functional entities that provide specific control plane services such as mobility management, session management, policy control, subscriber management, and authentication. |
| **Service-Based Interface (SBI)** | The standardized communication framework that enables Network Functions to exchange information using HTTP/2 and RESTful APIs. |
| **Network Repository Function (NRF)** | Maintains the registry of available Network Functions, their supported services, capabilities, and operational status. It enables service registration and discovery. |
| **NF Service Producer** | A Network Function that publishes one or more services for consumption by other authorized Network Functions. |
| **NF Service Consumer** | A Network Function that discovers and invokes services provided by another Network Function through the Service-Based Interface. |
| **NF Service Instance** | A deployable instance of a Network Function capable of providing one or more standardized services. Multiple instances may exist to support redundancy and load balancing. |
| **NF Profile** | A standardized profile registered with the NRF containing service information, supported capabilities, endpoint addresses, version information, and operational status of a Network Function. |
| **Service Registry** | The logical repository maintained by the NRF that stores information about all registered Network Functions and their available services. |
| **Security Framework** | Provides authentication, authorization, confidentiality, and integrity protection for Service-Based Interface communication using TLS and OAuth 2.0. |
| **Service Discovery Mechanism** | Enables Network Functions to dynamically locate suitable service providers based on service type, supported capabilities, network slice, locality, or other selection criteria. |

Together, these components form the foundation of the Service-Based Architecture, enabling the 5G Core to operate as a cloud-native, service-oriented platform capable of supporting highly scalable, resilient, and programmable mobile networks.

---
# Architecture Diagram

The following diagram illustrates the overall Service-Based Architecture (SBA) of the 5G Core. It shows the major Network Functions (NFs), the Service-Based Interface (SBI), and the communication model used between Network Functions.

![5G Core Service-Based Architecture](images/service-based-architecture-overview.png)


# Service-Based Interface (SBI)

## Overview

The **Service-Based Interface (SBI)** is the standardized communication framework that enables interaction between Network Functions (NFs) within the 5G Core (5GC). Introduced as part of the Service-Based Architecture (SBA), the SBI replaces the traditional point-to-point interface model used in previous mobile core networks with a flexible, service-oriented communication mechanism.

Unlike the LTE Evolved Packet Core (EPC), where Network Functions communicate through dedicated interfaces such as S11, Gx, and S6a using protocols like GTP and Diameter, the 5G Core enables Network Functions to expose standardized services that can be dynamically discovered and consumed by other authorized Network Functions.

Communication over the SBI is based on **HTTP/2**, **RESTful APIs**, and **JSON** data encoding, allowing Network Functions to exchange information using modern web technologies while maintaining interoperability across multi-vendor deployments.

The Service-Based Interface enables dynamic service discovery, independent Network Function scaling, simplified service integration, and cloud-native deployment, making it one of the key innovations of the 5G Core architecture.

---

## Communication Technologies

The Service-Based Interface utilizes widely adopted Internet technologies to provide secure, reliable, and standardized communication between Network Functions.

| Technology | Purpose |
|------------|---------|
| **HTTP/2** | Transport protocol used for service-based communication between Network Functions. |
| **RESTful APIs** | Standardized application programming interfaces used to request and provide network services. |
| **JSON** | Lightweight data format used for exchanging service information between Network Functions. |
| **TLS** | Provides confidentiality, integrity, and authentication for SBI communication. |
| **OAuth 2.0** | Provides authorization and access control for service requests between Network Functions. |

---

## Service Communication Model

Within the Service-Based Architecture, every Network Function may operate as both a **Service Producer** and a **Service Consumer**.

A Service Producer exposes one or more standardized services through the Service-Based Interface, while a Service Consumer discovers those services through the Network Repository Function (NRF) and invokes them using HTTP/2 requests.

This communication model eliminates static interface dependencies and enables Network Functions to interact dynamically based on service availability and operational requirements.

---

## Common Service-Based Interfaces

The 3GPP defines a standardized set of service interfaces that enable communication between Network Functions.

| Service Interface | Description |
|-------------------|-------------|
| **Namf** | Services provided by the Access and Mobility Management Function (AMF). |
| **Nsmf** | Session Management services provided by the Session Management Function (SMF). |
| **Nudm** | Subscriber management services provided by the Unified Data Management (UDM). |
| **Nausf** | Authentication services provided by the Authentication Server Function (AUSF). |
| **Npcf** | Policy Control services provided by the Policy Control Function (PCF). |
| **Nnrf** | Registration and discovery services provided by the Network Repository Function (NRF). |
| **Nnef** | Network capability exposure services provided by the Network Exposure Function (NEF). |
| **Nnssf** | Network Slice Selection services provided by the Network Slice Selection Function (NSSF). |
| **Nudsf** | Unstructured data storage services provided by the Unstructured Data Storage Function (UDSF). |

---

## Benefits of the Service-Based Interface

The adoption of the Service-Based Interface provides several architectural and operational advantages:

- Standardized communication based on open Internet technologies.
- Dynamic service registration and discovery.
- Vendor-neutral interoperability.
- Independent scaling of Network Functions.
- Simplified integration of new services.
- Cloud-native deployment and orchestration.
- Improved resiliency through service redundancy.
- Flexible deployment across centralized and distributed cloud environments.
- Enhanced security through TLS and OAuth 2.0.
- Support for automation and lifecycle management.

The Service-Based Interface is a fundamental enabler of the cloud-native 5G Core, providing the communication framework that allows Network Functions to operate as modular, reusable, and independently scalable services.
