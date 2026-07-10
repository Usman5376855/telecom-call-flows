
# 5G Paging Procedure

## Overview

The **5G Paging Procedure** is used by the 5G Core Network to notify an idle User Equipment (UE) of pending downlink data, voice services, emergency notifications, or signaling procedures. Since the UE is in **RRC_IDLE** or **RRC_INACTIVE** state, the network cannot deliver data directly and must first page the UE.

The Access and Mobility Management Function (AMF) initiates paging by sending a Paging message to one or more gNodeBs within the UE's registered Tracking Area. After receiving the Paging message, the UE establishes an RRC connection and typically initiates a **Service Request** procedure to resume active communication.

## Purpose

- Notify an idle UE of pending downlink data.
- Trigger the UE to resume active communication.
- Support incoming voice services such as VoNR.
- Deliver emergency notifications and network signaling.
- Minimize UE battery consumption by allowing the UE to remain idle until needed.

## Network Functions Involved

- User Equipment (UE)
- gNodeB (gNodeB)
- Access and Mobility Management Function (AMF)
- Session Management Function (SMF) *(for pending user data scenarios)*
- User Plane Function (UPF) *(stores or buffers downlink packets until the UE becomes reachable)*

## Step-by-Step Procedure

### Step 1: Downlink Data Arrives

A remote server sends packets toward the UE.

The packets reach the UPF, but the UE is currently in **RRC_IDLE** or **RRC_INACTIVE**, so user data cannot be delivered immediately.

The UPF informs the SMF about pending downlink data, and the SMF notifies the AMF that paging is required.

**Interfaces**

- N4 (UPF ↔ SMF)
- N11 (SMF ↔ AMF)

---

### Step 2: AMF Initiates Paging

The AMF determines the UE's last known Tracking Area (or Tracking Area List) and sends an **NGAP Paging** message to all relevant gNodeBs.

The Paging message typically contains:

- 5G-S-TMSI
- Paging DRX
- Paging Priority (if applicable)
- Paging Cause (optional)

**Messages**

- NGAP Paging

**Interfaces**

- N2 (AMF ↔ gNodeB)

---

### Step 3: gNodeBs Broadcast the Paging Message

Each gNodeB broadcasts the Paging message over the radio interface.

Only the UE matching the paging identity responds.

Other UEs ignore the paging message.

**Interfaces**

- Uu (gNodeB ↔ UE)

---

### Step 4: UE Responds

After detecting its paging identity, the UE establishes an RRC connection with the serving gNodeB.

The UE then initiates the **Service Request** procedure to resume NAS signaling and user traffic.

**Messages**

- RRC Setup Procedure
- Service Request (NAS)

**Interfaces**

- Uu
- N2

---

### Step 5: Service is Resumed

The AMF processes the Service Request.

If required, the SMF updates the user plane path and the UPF resumes forwarding buffered packets.

The UE returns to **RRC_CONNECTED**, and downlink traffic continues normally.

**Result**

- UE successfully paged.
- RRC connection established.
- Service Request completed.
- Downlink traffic resumed.
- Buffered packets delivered.

  ## Common Paging Triggers

The network may initiate paging for several reasons. The table below summarizes the most common scenarios.

| Trigger | Description |
|---------|-------------|
| Downlink Data Arrival | The UPF receives downlink packets for a UE in RRC_IDLE or RRC_INACTIVE. |
| Incoming VoNR Call | The UE is paged to establish the signaling required for an incoming voice call. |
| SMS Delivery | The network pages the UE to deliver SMS over NAS. |
| Emergency Notification | The network needs to notify the UE of emergency-related information. |
| Network-Initiated Signaling | The AMF needs to deliver NAS signaling to the UE. |
| UE Configuration Update | The network pages the UE before delivering configuration updates. |

> **Note:** The most common paging trigger in commercial 5G networks is **pending downlink user data**.

## Prerequisites for Paging

Before the Paging procedure can be initiated, the following conditions should be met:

| Requirement | Description |
|------------|-------------|
| UE Registered | The UE must already be successfully registered with the 5G Core Network. |
| Valid UE Context | The AMF must maintain a valid UE context. |
| Known Tracking Area | The AMF must know the UE's last registered Tracking Area or Tracking Area List (TAI/TAI List). |
| Idle UE | The UE should be in **RRC_IDLE** or **RRC_INACTIVE** state. |
| Active PDU Session *(Typical)* | An active PDU Session usually exists when paging is triggered by downlink user data. |
| N2 Connectivity | The AMF must have connectivity with the serving gNodeBs through the N2 interface. |

## Message Summary

| Step | Sender | Receiver | Message | Purpose |
|------|---------|----------|---------|---------|
| 1 | UPF | SMF | Downlink Data Notification | Inform the SMF that buffered downlink data exists |
| 2 | SMF | AMF | Downlink Data Notification | Request paging for the UE |
| 3 | AMF | gNodeB(s) | NGAP Paging | Page the UE within the Tracking Area |
| 4 | gNodeB | UE | RRC Paging | Broadcast the paging message |
| 5 | UE | gNodeB | RRC Setup Request | Respond to paging and establish an RRC connection |
| 6 | gNodeB | UE | RRC Setup | Configure the radio connection |
| 7 | UE | gNodeB | RRC Setup Complete | Complete RRC connection establishment |
| 8 | UE | AMF | Service Request | Resume NAS signaling and user traffic |

## Interface Summary

| Interface | Network Functions | Protocol | Purpose |
|-----------|-------------------|----------|---------|
| N4 | UPF ↔ SMF | PFCP | Notify the SMF of buffered downlink data |
| N11 | SMF ↔ AMF | HTTP/2 (SBI) | Request paging for the UE |
| N2 | AMF ↔ gNodeB | NGAP | Deliver the Paging message |
| Uu | gNodeB ↔ UE | RRC / NAS | Broadcast paging and establish the RRC connection |
| N3 | gNodeB ↔ UPF | GTP-U | Resume user traffic after the Service Request completes |

