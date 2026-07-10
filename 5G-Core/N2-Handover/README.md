
# 5G N2 Handover Procedure

## Overview

The **5G N2 Handover Procedure** is an AMF-assisted mobility procedure used when a User Equipment (UE) moves from one gNodeB to another and direct Xn-based handover cannot be performed. Unlike Xn Handover, the source gNodeB does not communicate directly with the target gNodeB to prepare the handover. Instead, the **Access and Mobility Management Function (AMF)** coordinates the handover using the N2 interface.

During the procedure, the AMF coordinates signaling between the source gNodeB, target gNodeB, Session Management Function (SMF), and User Plane Function (UPF). Once the UE successfully connects to the target gNodeB, the user plane path is updated so that traffic is forwarded through the new serving cell.

## Purpose

- Maintain service continuity during mobility
- Support handover when Xn connectivity is unavailable
- Coordinate mobility through the AMF
- Preserve active PDU Sessions and QoS Flows
- Redirect the user plane to the target gNodeB

## Purpose

- Maintain service continuity during mobility
- Support handover when Xn connectivity is unavailable
- Coordinate mobility through the AMF
- Preserve active PDU Sessions and QoS Flows
- Redirect the user plane to the target gNodeB

## Network Functions Involved

- User Equipment (UE)
- Source gNodeB
- Target gNodeB
- Access and Mobility Management Function (AMF)
- Session Management Function (SMF)
- User Plane Function (UPF)

## Step-by-Step Procedure

### Step 1: UE Sends Measurement Report

The UE continuously measures the signal quality of the serving cell and neighboring cells. When the configured measurement event (such as Event A3) is satisfied, the UE sends a **Measurement Report** to the source gNodeB.

The report typically contains:

- RSRP
- RSRQ
- SINR
- Neighbor Cell Information

**Messages**

- Measurement Report

**Interfaces**

- Uu (UE ↔ Source gNodeB)

---

### Step 2: Source gNodeB Decides to Initiate Handover

The source gNodeB analyzes the measurement report together with mobility policies, neighbor relations, and radio conditions.

If Xn-based handover cannot be performed (for example, because Xn connectivity is unavailable), the source gNodeB requests the AMF to coordinate an N2 Handover.

---

### Step 3: Source gNodeB Sends Handover Required

The source gNodeB sends an **NGAP Handover Required** message to the AMF.

The message contains:

- Target Cell Identifier
- UE Context
- PDU Session Information
- QoS Flow Information
- Handover Cause

**Messages**

- NGAP Handover Required

**Interfaces**

- N2 (Source gNodeB ↔ AMF)

---

### Step 4: AMF Sends Handover Request

The AMF selects the target gNodeB and sends an **NGAP Handover Request**.

The request includes:

- UE Context
- Security Context
- Allowed NSSAI
- PDU Session Information
- QoS Flow Information
- Security Parameters

**Messages**

- NGAP Handover Request

**Interfaces**

- N2 (AMF ↔ Target gNodeB)

---

### Step 5: Target gNodeB Prepares Resources

The target gNodeB allocates the required radio resources and prepares the UE context.

If successful, it returns an **NGAP Handover Request Acknowledge** to the AMF.

**Messages**

- NGAP Handover Request Acknowledge

**Interfaces**

- N2 (Target gNodeB ↔ AMF)

---

### Step 6: AMF Sends Handover Command

The AMF forwards the handover information to the source gNodeB.

The source gNodeB then delivers an **RRC Reconfiguration** message (handover command) to the UE.

**Messages**

- NGAP Handover Command
- RRC Reconfiguration

**Interfaces**

- N2 (AMF ↔ Source gNodeB)
- Uu (UE ↔ Source gNodeB)

---

### Step 7: UE Accesses the Target gNodeB

The UE disconnects from the source cell and synchronizes with the target cell.

After completing the Random Access Procedure, the UE sends an **RRC Reconfiguration Complete** message.

**Messages**

- Random Access Procedure
- RRC Reconfiguration Complete

**Interfaces**

- Uu (UE ↔ Target gNodeB)

---

### Step 8: Target gNodeB Notifies the AMF

After successfully receiving the UE, the target gNodeB sends an **NGAP Handover Notify** message to the AMF.

This informs the AMF that the UE has successfully attached to the target cell.

**Messages**

- NGAP Handover Notify

**Interfaces**

- N2 (Target gNodeB ↔ AMF)

---

### Step 9: Path Switch Procedure

The AMF informs the SMF that the serving gNodeB has changed.

The SMF updates the UPF by modifying the PFCP session so that GTP-U traffic is redirected to the target gNodeB.

**Messages**

