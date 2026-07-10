# 5G Xn Handover Procedure

## Overview

The **5G Xn Handover Procedure** enables a User Equipment (UE) to move seamlessly between two gNodeBs connected through the **Xn interface** while maintaining active user sessions. The handover is performed without changing the serving AMF, allowing the UE to continue data transmission with minimal interruption.

During the procedure, the source gNodeB coordinates directly with the target gNodeB over the Xn interface. User context, security information, QoS parameters, and PDU Session information are transferred before the UE switches to the target cell.

## Purpose

- Maintain uninterrupted user connectivity during mobility
- Minimize packet loss and handover interruption time
- Transfer UE context between gNodeBs
- Preserve active PDU Sessions and QoS Flows
- Maintain ongoing voice and data services

## Network Functions Involved

- User Equipment (UE)
- Source gNodeB
- Target gNodeB
- Access and Mobility Management Function (AMF)
- Session Management Function (SMF)
- User Plane Function (UPF)

  ## Step-by-Step Procedure

### Step 1: UE Sends Measurement Report

The UE continuously measures the radio quality of the serving cell and neighboring cells. When predefined measurement events (such as A3) are met, the UE sends a **Measurement Report** to the source gNodeB.

The report contains information such as:

- RSRP (Reference Signal Received Power)
- RSRQ (Reference Signal Received Quality)
- SINR (Signal-to-Interference-plus-Noise Ratio)

**Messages**

- Measurement Report

**Interfaces**

- Uu (UE ↔ Source gNodeB)

---

### Step 2: Source gNodeB Makes Handover Decision

The source gNodeB evaluates the measurement report together with network policies, load conditions, and mobility parameters.

If a neighboring cell provides better radio conditions and an Xn connection exists, the source gNodeB decides to initiate an Xn Handover.

---

### Step 3: Source gNodeB Sends Xn Handover Request

The source gNodeB sends an **XnAP Handover Request** to the target gNodeB.

The request includes:

- UE Context
- Security Context
- PDU Session Information
- QoS Flow Information
- DRB Configuration
- UE Capability Information

**Messages**

- XnAP Handover Request

**Interfaces**

- Xn (Source gNodeB ↔ Target gNodeB)

---

### Step 4: Target gNodeB Prepares Resources

The target gNodeB reserves the required radio resources for the incoming UE.

If resource allocation is successful, it returns an **XnAP Handover Request Acknowledge** containing the target cell configuration required by the UE.

**Messages**

- XnAP Handover Request Acknowledge

**Interfaces**

- Xn (Target gNodeB ↔ Source gNodeB)

---

### Step 5: Source gNodeB Sends Handover Command

After receiving the acknowledgement, the source gNodeB sends an **RRC Reconfiguration** message to the UE.

This message contains the handover command and the information required to access the target cell.

**Messages**

- RRC Reconfiguration (Handover Command)

**Interfaces**

- Uu (UE ↔ Source gNodeB)

---

### Step 6: UE Synchronizes with the Target gNodeB

The UE disconnects from the source cell and synchronizes with the target cell.

After successful synchronization, the UE sends an **RRC Reconfiguration Complete** message to the target gNodeB.

**Messages**

- Random Access Procedure
- RRC Reconfiguration Complete

**Interfaces**

- Uu (UE ↔ Target gNodeB)

---

### Step 7: Path Switch Procedure

After confirming that the UE has successfully attached to the target cell, the target gNodeB notifies the AMF.

The AMF requests the SMF to update the user plane path, and the SMF updates the UPF using PFCP signaling so that GTP-U traffic is redirected to the target gNodeB.

**Messages**

- NGAP Path Switch Request
- Nsmf_PDUSession_UpdateSMContext
- PFCP Session Modification Request
- PFCP Session Modification Response
- NGAP Path Switch Request Acknowledge

**Interfaces**

- N2 (gNodeB ↔ AMF)
- N11 (AMF ↔ SMF)
- N4 (SMF ↔ UPF)

---

### Step 8: Source gNodeB Releases Resources

After the path switch is complete, the target gNodeB informs the source gNodeB that the handover has finished.

The source gNodeB releases the UE context, radio resources, and forwarding state associated with the previous serving cell.

**Messages**

- XnAP UE Context Release

**Interfaces**

- Xn (Source gNodeB ↔ Target gNodeB)

---

### Procedure Complete

The UE is now served by the target gNodeB.

**Result**

- UE successfully connected to the target gNodeB
- Existing PDU Sessions preserved
- QoS Flows maintained
- GTP-U path switched to the target gNodeB
- User traffic continues with minimal interruption
- Source gNodeB resources released
  ## Prerequisites for Xn Handover

