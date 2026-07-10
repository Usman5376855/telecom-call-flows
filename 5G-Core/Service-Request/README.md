
# 5G Service Request Procedure

## Overview

The **5G Service Request Procedure** is used when a User Equipment (UE) is in **CM-IDLE** state and needs to resume data communication. The UE initiates the Service Request to transition back to **CM-CONNECTED**, allowing the network to re-establish the N2 signaling connection and reactivate the user plane.

Unlike the Registration procedure, the UE is already authenticated and registered. Unlike PDU Session Establishment, the PDU Session already exists. The Service Request simply restores connectivity so that uplink or downlink user traffic can continue.

## Purpose

- Resume data transfer for an existing PDU Session
- Transition the UE from CM-IDLE to CM-CONNECTED
- Re-establish the N2 signaling connection
- Reactivate user plane resources
- Enable uplink and downlink data transmission with minimal signaling overhead

## Network Functions Involved

- User Equipment (UE)
- gNodeB (gNB)
- Access and Mobility Management Function (AMF)
- Session Management Function (SMF)
- User Plane Function (UPF)

## Step-by-Step Procedure

### Step 1: UE Initiates Service Request

When the UE is in **CM-IDLE** state and needs to send uplink data or respond to pending downlink data, it initiates the Service Request procedure.

The UE establishes an RRC connection with the gNodeB and sends a **NAS Service Request** message encapsulated within the **RRC Connection Setup Complete** message.

**Messages**

- RRC Connection Request
- RRC Connection Setup
- RRC Connection Setup Complete + NAS Service Request

**Interfaces**

- Uu (UE ↔ gNB)

---

### Step 2: gNodeB Sends Initial UE Message to AMF

The gNodeB extracts the NAS Service Request and forwards it to the AMF using an **NGAP Initial UE Message**.

The AMF identifies the UE using the 5G-S-TMSI, validates the security context, and resumes the UE context.

**Messages**

- NGAP Initial UE Message

**Interfaces**

- N2 (gNB ↔ AMF)

---

### Step 3: AMF Requests User Plane Reactivation

The AMF notifies the SMF that the UE has become active again.

The SMF updates the PDU Session context and instructs the UPF to reactivate the user plane tunnel if required.

**Messages**

- Nsmf_PDUSession_UpdateSMContext Request
- Nsmf_PDUSession_UpdateSMContext Response

**Interfaces**

- N11 (AMF ↔ SMF)

---

### Step 4: SMF Updates the UPF

The SMF modifies the existing PFCP session to activate forwarding rules and user plane resources.

No new PDU Session is created; the existing session is simply resumed.

**Messages**

- PFCP Session Modification Request
- PFCP Session Modification Response

**Interfaces**

- N4 (SMF ↔ UPF)

---

### Step 5: AMF Establishes UE Context

After receiving confirmation from the SMF, the AMF sends an **Initial Context Setup Request** to the gNodeB.

The gNodeB allocates radio resources, configures security, and completes the UE context setup.

**Messages**

- NGAP Initial Context Setup Request
- NGAP Initial Context Setup Response

**Interfaces**

- N2 (AMF ↔ gNB)

---

### Step 6: User Plane Becomes Active

Once the UE context has been successfully established, the N3 tunnel becomes active again.

The UE can now exchange uplink and downlink user traffic through the existing PDU Session.

No authentication or registration is performed because the UE is already registered and authenticated.

**Result**

- UE transitions from CM-IDLE to CM-CONNECTED
- Existing PDU Session remains active
- User Plane is restored
- User data transfer resumes

  ## Message Summary

| Step | Sender | Receiver | Message | Purpose |
|------|---------|----------|---------|---------|
| 1 | UE | gNodeB | RRC Connection Request | Request establishment of RRC connection |
| 2 | gNodeB | UE | RRC Connection Setup | Configure radio connection |
| 3 | UE | gNodeB | RRC Connection Setup Complete + NAS Service Request | Carry NAS Service Request to the network |
| 4 | gNodeB | AMF | NGAP Initial UE Message | Forward NAS Service Request |
| 5 | AMF | SMF | Nsmf_PDUSession_UpdateSMContext Request | Notify SMF that UE is active |
| 6 | SMF | UPF | PFCP Session Modification Request | Reactivate user plane forwarding |
| 7 | UPF | SMF | PFCP Session Modification Response | Confirm PFCP modification |
| 8 | SMF | AMF | Nsmf_PDUSession_UpdateSMContext Response | Confirm PDU Session update |
| 9 | AMF | gNodeB | NGAP Initial Context Setup Request | Establish UE context at gNodeB |
| 10 | gNodeB | AMF | NGAP Initial Context Setup Response | Confirm UE context setup |
| 11 | UE | Data Network | User Data | Resume uplink and downlink traffic |

## Interface Summary

| Interface | Network Functions | Protocol | Purpose |
|-----------|-------------------|----------|---------|
| Uu | UE ↔ gNodeB | RRC / NAS | Radio signaling and Service Request |
| N2 | gNodeB ↔ AMF | NGAP | Transport NAS signaling and UE context management |
| N11 | AMF ↔ SMF | HTTP/2 (SBI) | Update PDU Session context |
| N4 | SMF ↔ UPF | PFCP | Reactivate user plane forwarding rules |
| N3 | gNodeB ↔ UPF | GTP-U | Carry user plane traffic after service resumption |

## Key Takeaways

