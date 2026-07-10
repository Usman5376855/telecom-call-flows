
# 5G PDU Session Establishment

## Overview

The PDU Session Establishment Procedure enables a User Equipment (UE) to establish an IP connectivity session through the 5G Core Network. During this procedure, the network allocates an IP address, selects the appropriate User Plane Function (UPF), applies subscriber policies, establishes PFCP sessions, and creates GTP-U tunnels for user-plane data transfer.

The procedure is defined in **3GPP TS 23.502** and represents one of the most important signaling flows in the 5G Core.

---

## Objectives

- Establish a PDU Session for the UE.
- Allocate an IP address to the UE.
- Select an appropriate UPF.
- Establish user-plane connectivity.
- Configure QoS flows.
- Apply subscriber policy and charging rules.
- Enable end-to-end data connectivity.

---

## Network Functions Involved

| Network Function | Role |
|------------------|------|
| UE | Initiates the PDU Session Establishment request. |
| gNodeB (gNB) | Forwards NAS signaling and establishes N3 user-plane connectivity. |
| AMF | Handles NAS signaling and coordinates session establishment. |
| SMF | Creates and manages the PDU Session, allocates IP addresses, and controls the UPF. |
| UPF | Establishes the user plane and forwards user traffic. |
| PCF | Provides QoS and policy rules for the session. |
| UDM | Provides subscriber and session subscription information. |
| NRF | Provides Network Function discovery through the Service Based Architecture. |
| AUSF | Provides authentication support during subscriber authentication procedures. |

---

## PDU Session Establishment Call Flow

![5G PDU Session Establishment](images/pdu-session-establishment-call-flow.png)

---

# Detailed Procedure

The PDU Session Establishment procedure consists of multiple signaling exchanges between UE, gNB, AMF, SMF, PCF, UDM, and UPF.

The procedure can be divided into the following steps:

1. UE sends PDU Session Establishment Request
2. AMF selects SMF
3. SMF creates PDU Session context
4. SMF retrieves subscriber session data from UDM
5. SMF obtains policy rules from PCF
6. SMF selects UPF
7. SMF establishes PFCP session with UPF
8. gNB establishes QoS Flow and Data Radio Bearer
9. UE receives PDU Session Establishment Accept
10. User-plane traffic becomes active

---

## 1. UE Sends PDU Session Establishment Request

After successful Registration, the UE initiates a PDU Session Establishment procedure to request data connectivity.

The UE sends:
NAS: PDU Session Establishment Request

The message is transported:
UE → gNB → AMF

using the N1 interface.

The request contains:

- PDU Session ID
- Request Type
- DNN (Data Network Name)
- S-NSSAI
- PDU Session Type (IPv4 / IPv6 / IPv4v6)
- SSC Mode

Purpose:

The UE informs the network about the requested data session parameters.

---

## 2. AMF Receives Request and Selects SMF

The AMF receives the NAS request from the gNB.

The AMF determines the SMF responsible for managing this PDU Session.

SMF selection is based on:

- DNN
- S-NSSAI
- UE location
- Operator configuration

Interface:

N11 (AMF - SMF)

Message:

Nsmf_PDUSession_CreateSMContext Request


The AMF forwards:

- SUPI
- PDU Session ID
- DNN
- S-NSSAI
- Access Type
- UE location information

to the selected SMF.

---

## 3. SMF Creates PDU Session Context

The SMF creates the internal session context and becomes responsible for:

- PDU Session management
- IP address allocation
- UPF selection
- QoS management
- User-plane establishment

The SMF starts interaction with other Network Functions required for session establishment.

SMF = The SMF acts as the control plane entity responsible for PDU Session lifecycle management.

---

## 4. SMF Retrieves Subscriber Data from UDM

The SMF requests subscriber session information from UDM.

Interface:
N10

Service:

Nudm_SDM (Subscription Data Management)

Information retrieved:

- Allowed DNN
- Allowed S-NSSAI
- Session subscription information
- QoS restrictions

Purpose:

Verify that the subscriber is allowed to establish the requested PDU Session.

---


