
# LTE Paging Procedure

## Overview

The LTE Paging procedure is used to notify a User Equipment (UE) in **ECM-IDLE** state that the network has pending downlink data or signaling. Since the UE does not have an active S1 connection, the Mobility Management Entity (MME) instructs one or more eNodeBs to transmit a Paging message over the radio interface.

After receiving the Paging message, the UE initiates the **LTE Service Request** procedure to resume radio and core network connectivity.

Paging enables efficient battery usage by allowing the UE to remain idle while still being reachable for mobile-terminated services such as VoLTE calls, SMS delivery, and packet data.

---

## Purpose

The LTE Paging procedure allows the network to:

* Notify an idle UE of an incoming VoLTE call.
* Notify an idle UE of an incoming SMS.
* Notify an idle UE of pending downlink packet data.
* Trigger mobile-terminated signaling procedures.
* Resume communication by initiating the LTE Service Request procedure.

---

## Preconditions

Before Paging can occur:

* The UE has successfully completed the LTE Attach procedure.
* The UE is registered with the MME.
* The UE is in **ECM-IDLE** state.
* The MME knows the UE's current Tracking Area (TA) or Tracking Area List (TAL).
* The serving eNodeB(s) are operational.

---

## Network Elements Involved

* User Equipment (UE)
* eNodeB (eNB)
* Mobility Management Entity (MME)
* Serving Gateway (SGW)
* PDN Gateway (PGW)
* IMS Core (for VoLTE mobile-terminated calls)

---

## Call Flow

> *(Paging call flow diagram will be added in the next step.)*

---

## Message Sequence

The detailed explanation of each signaling message will be added in the following sections.

---

## Common Paging Triggers

The network may initiate Paging for several reasons, including:

* Incoming VoLTE call
* Mobile-terminated SMS
* Downlink user data
* NAS signaling
* Emergency notifications
* IMS registration updates

---

## Troubleshooting

Common issues covered in this section include:

* UE does not receive Paging.
* Paging retransmissions.
* Incorrect Tracking Area List (TAL).
* MME paging failures.
* eNodeB paging failures.
* Paging congestion.
* Service Request not triggered after Paging.

---

## References

* 3GPP TS 23.401 – General Packet Radio Service (GPRS) enhancements for E-UTRAN access
* 3GPP TS 24.301 – Non-Access-Stratum (NAS) protocol for EPS
* 3GPP TS 36.300 – E-UTRAN overall description
* 3GPP TS 36.304 – User Equipment (UE) procedures in idle mode
* 3GPP TS 36.413 – S1 Application Protocol (S1AP)

---

## Related Procedures

* [LTE Attach](../Attach/README.md)
* [Tracking Area Update (TAU)](../TAU/README.md)
* [LTE Service Request](../Service-Request/README.md)
* LTE Detach *(Coming Soon)*

