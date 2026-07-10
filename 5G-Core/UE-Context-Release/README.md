
# 5G UE Context Release Procedure

## Overview

The **5G UE Context Release Procedure** is used to release the UE's signaling context and radio resources after communication has finished or when the UE transitions from **RRC_CONNECTED** to **RRC_IDLE**. This procedure frees network resources while allowing the UE to remain registered with the 5G Core Network.

The procedure is coordinated between the gNodeB and the Access and Mobility Management Function (AMF). Although the RAN context is released, the AMF retains the UE's registration context, enabling the UE to resume communication later through the **Paging** and **Service Request** procedures.

## Purpose

- Release unnecessary radio resources
- Remove UE context from the gNodeB
- Reduce signaling and radio resource utilization
- Preserve the UE registration context in the AMF
- Enable efficient transition to idle mode

## Network Functions Involved

- User Equipment (UE)
- gNodeB
- Access and Mobility Management Function (AMF)

## Step-by-Step Procedure

### Step 1: UE Communication Becomes Inactive

The UE completes its data transmission or signaling activity.

No additional uplink or downlink traffic is detected for a configured period, and the gNodeB starts monitoring the inactivity timer.

Typical scenarios include:

- Web browsing session completed
- File download finished
- Video streaming stopped
- User becomes inactive

The UE remains in **RRC_CONNECTED** during this period.

---

### Step 2: Inactivity Timer Expires

When the configured RAN inactivity timer expires, the gNodeB decides that the dedicated radio resources are no longer required.

Instead of keeping the UE in **RRC_CONNECTED**, the gNodeB initiates the UE Context Release procedure to optimize radio resource utilization.

> **Note:** The inactivity timer is vendor-specific and configurable by the operator.

---

### Step 3: gNodeB Sends UE Context Release Request

The gNodeB informs the AMF that the UE context can be released.

The request includes the release cause, such as:

- User inactivity
- Radio connection lost
- Normal release
- Radio resource optimization

**Messages**

- NGAP UE Context Release Request

**Interfaces**

- N2 (gNodeB ↔ AMF)

---

### Step 4: AMF Responds

The AMF verifies the UE context.

Since the UE remains registered, the AMF retains:

- Registration Context
- Security Context
- Subscription Information
- Mobility Information
- PDU Session Context

The AMF instructs the gNodeB to release the RAN resources.

**Messages**

- NGAP UE Context Release Command

**Interfaces**

- N2 (AMF ↔ gNodeB)

---

### Step 5: gNodeB Releases Radio Resources

The gNodeB releases all radio resources associated with the UE, including:

- RRC Context
- SRBs (Signaling Radio Bearers)
- DRBs (Data Radio Bearers)
- Scheduling resources

The gNodeB removes the UE context from the RAN.

---

### Step 6: UE Enters RRC_IDLE

After the radio connection is released, the UE transitions from:

**RRC_CONNECTED → RRC_IDLE**

The UE begins monitoring:

- Paging occasions
- System Information
- Neighboring cells

Battery consumption is significantly reduced while the UE remains reachable through paging.

---

### Step 7: Procedure Complete

The UE is no longer connected to the gNodeB, but it is **still registered** with the 5G Core Network.

If new downlink data arrives:

1. The AMF initiates the **Paging Procedure**.
2. The UE responds to the Paging message.
3. The UE performs a **Service Request**.
4. User traffic resumes without requiring a new Registration procedure.

**Result**

- Radio resources released
- UE context removed from the gNodeB
- Registration context retained by the AMF
- PDU Session context preserved
- UE transitions to **RRC_IDLE**
- Network resources optimized

  ## Common Triggers for UE Context Release

The UE Context Release procedure may be initiated in several scenarios:

| Trigger | Description |
|---------|-------------|
| User Inactivity | No uplink or downlink traffic is detected before the configured inactivity timer expires. |
| Radio Resource Optimization | The gNodeB releases dedicated radio resources to improve overall cell capacity. |
| RRC Connection Release | The UE transitions from **RRC_CONNECTED** to **RRC_IDLE** after communication ends. |
| Radio Link Failure | The radio connection is lost due to poor signal quality or mobility events. |
| Handover Completion | The source gNodeB releases the old UE context after a successful handover. |
| UE-Initiated Connection Release | The UE releases the RRC connection under specific network conditions. |

> **Note:** The most common trigger in commercial networks is **user inactivity**, where the RAN inactivity timer expires.
## Prerequisites for UE Context Release

Before the UE Context Release procedure can occur, the following conditions should be satisfied:

| Requirement | Description |
|------------|-------------|
| Successful Registration | The UE must already be registered with the 5G Core Network. |
| Active RRC Connection | The UE must initially be in **RRC_CONNECTED** state. |
| Valid UE Context | The gNodeB and AMF must maintain the UE context. |
| N2 Connectivity | The gNodeB and AMF must have an active NGAP connection. |
| Active or Preserved PDU Session | One or more PDU Sessions may remain established after the release. |
| No Ongoing Critical Procedure | No registration, handover, or session management procedure should be in progress. |
## Message Summary

| Step | Sender | Receiver | Message | Purpose |
|------|---------|----------|---------|---------|
| 1 | gNodeB | AMF | NGAP UE Context Release Request | Request release of the UE's RAN context |
| 2 | AMF | gNodeB | NGAP UE Context Release Command | Instruct the gNodeB to release radio resources |
| 3 | gNodeB | AMF | NGAP UE Context Release Complete | Confirm successful release of the UE context |
| 4 | UE | — | RRC State Transition | UE transitions from **RRC_CONNECTED** to **RRC_IDLE** |
## Interface Summary

