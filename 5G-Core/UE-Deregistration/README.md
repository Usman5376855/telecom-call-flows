# 5G UE Deregistration Procedure

## Overview

The **5G UE Deregistration Procedure** allows a User Equipment (UE) to gracefully disconnect from the 5G Core Network. During this procedure, the UE informs the Access and Mobility Management Function (AMF) that it wishes to terminate its registration. The network then releases the UE's registration context and, if applicable, releases associated PDU Sessions.

UE Deregistration may be initiated by the UE (for example, when the device is powered off or airplane mode is enabled) or by the network under specific conditions. This document focuses on the **UE-Initiated Deregistration** procedure.

## Purpose

- Remove the UE registration from the 5G Core Network.
- Release NAS and mobility management context.
- Release active PDU Sessions when required.
- Free network resources.
- Ensure a clean and orderly network disconnect.## Step-by-Step Procedure

### Step 1: UE Initiates Deregistration

The UE decides to leave the 5G network.

Common reasons include:

- Device power off
- Airplane mode enabled
- User manually disconnects from the network
- SIM removal
- Subscription change

The UE sends a **NAS Deregistration Request** to the AMF.

The request includes:

- Deregistration Type
- 5G-GUTI (or SUCI/SUPI as applicable)
- Security Information

**Messages**

- NAS Deregistration Request

**Interfaces**

- Uu
- N2

---

### Step 2: AMF Validates the Request

The AMF verifies:

- UE identity
- NAS security
- Registration context
- Active PDU Sessions

If the request is valid, the AMF begins the deregistration procedure.

---

### Step 3: AMF Requests PDU Session Release

If one or more PDU Sessions are active, the AMF notifies the corresponding SMF(s).

The SMF starts releasing the PDU Session.

The SMF then updates the UPF to remove user-plane resources.

**Messages**

- Nsmf_PDUSession_ReleaseSMContext
- PFCP Session Deletion Request
- PFCP Session Deletion Response

**Interfaces**

- N11 (AMF ↔ SMF)
- N4 (SMF ↔ UPF)

---

### Step 4: SMF Releases User Plane Resources

The SMF removes:

- PDU Session Context
- QoS Flow Context
- Tunnel Information
- Charging Context (if applicable)

The UPF deletes the corresponding PFCP session and GTP-U forwarding rules.

---

### Step 5: AMF Removes Registration Context

After all required sessions have been released, the AMF removes:

- Registration Context
- Mobility Context
- Security Context
- Reachability Information

The UE is no longer considered registered with the 5G Core Network.

---

### Step 6: Deregistration Accept

The AMF sends a **NAS Deregistration Accept** message to the UE.

The UE confirms the completion of the deregistration procedure.

**Messages**

- NAS Deregistration Accept

**Interfaces**

- Uu
- N2

---

### Step 7: Procedure Complete

The UE is fully deregistered from the network.

**Result**

- Registration context removed
- Mobility context deleted
- PDU Sessions released (if applicable)
- User plane resources deleted
- UE no longer reachable through Paging
- A new Registration procedure is required before network services can be used again

  ## Common Deregistration Triggers

The UE Deregistration procedure may be initiated for several reasons.

| Trigger | Description |
|---------|-------------|
| Device Power Off | The UE performs a normal deregistration before shutting down. |
| Airplane Mode Enabled | The UE disconnects from the 5G network while remaining powered on. |
| User Manual Network Disconnect | The user intentionally disconnects from the mobile network. |
| SIM or USIM Removal | The UE deregisters because subscriber credentials are no longer available. |
| Network Change | The UE leaves the current PLMN and deregisters before moving to another network (when applicable). |
| Subscription Change | The UE deregisters after changes affecting network access. |

> **Note:** In some situations, such as an unexpected battery removal or crash, the UE may not be able to perform a graceful deregistration. The network then clears the context later using timers or network-initiated procedures.

## Message Summary

| Step | Sender | Receiver | Message | Purpose |
|------|---------|----------|---------|---------|
| 1 | UE | AMF | NAS Deregistration Request | Request deregistration from the 5G Core Network |
| 2 | AMF | SMF | Nsmf_PDUSession_ReleaseSMContext | Request release of active PDU Sessions (if applicable) |
| 3 | SMF | UPF | PFCP Session Deletion Request | Delete user plane resources |
| 4 | UPF | SMF | PFCP Session Deletion Response | Confirm PFCP session deletion |
| 5 | SMF | AMF | Nsmf_PDUSession_ReleaseSMContext Response | Confirm session release |
| 6 | AMF | UE | NAS Deregistration Accept | Confirm successful deregistration |

## Interface Summary

| Interface | Network Functions | Protocol | Purpose |
|-----------|-------------------|----------|---------|
| Uu | UE ↔ gNodeB | RRC / NAS | Carry NAS Deregistration messages |
| N2 | gNodeB ↔ AMF | NGAP | Transport NAS messages between the UE and AMF |
| N11 | AMF ↔ SMF | HTTP/2 (SBI) | Release PDU Session context |
| N4 | SMF ↔ UPF | PFCP | Delete PFCP sessions and user plane forwarding rules |

## Key Takeaways

