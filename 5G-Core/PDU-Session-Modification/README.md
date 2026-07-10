
# 5G PDU Session Modification Procedure

## Overview

The **5G PDU Session Modification Procedure** is used to update an existing PDU Session without releasing and re-establishing it. During this procedure, the network modifies session parameters such as QoS flows, forwarding rules, traffic filters, user plane paths, or policy information while maintaining uninterrupted connectivity.

PDU Session Modification may be initiated by either the **UE** or the **network**, depending on the reason for the update. Common triggers include QoS changes, policy updates from the PCF, application requirements, mobility events, or network optimization.

## Purpose

- Modify an existing PDU Session
- Add, update, or remove QoS Flows
- Update packet forwarding rules in the UPF
- Apply new policy and charging rules
- Modify traffic filters
- Update user plane resources without interrupting the session

## Network Functions Involved

- User Equipment (UE)
- gNodeB (gNB)
- Access and Mobility Management Function (AMF)
- Session Management Function (SMF)
- User Plane Function (UPF)
- Policy Control Function (PCF) *(when policy changes trigger the modification)*

## Step-by-Step Procedure

### Step 1: Modification Trigger

A PDU Session Modification may be initiated by either the UE or the network.

Common triggers include:

- QoS Flow addition or removal
- Policy update from the PCF
- Application bandwidth changes
- Mobility-related user plane updates
- Network optimization
- Charging policy updates

The SMF is responsible for coordinating the modification procedure.

---

### Step 2: Modification Request

Depending on the trigger, the modification request is initiated by the UE or the network.

The AMF receives the request and forwards it to the SMF for processing.

**Messages**

- PDU Session Modification Request (NAS) *(if UE initiated)*
- Nsmf_PDUSession_UpdateSMContext Request

**Interfaces**

- Uu (UE ↔ gNodeB)
- N2 (gNodeB ↔ AMF)
- N11 (AMF ↔ SMF)

---

### Step 3: SMF Evaluates the Requested Changes

The SMF validates the requested modification and determines whether changes to the user plane or QoS are required.

If the modification is policy-driven, the SMF may interact with the PCF to obtain updated policy and charging rules.

Possible modifications include:

- Adding or removing QoS Flows
- Updating QoS parameters
- Modifying packet forwarding rules
- Updating charging rules
- Updating traffic filters

---

### Step 4: SMF Updates the UPF

If user plane changes are required, the SMF updates the UPF using PFCP signaling.

The UPF applies the new forwarding rules, QoS enforcement, and packet detection rules while maintaining the existing PDU Session.

**Messages**

- PFCP Session Modification Request
- PFCP Session Modification Response

**Interfaces**

- N4 (SMF ↔ UPF)

---

### Step 5: AMF Updates the gNodeB (If Required)

If the modification affects radio resources or QoS handling, the AMF instructs the gNodeB to update the UE context.

The gNodeB configures the required radio bearer or QoS changes.

This step is only required when radio access network resources need to be modified.

**Interfaces**

- N2 (AMF ↔ gNodeB)

---

### Step 6: Modification Complete

After all network functions successfully apply the requested changes, the SMF confirms completion to the AMF.

If the UE initiated the request, the network returns a **PDU Session Modification Command** and the UE responds with **PDU Session Modification Complete**.

The PDU Session remains active throughout the procedure.

**Result**

- Existing PDU Session retained
- QoS Flows updated if required
- PFCP session updated
- User plane continues without interruption
- Policy and charging rules synchronized

  ## Common PDU Session Modifications

The following changes may be performed during a PDU Session Modification procedure:

| Modification | Description |
|--------------|-------------|
| QoS Flow Addition | Add a new QoS Flow for a service or application |
| QoS Flow Removal | Remove an existing QoS Flow that is no longer required |
| QoS Parameter Update | Modify 5QI, ARP, GBR, or MBR values |
| Traffic Filter Update | Add, remove, or modify packet filters |
| Forwarding Rule Update | Update Packet Detection Rules (PDRs), FARs, or QERs in the UPF |
| Charging Rule Update | Apply new charging policies |
| Policy Update | Apply updated policy decisions received from the PCF |
| User Plane Path Update | Modify forwarding behavior without creating a new PDU Session |

## Message Summary

| Step | Sender | Receiver | Message | Purpose |
|------|---------|----------|---------|---------|
| 1 | UE / Network | AMF | PDU Session Modification Trigger | Initiate modification of an existing PDU Session |
| 2 | AMF | SMF | Nsmf_PDUSession_UpdateSMContext Request | Request modification of the PDU Session |
| 3 | SMF | PCF *(Optional)* | Policy Update Request | Retrieve updated policy and charging information |
| 4 | PCF | SMF *(Optional)* | Policy Update Response | Return updated policy decisions |
| 5 | SMF | UPF | PFCP Session Modification Request | Update forwarding rules and QoS information |
| 6 | UPF | SMF | PFCP Session Modification Response | Confirm successful PFCP update |
| 7 | SMF | AMF | Nsmf_PDUSession_UpdateSMContext Response | Confirm completion of the modification |
| 8 | AMF | gNodeB | NGAP UE Context Modification Request *(If Required)* | Update radio resources or QoS configuration |
| 9 | gNodeB | AMF | NGAP UE Context Modification Response | Confirm successful UE context update |
| 10 | AMF | UE | PDU Session Modification Command *(If UE Involved)* | Inform the UE of the updated session parameters |
| 11 | UE | AMF | PDU Session Modification Complete | Confirm successful modification |

## Interface Summary

| Interface | Network Functions | Protocol | Purpose |
|-----------|-------------------|----------|---------|
| Uu | UE ↔ gNodeB | NAS / RRC | Transport modification signaling between the UE and network |
| N2 | gNodeB ↔ AMF | NGAP | Carry UE context modification messages |
| N11 | AMF ↔ SMF | HTTP/2 (SBI) | Coordinate PDU Session updates |
| N4 | SMF ↔ UPF | PFCP | Modify forwarding rules, QoS, and user plane state |
| N7 *(Optional)* | SMF ↔ PCF | HTTP/2 (SBI) | Retrieve updated policy and charging decisions |
| N3 | gNodeB ↔ UPF | GTP-U | Continue carrying user plane traffic during and after modification |

## Key Takeaways

- PDU Session Modification updates an **existing** PDU Session without interrupting user connectivity.
- No new PDU Session is created, and the existing PDU Session ID remains unchanged.
- The SMF is responsible for coordinating all session modifications.
- The UPF applies updated forwarding rules through **PFCP Session Modification** procedures.
- The PCF may participate when policy or charging rules need to be updated.
- Radio resource updates at the gNodeB are only required if the modification impacts QoS or bearer configuration.
- The GTP-U tunnel remains active, allowing user traffic to continue during most modifications.
- Common modification scenarios include QoS Flow changes, policy updates, traffic filter updates, and forwarding rule updates.
- This procedure enables dynamic service adaptation without requiring PDU Session Release and re-establishment.
- PDU Session Modification is frequently used for applications with changing bandwidth or latency requirements, such as video streaming, VoNR, gaming, and enterprise services.

  
