# LTE Service Request

## Overview

The LTE Service Request procedure is a mobility management procedure used to transition a User Equipment (UE) from **ECM-IDLE** to **ECM-CONNECTED** state when the UE needs to send or receive user data.

Unlike the Attach procedure, the Service Request does not create a new EPS session or establish new default bearers. Instead, it resumes existing signaling and user-plane resources, allowing the UE to continue communication using previously established EPS bearer contexts.

The procedure is commonly triggered by uplink data from the UE or by downlink data received from the network, making it one of the most frequently observed procedures in commercial LTE networks.

This document explains the LTE Service Request procedure, signaling sequence, protocol messages, important Information Elements (IEs), timers, common failure scenarios, and troubleshooting considerations based on 3GPP specifications.

## Purpose

The primary objectives of the LTE Service Request procedure are:

- Resume communication for an idle UE without performing a new Attach.
- Transition the UE from ECM-IDLE to ECM-CONNECTED state.
- Re-establish S1 signaling between the UE, eNodeB, and MME.
- Reactivate existing user-plane bearers.
- Enable uplink and downlink packet transmission.
- Reduce signaling overhead while maintaining efficient resource utilization.
## When is Service Request Triggered?

The Service Request procedure may be initiated under the following conditions:

### 1. Mobile Originated Data

The UE has uplink data to transmit while it is in ECM-IDLE state.

### 2. Mobile Terminated Data

The network receives downlink data for an idle UE. The MME pages the UE, and after responding to the page, the UE initiates the Service Request procedure.

### 3. VoLTE Call Establishment

Before SIP signaling and voice bearer activation, the UE performs a Service Request to resume connectivity.

### 4. SMS over NAS

Some SMS procedures require the UE to establish signaling resources before message delivery.

### 5. Other NAS Signaling

The UE may initiate a Service Request before NAS procedures that require an active S1 connection.
> **Engineer Note**
>
> Service Request is one of the most common procedures observed in MME traces. Unlike the Attach procedure, it does not create new EPS bearer contexts. Instead, it restores the existing connection, allowing rapid data transfer while minimizing signaling overhead. In live networks, Service Request procedures are frequently triggered by smartphone applications generating background traffic.

## Network Elements

The following network elements participate in the LTE Service Request procedure:

| Network Element | Function |
|-----------------|----------|
| **UE (User Equipment)** | Initiates the Service Request when uplink data needs to be transmitted or responds to paging for downlink data. |
| **eNodeB (eNB)** | Establishes the radio connection and forwards NAS signaling to the MME over the S1 interface. |
| **MME (Mobility Management Entity)** | Processes the Service Request, validates the UE context, establishes signaling connections, and coordinates bearer activation. |
| **Serving Gateway (SGW)** | Reactivates the user-plane tunnel and forwards user data between the eNodeB and the PDN Gateway. |
| **PDN Gateway (PGW)** | Continues providing connectivity to external packet data networks using the existing EPS bearer. |
| **HSS (Home Subscriber Server)** | Normally does not participate unless subscriber information needs to be updated or revalidated. |

> **Engineer Note**
>
> Unlike the Attach procedure, the HSS is typically **not involved** during a normal Service Request because the UE context already exists in the MME. This makes the procedure significantly faster and reduces signaling within the EPC.

## Call Flow

The following call flow illustrates a typical LTE Service Request procedure. The exact signaling sequence may vary depending on the trigger (uplink or downlink), paging, and bearer modification requirements.

| Step | Message | Protocol | Interface |
|------|---------|----------|-----------|
| 1 | Service Request | NAS | UE → eNodeB → MME |
| 2 | Initial UE Message | S1AP | eNodeB → MME |
| 3 | Initial Context Setup Request | S1AP | MME → eNodeB |
| 4 | Initial Context Setup Response | S1AP | eNodeB → MME |
| 5 | Modify Bearer Request | GTPv2-C | MME → SGW |
| 6 | Modify Bearer Response | GTPv2-C | SGW → MME |
| 7 | Service Accept (if applicable) | NAS | MME → UE |
| 8 | User Data Transfer | GTP-U | UE ↔ Network |

> **Engineer Note**
>
> One of the key differences between the Attach and Service Request procedures is that **no new EPS bearer is created**. Instead, the existing bearer is resumed by updating the downlink tunnel information through the S11 Modify Bearer procedure.
UE
│
├── Service Request
│
eNB
│
├── Initial UE Message
│
MME
│
├── Initial Context Setup Request
│
├── Modify Bearer Request
│
SGW
│
├── Modify Bearer Response
│
MME
│
└── Initial Context Setup Complete






