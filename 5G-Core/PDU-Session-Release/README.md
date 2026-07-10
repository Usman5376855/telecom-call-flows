
# 5G PDU Session Release Procedure

## Overview

The **5G PDU Session Release Procedure** is used to terminate an existing PDU Session between the User Equipment (UE) and the 5G Core network. During this procedure, the network releases the signaling context, removes user plane tunnels, and frees resources that were allocated for the PDU Session.

A PDU Session Release may be initiated either by the **UE** or by the **network** depending on operational requirements. Common triggers include user disconnect requests, subscription changes, policy decisions, network optimization, or prolonged inactivity.

## Purpose

- Release an existing PDU Session
- Remove user plane resources from the UPF
- Delete the PFCP session associated with the PDU Session
- Release QoS flows and forwarding rules
- Free network resources
- Notify all involved network functions about the session release

## Network Functions Involved

- User Equipment (UE)
- gNodeB (gNB)
- Access and Mobility Management Function (AMF)
- Session Management Function (SMF)
- User Plane Function (UPF)

## Step-by-Step Procedure

### Step 1: UE Initiates PDU Session Release

The UE decides to release an active PDU Session. This may occur when the user disconnects from a data service, disables mobile data, or an application no longer requires network connectivity.

The UE sends a **PDU Session Release Request** NAS message to the network.

**Messages**

- PDU Session Release Request (NAS)

**Interfaces**

- Uu (UE ↔ gNodeB)

---

### Step 2: gNodeB Forwards the Request to the AMF

The gNodeB receives the NAS message and forwards it to the AMF using NGAP signaling.

The AMF validates the request and identifies the associated PDU Session.

**Messages**

- NGAP Uplink NAS Transport

**Interfaces**

- N2 (gNodeB ↔ AMF)

---

### Step 3: AMF Requests PDU Session Release from the SMF

The AMF forwards the release request to the SMF over the Service-Based Interface (SBI).

The SMF determines the resources associated with the PDU Session and begins the release procedure.

**Messages**

- Nsmf_PDUSession_ReleaseSMContext Request
- Nsmf_PDUSession_ReleaseSMContext Response

**Interfaces**

- N11 (AMF ↔ SMF)

---

### Step 4: SMF Deletes the PFCP Session

The SMF instructs the UPF to remove the PFCP session associated with the PDU Session.

The UPF deletes packet forwarding rules, QoS enforcement rules, charging rules, and GTP-U tunnel information.

After successful deletion, the UPF acknowledges the request.

**Messages**

- PFCP Session Deletion Request
- PFCP Session Deletion Response

**Interfaces**

- N4 (SMF ↔ UPF)

---

### Step 5: User Plane Resources are Released

After the PFCP session is deleted, the user plane resources are released.

The GTP-U tunnel between the gNodeB and UPF is removed, and all traffic associated with the PDU Session stops.

**Result**

- N3 tunnel released
- QoS flows removed
- Packet forwarding terminated

---

### Step 6: AMF Sends Release Command to the UE

After receiving confirmation from the SMF, the AMF informs the UE that the PDU Session has been successfully released.

The UE removes the local PDU Session context.

**Messages**

- PDU Session Release Command
- PDU Session Release Complete

**Interfaces**

- Uu (UE ↔ gNodeB)
- N2 (gNodeB ↔ AMF)

---

### Step 7: Procedure Complete

The PDU Session has now been completely removed from the UE and the 5G Core.

The UE remains registered with the network and can establish a new PDU Session whenever required without performing the Registration procedure again.

**Result**

- PDU Session deleted
- PFCP session deleted
- GTP-U tunnel removed
- QoS flows released
- Network resources freed
- UE remains registered

  ## Types of PDU Session Release

PDU Session Release can be initiated in two ways:

### UE-Initiated Release
The UE requests the release of an existing PDU Session. This is the most common scenario when a user disconnects from a service or no longer requires data connectivity.

### Network-Initiated Release
The network initiates the release due to policy decisions, subscription changes, resource optimization, session timeout, or operator actions.

