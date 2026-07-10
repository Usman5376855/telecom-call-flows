
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
| NRF | Supports service discovery between Network Functions. |

---

## PDU Session Establishment Call Flow

![5G PDU Session Establishment](images/pdu-session-establishment-call-flow.png)