- UE Deregistration removes the UE's registration from the 5G Core Network.
- The AMF deletes the UE's registration, mobility, and security context.
- Active PDU Sessions are normally released during the procedure.
- The SMF instructs the UPF to delete PFCP sessions and user plane resources.
- After deregistration, the UE cannot receive Paging messages.
- To access network services again, the UE must perform a new **Registration Procedure**.
- UE Deregistration is different from **UE Context Release**, where the UE remains registered and reachable.

  ## UE Context Release vs UE Deregistration

| Feature | UE Context Release | UE Deregistration |
|---------|--------------------|-------------------|
| Registration Context | ✅ Retained | ❌ Deleted |
| RRC Connection | ❌ Released | ❌ Released |
| Radio Resources | ❌ Released | ❌ Released |
| PDU Session Context | ✅ Usually Retained | ❌ Usually Released |
| Paging Supported | ✅ Yes | ❌ No |
| Service Request Possible | ✅ Yes | ❌ No (Registration required first) |
| Registration Required Before Data Transfer | ❌ No | ✅ Yes |
## Troubleshooting

The following table lists common issues that may occur during the UE Deregistration procedure, along with their possible causes and recommended troubleshooting actions.

| Issue | Possible Cause | Recommended Action |
|-------|----------------|--------------------|
| Deregistration Request not received by the AMF | Radio failure, N2 connectivity issue, or NGAP transport problem | Verify RRC connection, SCTP association, NGAP signaling, and AMF logs |
| Deregistration Request rejected | NAS security failure or invalid UE context | Verify NAS security, UE identity, and AMF registration context |
| PDU Session Release failure | SMF processing issue or SBI communication failure | Check N11 signaling, SMF logs, and PDU Session status |
| PFCP Session Deletion failure | UPF unreachable or PFCP session inconsistency | Verify N4 connectivity, PFCP session state, and UPF health |
| Registration context not removed | AMF internal processing failure | Review AMF logs and confirm successful completion of the deregistration procedure |
| UE still receives Paging | UE context not fully removed or paging information not updated | Verify AMF context deletion and paging database synchronization |
| Resource leakage | PDU Session or UPF resources not released | Confirm successful PFCP Session Deletion and monitor UPF resource utilization |

### Recommended Troubleshooting Flow

When troubleshooting a UE Deregistration procedure, verify the following sequence:

1. Confirm that the UE initiated the **NAS Deregistration Request**.
2. Verify successful delivery of the request to the AMF.
3. Confirm that the AMF validated the UE's security context.
4. Verify **PDU Session Release** signaling between the AMF and SMF.
5. Confirm successful **PFCP Session Deletion** between the SMF and UPF.
6. Verify that the AMF removed the UE registration context.
7. Confirm that the UE received the **NAS Deregistration Accept** message.
8. Ensure that the UE is no longer reachable through the Paging procedure.

### KPIs to Monitor

The following KPIs are commonly monitored for UE Deregistration:

- UE Deregistration Success Rate
- UE Deregistration Failure Rate
- PDU Session Release Success Rate
- PFCP Session Deletion Success Rate
- Context Deletion Success Rate
- Resource Cleanup Success Rate
- Deregistration Completion Time

  ## Wireshark / Packet Capture Analysis

When analyzing the UE Deregistration procedure, the following signaling messages should be observed:

| Protocol | Message | Description |
|----------|----------|-------------|
| NAS | Deregistration Request | UE requests deregistration from the 5G Core |
| NGAP | Uplink NAS Transport | gNodeB forwards the NAS Deregistration Request to the AMF |
| HTTP/2 (SBI) | Nsmf_PDUSession_ReleaseSMContext | AMF requests release of active PDU Sessions |
| PFCP | Session Deletion Request | SMF instructs the UPF to delete the PFCP session |
| PFCP | Session Deletion Response | UPF confirms successful deletion |
| NGAP | Downlink NAS Transport | gNodeB forwards the NAS Deregistration Accept to the UE |
| NAS | Deregistration Accept | AMF confirms successful deregistration |

### Useful Wireshark Display Filters

```text
nas-5gs
ngap
pfcp
http2
gtp
```

> **Note:** After a successful UE Deregistration, the AMF no longer maintains a registration context for the UE, and any associated PDU Sessions are typically released. Subsequent network access requires a new Registration procedure.
> ## References

- 3GPP TS 23.502 – Procedures for the 5G System
- 3GPP TS 24.501 – Non-Access-Stratum (NAS) Protocol for the 5GS
- 3GPP TS 38.300 – NR Overall Description
- 3GPP TS 38.413 – NG Application Protocol (NGAP)
- 3GPP TS 29.244 – Packet Forwarding Control Protocol (PFCP)
- 3GPP TS 29.502 – Session Management Services

---

## Related Procedures

### Previous Procedures

- [5G Registration Procedure](../Registration/)
- [5G Paging Procedure](../Paging/)
- [5G Service Request](../Service-Request/)
- [5G UE Context Release](../UE-Context-Release/)
- [5G PDU Session Release](../PDU-Session-Release/)

### Related Procedures

- Network-Initiated Deregistration
- Mobility Registration Update
- Authentication Procedure
- Security Mode Control
- 

## Network Functions Involved

- User Equipment (UE)
- gNodeB
- Access and Mobility Management Function (AMF)
- Session Management Function (SMF)
- User Plane Function (UPF)