- Nsmf_PDUSession_UpdateSMContext Request
- Nsmf_PDUSession_UpdateSMContext Response
- PFCP Session Modification Request
- PFCP Session Modification Response

**Interfaces**

- N11 (AMF ↔ SMF)
- N4 (SMF ↔ UPF)

---

### Step 10: Source gNodeB Releases Resources

After the user plane has been successfully switched, the AMF instructs the source gNodeB to release the old UE context.

The source gNodeB releases:

- Radio resources
- UE Context
- Temporary forwarding resources
- Bearer information associated with the old cell

**Messages**

- NGAP UE Context Release Command
- NGAP UE Context Release Complete

**Interfaces**

- N2 (AMF ↔ Source gNodeB)

---

### Procedure Complete

The UE is now fully served by the target gNodeB.

**Result**

- UE successfully handed over
- Existing PDU Sessions preserved
- QoS Flows maintained
- User Plane switched to the target gNodeB
- Source gNodeB resources released
- Service continuity maintained

## When is N2 Handover Used?

N2 Handover is typically selected in the following scenarios:

- The source and target gNodeBs do not have Xn connectivity.
- The Xn interface is unavailable due to transmission or network failures.
- The source and target gNodeBs belong to different Xn connectivity domains.
- Network deployment or vendor interoperability requires AMF-assisted mobility.
- Operator policies require the Core Network to coordinate the handover.
- Direct gNodeB-to-gNodeB signaling is not supported.

Although N2 Handover involves additional signaling through the AMF, it ensures seamless mobility when direct Xn-based handover cannot be performed.

## Prerequisites for N2 Handover

Before an N2 Handover can be performed, the following conditions should be satisfied:

| Requirement | Description |
|------------|-------------|
| Active RRC Connection | The UE must be in **RRC_CONNECTED** state. |
| Neighbor Cell Configuration | The target cell must be configured as a valid neighbor. |
| Measurement Configuration | Measurement events (such as A3) must be configured. |
| Active PDU Session | At least one PDU Session should be active. |
| Valid Security Context | The UE security context must be available for transfer. |
| AMF Reachability | The source and target gNodeBs must have N2 connectivity with the AMF. |
| Available Radio Resources | The target gNodeB must have sufficient resources to admit the UE. |
| SMF and UPF Reachability | User plane path updates must be possible after handover. |

## Message Summary

| Step | Sender | Receiver | Message | Purpose |
|------|---------|----------|---------|---------|
| 1 | UE | Source gNodeB | Measurement Report | Report neighboring cell measurements |
| 2 | Source gNodeB | AMF | NGAP Handover Required | Request AMF-assisted handover |
| 3 | AMF | Target gNodeB | NGAP Handover Request | Prepare resources at the target cell |
| 4 | Target gNodeB | AMF | NGAP Handover Request Acknowledge | Confirm successful resource allocation |
| 5 | AMF | Source gNodeB | NGAP Handover Command | Deliver handover information |
| 6 | Source gNodeB | UE | RRC Reconfiguration | Command the UE to move to the target cell |
| 7 | UE | Target gNodeB | Random Access Procedure | Synchronize with the target cell |
| 8 | UE | Target gNodeB | RRC Reconfiguration Complete | Confirm successful attachment |
| 9 | Target gNodeB | AMF | NGAP Handover Notify | Notify successful UE arrival |
| 10 | AMF | SMF | Nsmf_PDUSession_UpdateSMContext | Update PDU Session routing |
| 11 | SMF | UPF | PFCP Session Modification Request | Redirect GTP-U traffic |
| 12 | UPF | SMF | PFCP Session Modification Response | Confirm user plane update |
| 13 | AMF | Source gNodeB | NGAP UE Context Release Command | Release source resources |

## Interface Summary

| Interface | Network Functions | Protocol | Purpose |
|-----------|-------------------|----------|---------|
| Uu | UE ↔ gNodeB | RRC | Radio signaling and handover execution |
| N2 | gNodeB ↔ AMF | NGAP | Coordinate the entire handover procedure |
| N11 | AMF ↔ SMF | HTTP/2 (SBI) | Update PDU Session context |
| N4 | SMF ↔ UPF | PFCP | Redirect the user plane path |
| N3 | gNodeB ↔ UPF | GTP-U | Carry user traffic after the path switch |

## Key Takeaways

- N2 Handover is an **AMF-assisted mobility procedure**.
- Unlike Xn Handover, the source and target gNodeBs do **not** coordinate handover preparation directly through the Xn interface.
- The AMF manages the signaling exchange between the source and target gNodeBs.
- Existing PDU Sessions remain active throughout the handover.
- The SMF updates the UPF using **PFCP Session Modification** to redirect user plane traffic.
- The GTP-U tunnel is switched to the target gNodeB after the handover is completed.
- N2 Handover introduces more Core Network signaling than Xn Handover but provides greater deployment flexibility.
- N2 Handover is commonly used when Xn connectivity is unavailable or direct gNodeB coordination is not possible.

  ## Xn Handover vs N2 Handover