Although the initiator differs, both procedures ultimately release the same PDU Session context and user plane resources.
## Message Summary

| Step | Sender | Receiver | Message | Purpose |
|------|---------|----------|---------|---------|
| 1 | UE | gNodeB | PDU Session Release Request (NAS) | Request release of an existing PDU Session |
| 2 | gNodeB | AMF | NGAP Uplink NAS Transport | Forward the NAS Release Request |
| 3 | AMF | SMF | Nsmf_PDUSession_ReleaseSMContext Request | Request release of the PDU Session |
| 4 | SMF | UPF | PFCP Session Deletion Request | Delete the PFCP session and forwarding rules |
| 5 | UPF | SMF | PFCP Session Deletion Response | Confirm successful deletion of the PFCP session |
| 6 | SMF | AMF | Nsmf_PDUSession_ReleaseSMContext Response | Notify AMF that the session has been released |
| 7 | AMF | gNodeB | NGAP Downlink NAS Transport | Carry the Release Command to the UE |
| 8 | gNodeB | UE | PDU Session Release Command | Inform the UE that the PDU Session is being released |
| 9 | UE | gNodeB | PDU Session Release Complete | Confirm successful release |
| 10 | gNodeB | AMF | NGAP Uplink NAS Transport | Forward the Release Complete message |

## Interface Summary

| Interface | Network Functions | Protocol | Purpose |
|-----------|-------------------|----------|---------|
| Uu | UE ↔ gNodeB | NAS / RRC | Transport PDU Session Release signaling |
| N2 | gNodeB ↔ AMF | NGAP | Carry NAS messages between the RAN and Core |
| N11 | AMF ↔ SMF | HTTP/2 (SBI) | Coordinate release of the PDU Session |
| N4 | SMF ↔ UPF | PFCP | Delete the PFCP session and user plane rules |
| N3 | gNodeB ↔ UPF | GTP-U | User plane tunnel removed after session release |

## Key Takeaways

- A PDU Session Release removes an existing PDU Session but **does not deregister the UE** from the network.
- The UE remains registered with the AMF and can establish a new PDU Session without repeating the Registration procedure.
- The SMF is responsible for coordinating the release of the PDU Session.
- The UPF removes all packet forwarding rules, QoS enforcement rules, charging rules, and GTP-U tunnel information associated with the released session.
- The PFCP Session Deletion procedure is a key part of releasing user plane resources.
- Releasing unused PDU Sessions helps optimize network resource utilization and reduces unnecessary load on the user plane.
- A PDU Session Release may be initiated by either the UE or the network, depending on operational requirements.
- If the UE still has other active PDU Sessions, only the selected session is released while the remaining sessions continue to operate normally.
- If the released PDU Session was the only active session, the UE may transition to **CM-IDLE** after inactivity, but it remains registered with the network.
- Re-establishing connectivity after a PDU Session Release requires a new **PDU Session Establishment** procedure rather than a Service Request.

  ## Troubleshooting

The following table lists common issues that may occur during the PDU Session Release procedure, along with their possible causes and recommended troubleshooting actions.

| Issue | Possible Cause | Recommended Action |
|-------|----------------|--------------------|
| PDU Session Release Request not received by the AMF | RAN signaling issue, N2 connectivity problem, or NAS message loss | Verify RRC establishment, NGAP Uplink NAS Transport, and AMF logs |
| PDU Session Release rejected | Invalid PDU Session ID, session already released, or inconsistent UE context | Verify UE context, AMF logs, and SMF session information |
| Nsmf_PDUSession_ReleaseSMContext failure | N11 connectivity issue, SMF unavailable, or internal processing error | Check SBI connectivity, HTTP/2 transactions, and SMF logs |
| PFCP Session Deletion failure | UPF unreachable, N4 transport issue, or invalid PFCP session | Verify N4 connectivity, PFCP messages, and UPF status |
| GTP-U tunnel remains active after release | PFCP deletion incomplete or forwarding rules not removed | Verify PFCP Session Deletion Response and confirm GTP-U tunnel removal |
| User traffic continues after session release | Stale forwarding rules or delayed resource cleanup | Check UPF forwarding entries, QoS rules, and packet forwarding state |
| Resource leakage in the UPF | PFCP session not completely deleted | Verify PFCP session database and monitor UPF resource utilization |
| Release procedure timeout | High signaling latency, overloaded network functions, or transport failure | Analyze AMF, SMF, and UPF processing times and review timeout counters |

