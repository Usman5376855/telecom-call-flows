
# LTE Detach Procedure

## Overview
> **Note:** This document describes the most common **UE-Initiated EPS Detach** procedure while the UE is in **ECM-CONNECTED** state. The exact signaling sequence may vary depending on the detach type and the UE's ECM state.

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

---
## 1. NAS Detach Request (UE → MME)

The LTE Detach procedure begins when the User Equipment (UE) decides to deregister from the Evolved Packet System (EPS) or when the network requires the UE to terminate its EPS registration. In a UE-initiated detach, the UE sends a **NAS Detach Request** message to the Mobility Management Entity (MME) through the serving eNodeB.

The NAS Detach Request informs the MME that the UE intends to terminate its EPS registration and release all associated network resources. Depending on the detach type, the request may indicate that only EPS services are being detached or that both EPS and IMSI registrations should be removed.

The NAS Detach Request message typically contains:

* EPS Detach Type
* EPS Mobile Identity (GUTI or IMSI)
* NAS Security Header
* Detach Reason (when applicable)

**Purpose**

* Inform the MME that the UE wishes to detach from the EPS network.
* Trigger the release of radio and core network resources.
* Initiate the deletion of active EPS bearer contexts.
* Deregister the UE from the LTE network.

**Key Point**

The NAS Detach Request is a **NAS EMM (EPS Mobility Management)** message protected by NAS security. It is transparently forwarded by the eNodeB to the MME without modification.

**Common Troubleshooting**

* NAS Detach Request not received by the MME.
* Invalid or missing NAS security information.
* Unknown or invalid GUTI/IMSI.
* UE context not found in the MME.
* NAS integrity verification failure.
* UE powers off before transmitting the Detach Request.