| Feature | Xn Handover | N2 Handover |
|---------|-------------|-------------|
| Handover Coordination | Source and Target gNodeBs | AMF |
| Direct gNodeB Communication | Yes (Xn Interface) | No |
| Core Network Signaling | Lower | Higher |
| Handover Latency | Lower | Slightly Higher |
| Preferred Scenario | Xn connectivity available | Xn unavailable or unsupported |
| AMF Involvement | Mainly during Path Switch | Coordinates the complete handover |
| Complexity | Lower | Higher |
| Deployment Flexibility | Moderate | High |

## Troubleshooting

The following table lists common issues that may occur during the N2 Handover procedure, along with their possible causes and recommended troubleshooting actions.

| Issue | Possible Cause | Recommended Action |
|-------|----------------|--------------------|
| Handover not initiated | Measurement event not triggered or mobility thresholds incorrectly configured | Verify A3/A5 event configuration, neighbor relations, and Measurement Reports |
| NGAP Handover Required not received by the AMF | N2 connectivity issue, SCTP failure, or gNodeB configuration problem | Verify SCTP association, NGAP signaling, and AMF logs |
| Handover Request rejected by the target gNodeB | Insufficient radio resources, admission control failure, or invalid UE context | Check target gNodeB resource utilization and admission control logs |
| Random Access failure at the target gNodeB | Poor radio conditions or PRACH configuration issue | Verify radio coverage, PRACH configuration, and UE signal quality |
| Handover Notify missing | UE failed to attach successfully to the target gNodeB | Review RRC signaling and target gNodeB logs |
| Path Switch failure | N11 or N4 signaling failure between the AMF, SMF, and UPF | Verify NGAP, SBI, PFCP signaling, and UPF status |
| PFCP Session Modification failure | Invalid PFCP session or UPF connectivity issue | Verify N4 connectivity, PFCP session state, and UPF logs |
| User traffic interrupted after handover | GTP-U path not updated or forwarding rules not synchronized | Confirm successful Path Switch and verify GTP-U tunnel redirection |
| Source UE Context not released | NGAP UE Context Release procedure failed | Verify UE Context Release Command and Complete messages |

### Recommended Troubleshooting Flow

When troubleshooting an N2 Handover, verify the procedure in the following order:

1. Confirm that the UE transmitted valid Measurement Reports.
2. Verify that the source gNodeB sent the **NGAP Handover Required** message.
3. Confirm successful **NGAP Handover Request** and **Handover Request Acknowledge** signaling.
4. Verify that the UE completed the Random Access Procedure on the target gNodeB.
5. Confirm receipt of the **NGAP Handover Notify** message by the AMF.
6. Verify successful **Nsmf_PDUSession_UpdateSMContext** signaling.
7. Confirm successful **PFCP Session Modification** between the SMF and UPF.
8. Verify that the GTP-U tunnel has been redirected to the target gNodeB.
9. Confirm successful release of resources in the source gNodeB.

### KPIs to Monitor

The following KPIs are commonly monitored for N2 Handover performance:

- N2 Handover Success Rate
- N2 Handover Failure Rate
- Handover Preparation Success Rate
- Handover Execution Success Rate
- Random Access Success Rate
- Path Switch Success Rate
- PFCP Session Modification Success Rate
- Handover Interruption Time
- Packet Loss During Handover

  ## References

- 3GPP TS 23.502 – Procedures for the 5G System
- 3GPP TS 38.300 – NR Overall Description
- 3GPP TS 38.331 – NR Radio Resource Control (RRC)
- 3GPP TS 38.401 – NG-RAN Architecture Description
- 3GPP TS 38.413 – NG Application Protocol (NGAP)
- 3GPP TS 29.244 – Packet Forwarding Control Protocol (PFCP)
- 3GPP TS 29.502 – Session Management Services

---

## Related Procedures

### Previous Procedures

- [5G Xn Handover](../Xn-Handover/)
- [5G Registration Procedure](../Registration/)
- [5G PDU Session Establishment](../PDU-Session-Establishment/)
- [5G Service Request](../Service-Request/)
- [5G PDU Session Modification](../PDU-Session-Modification/)
- [5G PDU Session Release](../PDU-Session-Release/)

### Next Procedures

- Paging Procedure
- UE Context Release
- UE Deregistration
- Network-Initiated Deregistration
  

  
