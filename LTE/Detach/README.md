
# LTE Detach Procedure

## Overview

The LTE Detach procedure is used to deregister a User Equipment (UE) from the Evolved Packet System (EPS). It allows the UE or the network to gracefully terminate the UE's EPS registration, release allocated radio and core network resources, and remove the UE context from the Mobility Management Entity (MME).

A Detach procedure may be initiated by either the UE or the network for various reasons, such as the user powering off the device, disabling LTE services, removing the SIM card, or due to operator-initiated actions. During the procedure, the MME coordinates with the eNodeB and Serving Gateway (SGW) to release radio bearers, delete EPS bearer contexts, and terminate the UE's active session.

The successful completion of the Detach procedure ensures that network resources are efficiently released and that the UE is no longer reachable for mobile-terminated services until it performs a new LTE Attach procedure.

---

## Purpose

The LTE Detach procedure allows the network to:

* Deregister the UE from the EPS network.
* Release radio resources allocated to the UE.
* Remove the UE context from the MME.
* Delete active EPS bearer contexts.
* Release user-plane resources in the SGW and PGW.
* Prevent further paging of the detached UE.

---

## Preconditions

Before the Detach procedure can occur:

* The UE has successfully completed the LTE Attach procedure.
* The UE is registered with the MME.
* At least one active EPS bearer exists.
* The UE may be in **ECM-CONNECTED** or **ECM-IDLE** state.
* The MME maintains a valid UE context.

---

## Network Elements Involved

* User Equipment (UE)
* eNodeB (eNB)
* Mobility Management Entity (MME)
* Serving Gateway (SGW)
* PDN Gateway (PGW)
* Home Subscriber Server (HSS)

---

## Call Flow

> *(Detach call flow diagram will be added in the next step.)*

---

## Detach Triggers

The LTE Detach procedure may be initiated for several reasons, including:

* UE power off.
* User disables LTE or mobile network services.
* SIM card removal.
* Subscription termination by the operator.
* Network-initiated detach due to authentication or security failures.
* Administrative or maintenance operations.
* UE moving to a non-EPS network without EPS service continuity.

---

## Message Sequence

The detailed explanation of each signaling message will be added in the following sections.