Before an Xn Handover can be performed, the following conditions should be met:

| Requirement | Description |
|------------|-------------|
| Xn Connectivity | A functional Xn interface must exist between the source and target gNodeBs. |
| Common AMF | Both gNodeBs should be served by the same AMF. Otherwise, an N2 Handover may be required. |
| Neighbor Relation | The target cell must be configured as a valid neighbor of the source cell. |
| Active RRC Connection | The UE must be in **RRC_CONNECTED** state. |
| Active PDU Session | At least one active PDU Session should exist to maintain ongoing user traffic. |
| Valid Security Context | The UE security context must be available for transfer to the target gNodeB. |
| Available Radio Resources | The target gNodeB must have sufficient radio resources to admit the UE. |
| Mobility Configuration | Measurement events and handover parameters must be configured on both gNodeBs. |

## Message Summary

| Step | Sender | Receiver | Message | Purpose |
|------|---------|----------|---------|---------|
| 1 | UE | Source gNodeB | Measurement Report | Report neighboring cell measurements |
| 2 | Source gNodeB | Target gNodeB | XnAP Handover Request | Request handover preparation |
| 3 | Target gNodeB | Source gNodeB | XnAP Handover Request Acknowledge | Confirm resource reservation |
| 4 | Source gNodeB | UE | RRC Reconfiguration (Handover Command) | Instruct the UE to move to the target cell |
| 5 | UE | Target gNodeB | Random Access Procedure | Synchronize with the target cell |
| 6 | UE | Target gNodeB | RRC Reconfiguration Complete | Confirm successful handover |
| 7 | Target gNodeB | AMF | NGAP Path Switch Request | Notify the AMF that the UE is now served by the target gNodeB |
| 8 | AMF | SMF | Nsmf_PDUSession_UpdateSMContext Request | Update the user plane path |
| 9 | SMF | UPF | PFCP Session Modification Request | Redirect GTP-U traffic to the target gNodeB |
| 10 | UPF | SMF | PFCP Session Modification Response | Confirm path update |
| 11 | AMF | Target gNodeB | NGAP Path Switch Request Acknowledge | Confirm successful path switch |
| 12 | Target gNodeB | Source gNodeB | XnAP UE Context Release | Release resources in the source gNodeB |

## Interface Summary

| Interface | Network Functions | Protocol | Purpose |
|-----------|-------------------|----------|---------|
| Uu | UE ↔ gNodeB | RRC | Measurement reporting and handover execution |
| Xn | Source gNodeB ↔ Target gNodeB | XnAP | Exchange UE context and coordinate the handover |
| N2 | Target gNodeB ↔ AMF | NGAP | Notify the AMF and perform the Path Switch procedure |
| N11 | AMF ↔ SMF | HTTP/2 (SBI) | Update PDU Session routing information |
| N4 | SMF ↔ UPF | PFCP | Modify forwarding rules and update the user plane path |
| N3 | gNodeB ↔ UPF | GTP-U | Carry user plane traffic after the path switch |

## Key Takeaways

- Xn Handover is a **gNodeB-to-gNodeB handover** that uses the Xn interface for direct communication.
- The source and target gNodeBs exchange the UE context directly without involving the AMF during handover preparation.
- The AMF becomes involved only during the **Path Switch** procedure after the UE successfully attaches to the target gNodeB.
- Existing PDU Sessions remain active throughout the handover.
- The SMF updates the UPF so that GTP-U traffic is redirected to the target gNodeB.
- User interruption is typically very short because the target cell prepares resources before the UE moves.
- Xn Handover generally has lower signaling overhead and lower latency than N2 Handover.
- If an Xn interface is unavailable or the target gNodeB cannot accept the UE, the network may perform an **N2 Handover** instead.

  ## Comparison with Similar Procedures

  | Xn Handover                           | N2 Handover                                 |
| ------------------------------------- | ------------------------------------------- |
| Direct communication between gNodeBs  | AMF coordinates the handover                |
| Uses Xn interface                     | Uses N2 interface                           |
| Lower signaling overhead              | Higher signaling overhead                   |
| Faster execution                      | More signaling through the Core             |
| Preferred when Xn connectivity exists | Used when Xn is unavailable or not suitable |

## Troubleshooting

The following table lists common issues that may occur during the Xn Handover procedure, along with their possible causes and recommended troubleshooting actions.