- The Service Request procedure is used to transition the UE from **CM-IDLE** to **CM-CONNECTED**.
- The UE is **already registered, authenticated, and has an active PDU Session** before initiating a Service Request.
- No new Registration procedure is performed during Service Request.
- No new PDU Session is established; the existing PDU Session is resumed.
- The AMF coordinates the Service Request procedure and interacts with the SMF to restore user plane connectivity.
- The SMF updates the UPF using **PFCP Session Modification** to reactivate forwarding rules for the existing PDU Session.
- The gNodeB establishes the UE context and restores radio resources through the **NGAP Initial Context Setup** procedure.
- Once the N3 tunnel is active, uplink and downlink user traffic resumes without changing the UE's IP address.
- Service Request minimizes signaling overhead, enabling fast reconnection after the UE leaves idle mode.
- This procedure is one of the most frequently executed signaling flows in operational 5G networks due to normal UE inactivity timers and periodic transitions between idle and connected states.

### Common Triggers for Service Request

A Service Request may be initiated when:

- The UE has uplink data to transmit.
- The network has buffered downlink data for an idle UE.
- An application becomes active after a period of inactivity.
- Voice, video, or other real-time services require immediate user plane connectivity.
- Background applications resume data transmission after the UE has entered CM-IDLE.

  ## Troubleshooting

The following table lists common issues that may occur during the 5G Service Request procedure, along with their possible causes and recommended troubleshooting actions.

| Issue | Possible Cause | Recommended Action |
|-------|----------------|--------------------|
| Service Request rejected | Invalid NAS message, security context mismatch, or UE context unavailable | Verify NAS signaling, AMF logs, and UE security context |
| RRC Connection Failure | Poor radio conditions, coverage issues, or gNodeB resource limitations | Check radio KPIs, RRC failure counters, and gNodeB alarms |
| NGAP Initial UE Message not received by AMF | N2 interface failure, SCTP connectivity issue, or gNodeB configuration problem | Verify SCTP association, NGAP logs, and N2 transport connectivity |
| Initial Context Setup Failure | Radio bearer configuration failure or gNodeB resource allocation issue | Review NGAP logs, radio bearer configuration, and gNodeB resource utilization |
| PFCP Session Modification Failure | N4 connectivity issue, UPF unreachable, or PFCP session mismatch | Verify N4 connectivity, PFCP messages, and UPF status |
| User Plane traffic not restored | GTP-U tunnel issue, forwarding rule mismatch, or UPF configuration error | Validate N3 GTP-U tunnel, PFCP rules, and UPF forwarding configuration |
| Downlink packets dropped after Service Request | User plane not fully activated or tunnel synchronization issue | Verify PFCP Session Modification success and GTP-U tunnel establishment |
| Service Request timeout | Signaling delay, network congestion, or node overload | Analyze message timers, AMF processing time, and network latency |

### Recommended Troubleshooting Flow

When troubleshooting a failed Service Request, verify the procedure in the following order:

1. Confirm successful RRC Connection establishment between the UE and gNodeB.
2. Verify that the **NGAP Initial UE Message** reaches the AMF.
3. Check whether the AMF successfully processes the NAS Service Request.
4. Verify the **Nsmf_PDUSession_UpdateSMContext** exchange between the AMF and SMF.
5. Confirm successful **PFCP Session Modification** between the SMF and UPF.
6. Verify that the **NGAP Initial Context Setup** procedure completes successfully.
7. Confirm that the N3 GTP-U tunnel is active and user traffic is flowing.
8. Review logs and alarms on the UE, gNodeB, AMF, SMF, and UPF to identify failures.

### KPIs to Monitor

During Service Request troubleshooting, the following KPIs are commonly monitored:

- Service Request Success Rate
- Service Request Failure Rate
- Initial Context Setup Success Rate
- RRC Connection Success Rate
- NGAP Signaling Success Rate
- PFCP Session Modification Success Rate
- User Plane Recovery Time
- Service Request Latency

  ## Wireshark / Packet Capture Analysis

When analyzing the Service Request procedure in a packet capture, the following signaling messages should be observed in chronological order:

| Protocol | Message | Description |
|----------|----------|-------------|
| RRC | RRC Connection Request | UE requests establishment of an RRC connection |
| RRC | RRC Connection Setup | gNodeB configures the radio connection |
| RRC / NAS | RRC Connection Setup Complete + Service Request | UE sends the NAS Service Request message |
| NGAP | Initial UE Message | gNodeB forwards the NAS Service Request to the AMF |
| HTTP/2 (SBI) | Nsmf_PDUSession_UpdateSMContext | AMF requests the SMF to resume the existing PDU Session |
| PFCP | Session Modification Request | SMF updates the UPF forwarding rules |
| PFCP | Session Modification Response | UPF confirms the modification |
| NGAP | Initial Context Setup Request | AMF requests the gNodeB to establish the UE context |
| NGAP | Initial Context Setup Response | gNodeB confirms successful context setup |
| GTP-U | User Data | User plane traffic resumes over the existing tunnel |

### Useful Wireshark Display Filters

```text
ngap
nas-5gs
pfcp
http2
gtp
```

> **Note:** Depending on the deployment, SBI traffic may be encrypted using TLS. In such cases, HTTP/2 messages may not be directly visible without decryption keys.

## References

- 3GPP TS 23.502 – Procedures for the 5G System
- 3GPP TS 24.501 – Non-Access-Stratum (NAS) Protocol for 5GS
- 3GPP TS 38.300 – NR Overall Description
- 3GPP TS 38.331 – NR Radio Resource Control (RRC)
- 3GPP TS 38.413 – NG Application Protocol (NGAP)
- 3GPP TS 29.244 – PFCP Protocol
- 3GPP TS 29.502 – Session Management Services
  ## Related Procedures

- [5G Service-Based Architecture](../Service-Based-Architecture/)
- [5G Registration Procedure](../Registration/)
- [5G PDU Session Establishment](../PDU-Session-Establishment/)

### Recommended Next Reading

- PDU Session Release
- UE Deregistration
- Paging Procedure
- Xn Handover
- N2 Handover
- PDU Session Modification
  