```
## 2. Initial UE Message (eNodeB → MME)

After receiving the **NAS Detach Request** from the UE, the eNodeB encapsulates the NAS message within an **S1AP Initial UE Message** and forwards it to the Mobility Management Entity (MME) over the **S1-MME interface**.

The Initial UE Message establishes the signaling association between the eNodeB and the MME for the Detach procedure. Along with the NAS Detach Request, the eNodeB includes information identifying the UE's serving cell and tracking area, allowing the MME to correctly identify the UE context and continue processing the detach request.

The Initial UE Message typically contains:

* NAS Detach Request
* eNB UE S1AP ID
* TAI (Tracking Area Identity)
* ECGI (E-UTRAN Cell Global Identifier)
* S-TMSI (when available)

**Purpose**

* Forward the NAS Detach Request to the MME.
* Associate the UE with the serving eNodeB.
* Allow the MME to begin processing the Detach procedure.
* Provide the UE's current location information.

**Key Point**

The eNodeB does not interpret or modify the NAS Detach Request. It simply encapsulates the NAS message within the **S1AP Initial UE Message** and forwards it to the MME for further processing.

**Common Troubleshooting**

* Initial UE Message not received by the MME.
* Incorrect TAI or ECGI information.
* S1-MME interface communication failure.
* UE context mismatch between the eNodeB and MME.
* S1AP message decoding failure.
* eNB UE S1AP ID not matching the existing UE context.

---

## 3. Delete Session Request (MME → SGW)

After successfully processing the NAS Detach Request, the MME initiates the release of the UE's EPS bearer resources by sending a **GTPv2-C Delete Session Request** to the Serving Gateway (SGW) over the **S11 interface**.

The Delete Session Request instructs the SGW to remove the UE's bearer context and terminate the active PDN session. The SGW subsequently releases the user-plane resources and coordinates with the PDN Gateway (PGW), if required, to complete the session deletion.

The Delete Session Request message typically contains:

* EPS Bearer ID (EBI)
* Linked EPS Bearer ID
* Sender F-TEID for Control Plane
* Indication Flags (when applicable)
* User Location Information (optional)

**Purpose**

* Delete the UE's active EPS bearer(s).
* Release user-plane resources in the SGW.
* Terminate the active PDN session.
* Begin cleanup of the UE's core network context.

**Key Point**

The **Delete Session Request** is a **GTPv2-C** message exchanged between the MME and SGW. It is responsible for releasing the bearer context and stopping packet forwarding for the detached UE.

**Common Troubleshooting**

* Delete Session Request not received by the SGW.
* S11 interface communication failure.
* Invalid TEID in the Delete Session Request.
* SGW unable to locate the bearer context.
* GTPv2-C timeout between the MME and SGW.
* Session deletion rejected due to inconsistent bearer information.

---

## 4. Delete Session Response (SGW → MME)

After successfully deleting the UE's bearer context and releasing the associated user-plane resources, the Serving Gateway (SGW) returns a **GTPv2-C Delete Session Response** to the MME.

The response confirms that the bearer context has been removed and that the SGW has completed its portion of the session release procedure. Upon receiving this confirmation, the MME proceeds with releasing the remaining signaling resources associated with the UE.

The Delete Session Response message typically contains:

* Cause Value
* Recovery Information (when applicable)
* Protocol Configuration Options (optional)

**Purpose**

* Confirm successful deletion of the EPS bearer context.
* Notify the MME that user-plane resources have been released.
* Allow the Detach procedure to continue.

**Key Point**

Once the MME receives a successful **Delete Session Response**, the UE's packet data session is considered terminated, and no further user-plane traffic can be exchanged until a new LTE Attach procedure is performed.

**Common Troubleshooting**

* Delete Session Response timeout.
* GTP Cause indicating unsuccessful session deletion.
* SGW internal processing failure.
* Bearer context not completely released.
* User-plane tunnel remaining active after session deletion.
* S11 retransmissions caused by packet loss.
  
---

## 5. UE Context Release Command (MME → eNodeB)

After successfully releasing the UE's EPS bearer context, the MME initiates the release of the UE's radio and signaling resources by sending an **S1AP UE Context Release Command** to the serving eNodeB.

The UE Context Release Command instructs the eNodeB to release all radio resources allocated to the UE, terminate the S1 signaling connection, and prepare for removal of the UE context. This ensures that both the radio access network and the core network maintain a consistent view of the UE's detached state.

The UE Context Release Command message typically contains:

* MME UE S1AP ID
* eNB UE S1AP ID
* Cause Value
* UE Context Release Cause

**Purpose**

* Release radio resources allocated to the UE.
* Remove the S1 signaling connection.
* Synchronize UE context release between the MME and eNodeB.
* Continue the LTE Detach procedure.

**Key Point**

The UE Context Release Command only releases the radio and signaling resources. The EPS bearer context has already been removed through the **Delete Session Request/Response** procedure.

**Common Troubleshooting**

* UE Context Release Command not received by the eNodeB.
* S1AP communication failure.
* Invalid UE S1AP identifiers.
* Radio resources not released successfully.
* eNodeB unable to remove the UE context.
* S1 interface timeout.

---

## 6. UE Context Release Complete (eNodeB → MME)

After successfully releasing the UE's radio resources and deleting the associated UE context, the eNodeB responds with an **S1AP UE Context Release Complete** message to the MME.

This message confirms that the requested radio resource release has been completed successfully and that the UE context has been removed from the eNodeB. Upon receiving this confirmation, the MME proceeds with the final stages of the Detach procedure.

The UE Context Release Complete message typically contains:

* MME UE S1AP ID
* eNB UE S1AP ID

**Purpose**

* Confirm successful release of radio resources.
* Notify the MME that the UE context has been removed.
* Complete the S1 context release procedure.

**Key Point**

After the MME receives the UE Context Release Complete message, there is no active S1 signaling connection remaining between the UE and the network.

**Common Troubleshooting**

* UE Context Release Complete not received by the MME.
* S1AP timeout.
* Context mismatch between the eNodeB and MME.
* Incomplete removal of the UE context.
* S1 interface communication failure.

---

## 7. NAS Detach Accept (MME → UE)

After successfully completing the required resource release procedures, the MME sends a **NAS Detach Accept** message to acknowledge the UE's detach request.

The NAS Detach Accept informs the UE that the network has successfully processed the Detach procedure and that the UE is no longer registered with the EPS network. Upon receiving this message, the UE removes its locally stored EPS registration information and considers the Detach procedure complete.

The NAS Detach Accept message typically contains:

* NAS Security Header
* Detach Accept Information

**Purpose**

* Confirm successful completion of the LTE Detach procedure.
* Notify the UE that EPS registration has been removed.
* Complete the NAS signaling exchange.

**Key Point**

For some detach scenarios, particularly when the UE powers off immediately after transmitting the Detach Request, the NAS Detach Accept may not be delivered or may not be required. This behavior is defined by 3GPP and depends on the detach type and UE state.

**Common Troubleshooting**

* NAS Detach Accept not received by the UE.
* NAS security verification failure.
* UE powers off before receiving the response.
* Downlink NAS delivery failure.
* Radio link failure during Detach completion.

---

## 8. UE Context Deletion and Procedure Complete

After all signaling messages have been exchanged, the MME removes the remaining UE context from its internal database and marks the subscriber as detached from the EPS network.

The SGW has already released the user-plane resources, the eNodeB has removed the radio context, and the UE is no longer registered with the LTE network. Any future communication requires the UE to perform a new **LTE Attach** procedure.

**Purpose**

* Remove all remaining UE context information.
* Complete the LTE Detach procedure.
* Ensure network resources are fully released.
* Prepare the UE for a future LTE Attach.

**Key Point**

Once the Detach procedure is complete, the UE is no longer reachable for Paging, Service Request, VoLTE calls, SMS, or packet data until it successfully performs a new LTE Attach procedure.

**Common Troubleshooting**

* Stale UE context remaining in the MME.
* Incomplete session cleanup.
* Bearer context not fully removed.
* Subscriber still marked as attached.
* Resource leakage following abnormal Detach.

---

## Common Failure Scenarios

Common LTE Detach issues include:

* UE powers off before Detach completion.
* NAS Detach Request not received by the MME.
* Delete Session Request timeout.
* GTP-C communication failure on the S11 interface.
* UE Context Release failure.
* Stale UE context in the MME.
* NAS security verification failure.
* Incomplete EPS bearer cleanup.

---

## Troubleshooting

Common troubleshooting steps include:

* Verify NAS signaling between the UE and MME.
* Check S1AP messages on the S1-MME interface.
* Verify GTPv2-C Delete Session procedures on the S11 interface.
* Confirm successful UE Context Release.
* Check MME logs for detach cause values.
* Verify SGW bearer deletion.
* Monitor NAS and S1AP timers.
* Ensure UE context has been removed successfully.

---

## References

* 3GPP TS 23.401
* 3GPP TS 24.301
* 3GPP TS 29.274
* 3GPP TS 36.300
* 3GPP TS 36.413

---

## Related Procedures

* [LTE Attach](../Attach/README.md)
* [Tracking Area Update (TAU)](../TAU/README.md)
* [LTE Service Request](../Service-Request/README.md)
* [LTE Paging](../Paging/README.md)