| Interface | Network Functions | Protocol | Purpose |
|-----------|-------------------|----------|---------|
| N2 | gNodeB ↔ AMF | NGAP | Coordinate the UE Context Release procedure |
| Uu | UE ↔ gNodeB | RRC | Release the radio connection and transition the UE to **RRC_IDLE** |
## Key Takeaways

- UE Context Release frees radio resources while keeping the UE registered with the 5G Core Network.
- Only the **RAN context** is removed; the AMF retains the UE's registration and mobility context.
- Existing PDU Session contexts are typically preserved in the SMF and AMF.
- After the procedure, the UE transitions from **RRC_CONNECTED** to **RRC_IDLE**.
- The UE remains reachable through the **Paging Procedure**.
- When new traffic arrives, the UE resumes communication by responding to Paging and initiating a **Service Request**.
- UE Context Release improves radio resource efficiency and reduces unnecessary signaling load.
- This procedure should not be confused with **UE Deregistration**, which removes the UE's registration context from the Core Network.
  ## Context Status After UE Context Release

| Context | Status |
|---------|--------|
| Registration Context (AMF) | ✅ Retained |
| Security Context | ✅ Retained |
| PDU Session Context | ✅ Retained |
| QoS Flow Information | ✅ Retained |
| UE Context in gNodeB | ❌ Released |
| RRC Connection | ❌ Released |
| Radio Bearers (SRB/DRB) | ❌ Released |
| Dedicated Radio Resources | ❌ Released |
## Troubleshooting

The following table lists common issues that may occur during the UE Context Release procedure, along with their possible causes and recommended troubleshooting actions.

| Issue | Possible Cause | Recommended Action |
|-------|----------------|--------------------|
| UE Context Release Request not received by the AMF | N2 connectivity issue, SCTP failure, or NGAP signaling problem | Verify SCTP association, NGAP signaling, and AMF logs |
| UE Context Release Command not received by the gNodeB | AMF processing issue or N2 transport failure | Check AMF logs, N2 connectivity, and NGAP message flow |
| UE remains in RRC_CONNECTED | Inactivity timer not expired or ongoing user traffic | Verify RAN inactivity timer configuration and check for active data sessions |
| UE context not released in the gNodeB | Internal gNodeB processing issue or unsuccessful release procedure | Review gNodeB logs and confirm receipt of the UE Context Release Command |
| Paging fails after Context Release | UE not monitoring paging occasions, incorrect Tracking Area information, or stale UE context | Verify Paging procedure, TAI configuration, and UE registration status |
| Service Request fails after Paging | Registration context mismatch or NAS signaling issue | Check AMF UE context, NAS signaling, and security context |
| Radio resources remain allocated | Resource cleanup failure in the gNodeB | Verify resource release logs and monitor radio resource utilization |
| UE Context Release during handover fails | Handover procedure incomplete or context synchronization issue | Review handover logs, NGAP signaling, and UE context consistency |

### Recommended Troubleshooting Flow

When troubleshooting a UE Context Release procedure, verify the following sequence:

1. Confirm that the inactivity timer or release trigger occurred.
2. Verify the **NGAP UE Context Release Request** from the gNodeB to the AMF.
3. Confirm that the AMF sent the **NGAP UE Context Release Command**.
4. Verify that the gNodeB released the UE's RRC context and radio bearers.
5. Confirm the **NGAP UE Context Release Complete** message.
6. Verify that the UE transitioned to **RRC_IDLE**.
7. If paging is expected later, confirm successful **Paging** and **Service Request** procedures.
8. Ensure that the UE registration context remains available in the AMF.

### KPIs to Monitor

The following KPIs are commonly monitored for UE Context Release performance:

- UE Context Release Success Rate
- UE Context Release Failure Rate
- RRC Connection Release Success Rate
- RRC Connection Retention Time
- Paging Success Rate After Context Release
- Service Request Success Rate
- Radio Resource Utilization
- Average RRC Connected Duration

  ## Wireshark / Packet Capture Analysis

When analyzing the UE Context Release procedure, the following signaling messages should be observed:

| Protocol | Message | Description |
|----------|----------|-------------|
| NGAP | UE Context Release Request | gNodeB requests release of the UE context |
| NGAP | UE Context Release Command | AMF instructs the gNodeB to release the UE context |
| RRC | RRC Release | Release the radio connection between the UE and gNodeB |
| NGAP | UE Context Release Complete | gNodeB confirms successful release |

### Useful Wireshark Display Filters

```text
ngap
rrc-nr
nas-5gs
```

> **Note:** After UE Context Release, there should be no active RRC connection between the UE and the gNodeB. However, the UE remains registered with the AMF and can be reached later through the Paging procedure.

## References

- 3GPP TS 23.502 – Procedures for the 5G System
- 3GPP TS 24.501 – Non-Access-Stratum (NAS) Protocol for 5GS
- 3GPP TS 38.300 – NR Overall Description
- 3GPP TS 38.331 – NR Radio Resource Control (RRC)
- 3GPP TS 38.413 – NG Application Protocol (NGAP)

---

## Related Procedures

### Previous Procedures

- [5G Registration Procedure](../Registration/)
- [5G Paging Procedure](../Paging/)
- [5G Service Request](../Service-Request/)
- [5G Xn Handover](../Xn-Handover/)
- [5G N2 Handover](../N2-Handover/)

### Next Procedures

- UE Deregistration
- Network-Initiated Deregistration
- Mobility Registration Update
  