| Issue | Possible Cause | Recommended Action |
|-------|----------------|--------------------|
| Handover not triggered | Measurement thresholds not met or incorrect mobility configuration | Verify A3/A5 event configuration, neighbor relations, and measurement reports |
| XnAP Handover Request failure | Xn interface unavailable or target gNodeB unreachable | Check Xn connectivity, SCTP association, and XnAP logs |
| Handover Request rejected | Target gNodeB lacks available radio resources or admission control rejects the UE | Verify admission control logs, radio resource utilization, and target cell capacity |
| Random Access failure | Poor radio conditions or synchronization failure | Check PRACH configuration, radio coverage, and UE signal quality |
| Path Switch failure | N2, N11, or N4 signaling failure | Verify NGAP, SBI, and PFCP message exchanges |
| PFCP Session Modification failure | UPF unreachable or PFCP session inconsistency | Check UPF status, N4 connectivity, and PFCP session identifiers |
| User traffic interrupted after handover | GTP-U path not updated or forwarding rules incorrect | Verify Path Switch completion and GTP-U tunnel redirection |
| Source resources not released | UE Context Release message missing or failed | Check XnAP UE Context Release signaling and source gNodeB logs |

### Recommended Troubleshooting Flow

When troubleshooting an Xn Handover, verify the procedure in the following order:

1. Confirm that the UE sent valid Measurement Reports.
2. Verify that the source gNodeB selected the correct target cell.
3. Confirm successful **XnAP Handover Request** and **Handover Request Acknowledge** messages.
4. Verify that the UE successfully completed the Random Access Procedure on the target gNodeB.
5. Confirm the **RRC Reconfiguration Complete** message.
6. Verify the **NGAP Path Switch Request** between the target gNodeB and the AMF.
7. Confirm successful **PFCP Session Modification** between the SMF and UPF.
8. Verify that GTP-U traffic is flowing through the target gNodeB.
9. Confirm that the source gNodeB released the UE context.

### KPIs to Monitor

The following KPIs are commonly monitored for Xn Handover performance:

- Xn Handover Success Rate
- Xn Handover Failure Rate
- Handover Preparation Success Rate
- Handover Execution Success Rate
- Random Access Success Rate
- Path Switch Success Rate
- PFCP Session Modification Success Rate
- Handover Interruption Time
- Packet Loss During Handover

  ## Wireshark / Packet Capture Analysis

When analyzing an Xn Handover, the following signaling messages should be observed:

| Protocol | Message | Description |
|----------|----------|-------------|
| RRC | Measurement Report | UE reports neighboring cell measurements |
| XnAP | Handover Request | Source gNodeB requests handover preparation |
| XnAP | Handover Request Acknowledge | Target gNodeB confirms resource allocation |
| RRC | RRC Reconfiguration | Source gNodeB sends the handover command |
| RRC | Random Access Procedure | UE accesses the target cell |
| RRC | RRC Reconfiguration Complete | UE confirms successful attachment |
| NGAP | Path Switch Request | Target gNodeB informs the AMF |
| HTTP/2 (SBI) | Nsmf_PDUSession_UpdateSMContext | Update PDU Session routing |
| PFCP | Session Modification Request | Update forwarding rules in the UPF |
| PFCP | Session Modification Response | Confirm successful path update |
| NGAP | Path Switch Request Acknowledge | Complete the path switch |
| XnAP | UE Context Release | Release resources in the source gNodeB |

### Useful Wireshark Display Filters

```text
rrc-nr
xnap
ngap
pfcp
http2
gtp
```

> **Note:** During a successful Xn Handover, the GTP-U tunnel is redirected to the target gNodeB after the Path Switch procedure. Temporary packet buffering at the source or target gNodeB may minimize packet loss during the transition.
>
> ## References

- 3GPP TS 23.502 – Procedures for the 5G System
- 3GPP TS 38.300 – NR Overall Description
- 3GPP TS 38.331 – NR Radio Resource Control (RRC)
- 3GPP TS 38.401 – NG-RAN Architecture Description
- 3GPP TS 38.423 – Xn Application Protocol (XnAP)
- 3GPP TS 38.413 – NG Application Protocol (NGAP)
- 3GPP TS 29.244 – Packet Forwarding Control Protocol (PFCP)
- 3GPP TS 29.502 – Session Management Services

---

## Related Procedures

### Previous Procedures

- [5G Registration Procedure](../Registration/)
- [5G PDU Session Establishment](../PDU-Session-Establishment/)
- [5G Service Request](../Service-Request/)
- [5G PDU Session Modification](../PDU-Session-Modification/)
- [5G PDU Session Release](../PDU-Session-Release/)

### Related Mobility Procedures

- N2 Handover
- Paging Procedure
- UE Context Release
- UE Deregistration

- 