### Recommended Troubleshooting Flow

When troubleshooting a failed PDU Session Release, verify the procedure in the following order:

1. Confirm that the UE transmitted the **PDU Session Release Request**.
2. Verify that the **NGAP Uplink NAS Transport** message reached the AMF.
3. Confirm successful **Nsmf_PDUSession_ReleaseSMContext** signaling between the AMF and SMF.
4. Verify that the SMF sent a **PFCP Session Deletion Request** to the UPF.
5. Confirm that the UPF returned a successful **PFCP Session Deletion Response**.
6. Verify that the GTP-U tunnel and forwarding rules were removed from the UPF.
7. Confirm that the UE received the **PDU Session Release Command** and responded with **PDU Session Release Complete**.
8. Review logs and alarms on the UE, gNodeB, AMF, SMF, and UPF to identify any failures.

### KPIs to Monitor

During PDU Session Release troubleshooting, the following KPIs are commonly monitored:

- PDU Session Release Success Rate
- PDU Session Release Failure Rate
- PFCP Session Deletion Success Rate
- N11 Signaling Success Rate
- N2 Signaling Success Rate
- Session Release Latency
- UPF Resource Utilization
- Active PFCP Session Count
- Active GTP-U Tunnel Count
  ## Wireshark / Packet Capture Analysis

When analyzing the PDU Session Release procedure in a packet capture, the following signaling messages should be observed in chronological order:

| Protocol | Message | Description |
|----------|----------|-------------|
| NAS | PDU Session Release Request | UE requests the release of an existing PDU Session |
| NGAP | Uplink NAS Transport | gNodeB forwards the NAS message to the AMF |
| HTTP/2 (SBI) | Nsmf_PDUSession_ReleaseSMContext Request | AMF requests the SMF to release the PDU Session |
| HTTP/2 (SBI) | Nsmf_PDUSession_ReleaseSMContext Response | SMF confirms completion of the release procedure |
| PFCP | Session Deletion Request | SMF requests the UPF to delete the PFCP session |
| PFCP | Session Deletion Response | UPF confirms successful deletion of the PFCP session |
| NGAP | Downlink NAS Transport | AMF forwards the Release Command to the UE |
| NAS | PDU Session Release Command | UE is instructed to release the PDU Session |
| NAS | PDU Session Release Complete | UE confirms successful release of the PDU Session |

### Useful Wireshark Display Filters

```text
nas-5gs
ngap
pfcp
http2
gtp
```

> **Note:** After a successful PDU Session Release, the corresponding GTP-U tunnel should no longer carry user traffic. If packets continue to flow, verify that the PFCP session and forwarding rules have been removed correctly from the UPF.

## References

The following 3GPP specifications define the signaling procedures involved in the PDU Session Release process:

- 3GPP TS 23.502 – Procedures for the 5G System
- 3GPP TS 24.501 – Non-Access-Stratum (NAS) Protocol for 5GS
- 3GPP TS 29.244 – Packet Forwarding Control Protocol (PFCP)
- 3GPP TS 29.502 – Session Management Services
- 3GPP TS 38.300 – NR Overall Description
- 3GPP TS 38.413 – NG Application Protocol (NGAP)

  ## Related Procedures

### Previous Procedures

- [5G Service-Based Architecture](../Service-Based-Architecture/)
- [5G Registration Procedure](../Registration/)
- [5G PDU Session Establishment](../PDU-Session-Establishment/)
- [5G Service Request](../Service-Request/)

### Next Procedures

- PDU Session Modification
- UE Deregistration
- Network-Initiated Deregistration
- Paging Procedure
- Xn Handover
- N2 Handover
  