## Key Takeaways

- Paging is initiated by the network when an idle UE must be contacted.
- The UE is typically in **RRC_IDLE** or **RRC_INACTIVE** when paging occurs.
- The AMF is responsible for coordinating the Paging procedure.
- Paging messages are distributed to the appropriate gNodeBs over the **N2 interface**.
- Only the UE matching the paging identity responds to the Paging message.
- Paging itself does **not** resume user traffic; it prompts the UE to establish an RRC connection.
- After responding to Paging, the UE normally initiates the **Service Request** procedure to resume user plane communication.
- Paging minimizes UE battery consumption by allowing the UE to remain idle until network communication is required.
- The most common paging trigger is pending downlink user data.

  ## UE State Transition

The following state transitions typically occur during the Paging procedure:

RRC_IDLE
      │
      ▼
Paging Received
      │
      ▼
RRC Setup
      │
      ▼
Service Request
      │
      ▼
RRC_CONNECTED

## Troubleshooting

The following table lists common issues that may occur during the Paging procedure, along with their possible causes and recommended troubleshooting actions.

| Issue | Possible Cause | Recommended Action |
|-------|----------------|--------------------|
| UE does not respond to Paging | UE is powered off, out of coverage, or monitoring the wrong paging occasion | Verify UE availability, cell coverage, DRX configuration, and paging occasions |
| NGAP Paging not received by the gNodeB | N2 connectivity issue, SCTP failure, or AMF signaling problem | Verify N2 connectivity, SCTP association, and NGAP logs |
| Paging sent to incorrect Tracking Area | Outdated UE location information or incorrect Tracking Area configuration | Verify the UE Registration Area, Tracking Area List (TAI List), and mobility updates |
| RRC Setup failure after Paging | Poor radio conditions or PRACH configuration issue | Check radio coverage, PRACH parameters, and RRC logs |
| Service Request failure | NAS signaling issue or invalid UE context | Verify NAS messages, AMF logs, and UE registration status |
| Buffered downlink packets not delivered | User plane path not restored or PDU Session issue | Verify Service Request completion, N4 signaling, and GTP-U forwarding |
| Excessive Paging load | Large Tracking Areas or paging storms | Review Tracking Area planning, paging statistics, and AMF load balancing |
| High Paging latency | AMF overload, gNodeB processing delay, or transport latency | Analyze AMF processing time, NGAP latency, and paging success KPIs |

### Recommended Troubleshooting Flow

When troubleshooting a Paging procedure, verify the following sequence:

1. Confirm that downlink data or another paging trigger occurred.
2. Verify that the AMF generated the **NGAP Paging** message.
3. Confirm that the Paging message reached the appropriate gNodeBs.
4. Verify that the UE monitored the correct paging occasion.
5. Confirm successful **RRC Setup** between the UE and gNodeB.
6. Verify that the UE sent a **Service Request**.
7. Confirm that the AMF accepted the Service Request.
8. Verify that user traffic resumed successfully through the UPF.

### KPIs to Monitor

The following KPIs are commonly monitored for Paging performance:

- Paging Success Rate
- Paging Response Rate
- Paging Failure Rate
- Paging Latency
- RRC Setup Success Rate
- Service Request Success Rate
- Downlink Packet Delivery Success Rate
- Average Paging Attempts per UE
- ## Wireshark / Packet Capture Analysis

When analyzing the Paging procedure, the following signaling messages should be observed:

| Protocol | Message | Description |
|----------|----------|-------------|
| PFCP | Downlink Data Report *(if applicable)* | UPF indicates buffered downlink data |
| HTTP/2 (SBI) | Downlink Data Notification | SMF informs the AMF that paging is required |
| NGAP | Paging | AMF pages the UE through one or more gNodeBs |
| RRC | Paging | gNodeB broadcasts the paging message |
| RRC | RRC Setup Request | UE responds by requesting an RRC connection |
| RRC | RRC Setup | gNodeB establishes the radio connection |
| RRC | RRC Setup Complete | UE completes the RRC connection setup |
| NAS | Service Request | UE requests the resumption of signaling and user traffic |

### Useful Wireshark Display Filters

```text
ngap
rrc-nr
nas-5gs
pfcp
http2
gtp
```

> **Note:** Paging itself does not transport user data. It is a notification mechanism that causes the UE to establish an RRC connection and initiate a Service Request, after which buffered user traffic can be delivered.

## References

- 3GPP TS 23.502 – Procedures for the 5G System
- 3GPP TS 24.501 – Non-Access-Stratum (NAS) Protocol for 5GS
- 3GPP TS 38.300 – NR Overall Description
- 3GPP TS 38.331 – NR Radio Resource Control (RRC)
- 3GPP TS 38.413 – NG Application Protocol (NGAP)
- 3GPP TS 29.244 – Packet Forwarding Control Protocol (PFCP)
- 3GPP TS 29.502 – Session Management Services

---

## Related Procedures

### Previous Procedures

- [5G Registration Procedure](../Registration/)
- [5G Service Request](../Service-Request/)
- [5G PDU Session Establishment](../PDU-Session-Establishment/)
- [5G PDU Session Modification](../PDU-Session-Modification/)
- [5G PDU Session Release](../PDU-Session-Release/)
- [5G Xn Handover](../Xn-Handover/)
- [5G N2 Handover](../N2-Handover/)

### Next Procedures

- UE Context Release
- UE Deregistration
- Network-Initiated Deregistration
- Mobility Registration Update

  
