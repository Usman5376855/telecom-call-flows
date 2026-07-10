
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
---

## 5. SMF Obtains Policy Rules from PCF

After receiving subscriber session information from UDM, the SMF communicates with the Policy Control Function (PCF) to obtain policy and charging rules for the PDU Session.

The SMF establishes a policy association with PCF using the N7 interface.

Interface:
N7 (SMF - PCF)

Service:

Npcf_SMPolicyControl

Message:
SMPolicyControl Create Request

The SMF provides information including:

- SUPI
- PDU Session ID
- DNN
- S-NSSAI
- User location information
- Access type
- Session information

---

The PCF evaluates subscriber and network policies and provides the SMF with:

- QoS rules
- QoS parameters
- PCC (Policy and Charging Control) rules
- Charging information
- Traffic control policies

The SMF uses these policies to configure the required QoS flows and user-plane forwarding rules.

---

## 6. SMF Selects UPF

After receiving subscriber information and policy rules, the SMF selects the appropriate User Plane Function (UPF) for the PDU Session.

UPF selection is based on:

- DNN
- S-NSSAI
- UE location
- UPF capacity
- Network topology
- Operator configuration
- Local routing policies

The selected UPF provides connectivity between the UE and the Data Network (DN).

User-plane path:
UE
|
gNB
|
N3 (GTP-U)
|
UPF
|
N6
|
Data Network

---

The SMF provides the UPF selection information and prepares the user-plane session establishment.

The SMF is responsible for controlling:

- UPF forwarding behavior
- QoS enforcement
- Traffic detection rules
- Usage reporting

---

## 7. SMF Establishes PFCP Session with UPF

After selecting the appropriate UPF, the SMF establishes the user-plane session by creating a PFCP session between SMF and UPF.

The communication between SMF and UPF is performed over the N4 interface.

Interface:

N4 (SMF - UPF)

Protocol:
PFCP (Packet Forwarding Control Protocol)

---

## PFCP Session Establishment Request

The SMF sends a:
PFCP Session Establishment Request

to the selected UPF.

The request contains the forwarding and QoS rules required for packet processing.

The main rules included are:

### Packet Detection Rules (PDR)

PDR defines how the UPF identifies incoming packets.

PDR contains:

- Source Interface
- Packet matching criteria
- UE IP address
- Tunnel Endpoint Identifier (TEID)
- SDF filters

Example:
Detect packets from gNB N3 tunnel

---

### Forwarding Action Rules (FAR)

FAR defines what action UPF should perform after packet detection.

Actions include:

- Forward packet
- Drop packet
- Buffer packet

Example:
Forward downlink traffic towards gNB

---

### QoS Enforcement Rules (QER)

QER defines QoS treatment for user traffic.

Parameters include:

- QoS Flow Identifier (QFI)
- Guaranteed Flow Bit Rate (GFBR)
- Maximum Flow Bit Rate (MFBR)
- Priority level

---

### Usage Reporting Rules (URR)

URR is used for:

- Usage monitoring
- Charging information
- Traffic volume reporting

---

## UPF Processes PFCP Request

After receiving the PFCP Session Establishment Request, the UPF:

- Creates PFCP session context
- Installs PDR/FAR/QER/URR rules
- Allocates user-plane resources
- Creates GTP-U tunnel parameters

The UPF responds with:
PFCP Session Establishment Response

---

The response contains:

- Cause value
- UPF F-TEID information
- N3 tunnel endpoint information
- Rule installation status

Example:
Cause = Request accepted

---

## PFCP Session Result

After successful PFCP establishment:
SMF
|
N4 PFCP
|
UPF

The UPF is now ready to forward user traffic according to the configured rules.

The next step is establishing the radio QoS resources and completing the N3 user-plane tunnel between gNB and UPF.

---

## 8. gNB Establishes QoS Flow and Data Radio Bearer

After successful PFCP session establishment between SMF and UPF, the SMF provides the required session information to the gNB through the AMF.

The purpose of this step is to establish the radio resources required for user-plane data transmission.

---

## SMF Sends N2 Session Management Information

The SMF sends PDU Session resource information to the AMF over the N11 interface.

The AMF forwards this information to the gNB using the N2 interface.

The signaling path is:
SMF
|
N11
|
AMF
|
N2
|
gNB

The information contains:

- PDU Session ID
- S-NSSAI
- QoS Flow information
- QoS parameters
- UPF tunnel information
- Security information

---

## gNB Performs PDU Session Resource Setup

The gNB receives the PDU Session Resource Setup Request from the AMF.

Interface:
N2 (AMF - gNB)

Protocol:
NGAP (Next Generation Application Protocol)

Message:

PDU Session Resource Setup Request

The gNB performs:

- QoS Flow mapping
- QoS Flow to DRB mapping
- Radio resource allocation
- Data Radio Bearer establishment

---

## QoS Flow Establishment

In 5G, QoS is managed using QoS Flows.

Each QoS Flow is identified by:
QFI (QoS Flow Identifier)

The QoS Flow defines:

- 5QI
- Priority level
- Packet delay budget
- Packet error rate
- Guaranteed bit rate (if applicable)

The gNB maps the QoS Flow to a Data Radio Bearer (DRB).
QoS Flow
|
|
v
Data Radio Bearer (DRB)

---

## N3 User Plane Tunnel Activation

After DRB establishment, the user-plane tunnel between gNB and UPF becomes active.

Interface:
N3 (gNB - UPF)

Protocol:
GTP-U

The tunnel uses:

- GTP-U TEID
- Source IP address
- Destination IP address

User-plane path:
UE
|
Radio
|
gNB
|
N3 GTP-U Tunnel
|
UPF
|
N6
|
Data Network

---

## gNB Response to AMF

After successful resource establishment, the gNB sends:
PDU Session Resource Setup Response

towards the AMF.

The response contains:

- Successfully established PDU Session ID
- GTP-U tunnel information
- QoS Flow status

The AMF forwards the response information to the SMF.

---

After successful completion of this step:

- Radio resources are available.
- QoS flows are configured.
- N3 user-plane connectivity is established.

The next step is sending the final NAS message:

**PDU Session Establishment Accept**

