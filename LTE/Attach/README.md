# LTE Attach Procedure

## Overview

The LTE Attach procedure is the first signaling procedure executed when a User Equipment (UE) connects to an LTE network.

During this procedure, the network authenticates the subscriber, establishes mobility management context, creates the default EPS bearer, and allocates an IP address for data connectivity.

The Attach procedure enables the UE to access LTE packet services.

---

## Objectives

The LTE Attach procedure performs the following functions:

- Authenticate the subscriber
- Register the UE with the EPC
- Create the default EPS bearer
- Allocate an IP address
- Establish user plane connectivity
- Enable packet data services

---

## Network Elements

The following network elements participate in the LTE Attach procedure:

- UE (User Equipment)
- eNodeB
- MME
- HSS
- SGW
- PGW
- PCRF (optional depending on deployment)

---

## LTE Attach Signaling Flow

![LTE Attach Procedure](images/lte-attach.png)


## Summary

The LTE Attach procedure establishes the UE's connection to the Evolved Packet Core (EPC), enabling access to LTE packet data services. During this procedure, the subscriber is authenticated, security is activated, the UE is registered with the Mobility Management Entity (MME), a default EPS bearer is created, and an IP address is assigned by the Packet Data Network Gateway (PGW).

Upon successful completion of the Attach procedure, the UE is ready to exchange user-plane traffic and access network services.

## Message Summary

| Step | Message | From | To | Purpose |
|------|---------|------|----|---------|
| 1 | Attach Request | UE | eNodeB | Initiates LTE network registration |
| 2 | Initial UE Message | eNodeB | MME | Forwards the NAS Attach Request |
| 3 | Authentication Information Request/Answer | MME | HSS | Retrieves authentication vectors |
| 4 | Authentication Request/Response | MME | UE | Authenticates the subscriber |
| 5 | Security Mode Command/Complete | MME | UE | Activates NAS security |
| 6 | Update Location Request/Answer | MME | HSS | Updates subscriber location |
| 7 | Create Session Request/Response | MME | SGW/PGW | Creates the default EPS bearer and allocates an IP address |
| 8 | Attach Accept | MME | UE | Confirms successful attachment |
| 9 | Attach Complete | UE | MME | Completes the Attach procedure |

## Detailed Signaling

### 1. Attach Request (UE → eNodeB)

The LTE Attach procedure begins when the UE sends an **Attach Request** NAS message to the eNodeB. This message indicates that the UE wants to register with the EPC and access packet data services.

The Attach Request typically includes:
- IMSI or GUTI
- UE Network Capability
- EPS Attach Type
- Last Visited TAI (if available)
- PDN Connectivity Request

The eNodeB does not process the NAS message. Instead, it encapsulates the message within an **Initial UE Message** over the S1-AP interface and forwards it to the MME.




Detailed signaling procedures, sequence diagrams, troubleshooting scenarios, and references will be added in future updates.
