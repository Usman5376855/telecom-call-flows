# 5G EPS Fallback Procedure

## Overview

EPS Fallback (Evolved Packet System Fallback) is a standardized mobility procedure that enables a User Equipment (UE) connected to a 5G Standalone (5G SA) network to temporarily move from NR (New Radio) to LTE when a voice service cannot be provided over the 5G radio access network.

Many mobile operators deploy 5G Standalone initially for high-speed data services while continuing to provide voice services through the existing LTE/EPC infrastructure using VoLTE. In such deployments, the network performs an EPS Fallback whenever the UE initiates or receives a voice call and Voice over New Radio (VoNR) is unavailable or not supported.

During this procedure, the UE is redirected or handed over from the 5G NR cell to an LTE cell. Once connected to LTE, the UE establishes the voice session using IMS over VoLTE through the Evolved Packet Core (EPC). After the voice call ends, the UE may return to the 5G NR network depending on operator policy, coverage, and mobility conditions.

EPS Fallback provides seamless voice continuity while allowing operators to gradually migrate from LTE-based voice services to native VoNR without requiring nationwide VoNR deployment.

---

## Objectives

The primary objectives of EPS Fallback are:

- Ensure uninterrupted voice service for subscribers connected to a 5G Standalone network.
- Reuse the existing LTE/EPC and IMS infrastructure for voice services.
- Provide seamless mobility between 5G NR and LTE.
- Minimize voice call setup delay.
- Maintain service continuity during both Mobile Originated (MO) and Mobile Terminated (MT) voice calls.
- Reduce deployment complexity while operators gradually introduce VoNR.

---

## Why EPS Fallback is Required

Although 5G Standalone introduces a cloud-native core network and significantly enhances mobile broadband capabilities, many commercial networks do not initially deploy Voice over New Radio (VoNR).

There are several reasons for this:

- Existing nationwide VoLTE infrastructure is already mature and widely deployed.
- IMS platforms are optimized for LTE voice services.
- VoNR deployment requires software upgrades across the IMS Core, AMF, gNB, UE, and radio network.
- LTE typically provides broader coverage than early 5G SA deployments.
- Operators prefer a phased migration strategy instead of introducing VoNR across the entire network simultaneously.

As a result, the network temporarily moves the UE to LTE whenever voice services are required.

---

## Typical EPS Fallback Scenario

A common voice call scenario is illustrated below.

```text
UE (Connected to 5G NR)

        │
        │ Voice Call Trigger
        ▼

       gNB

        │
        ▼

       AMF

        │
        │ EPS Fallback Decision
        ▼

      LTE eNB

        │
        ▼

       MME

        │
        ▼

    SGW / PGW

        │
        ▼

       IMS

        │
        ▼

     VoLTE Call
```

The UE initially remains connected to the 5G NR network for data services. When a voice call is initiated or received, the network triggers an EPS Fallback procedure, allowing the UE to move to LTE where the voice session is established using VoLTE. Once the voice call is completed, the UE may return to the 5G NR network based on the operator's mobility policy.

---

## EPS Fallback vs VoNR

| Feature | EPS Fallback | VoNR |
|----------|--------------|------|
| Radio Access | LTE | 5G NR |
| Core Network | EPC | 5GC |
| Voice Technology | VoLTE | IMS over NR |
| Mobility | NR → LTE | Remains on NR |
| Call Setup Delay | Slightly Higher | Lower |
| LTE Dependency | Required | Not Required |
| IMS | Required | Required |
| Typical Deployment | Early 5G SA | Mature 5G SA |

---

## Standards

EPS Fallback is defined by the 3GPP specifications listed below.

| Specification | Description |
|--------------|-------------|
| 3GPP TS 23.502 | Procedures for the 5G System |
| 3GPP TS 23.501 | System Architecture for the 5G System |
| 3GPP TS 24.501 | NAS Signalling Procedures |
| 3GPP TS 38.300 | NR Overall Description |
| 3GPP TS 38.413 | NGAP Signalling |
| 3GPP TS 36.413 | S1AP Signalling |
| 3GPP TS 29.274 | GTPv2-C Procedures |
| 3GPP TS 24.229 | IMS SIP Signalling |

---


## EPS Fallback Call Flow

The following figure illustrates the complete 3GPP-compliant EPS Fallback signalling procedure, showing the interaction between the UE, NG-RAN, 5G Core, EPC, and IMS during the transition from 5G NR to LTE for voice service establishment.

<p align="center">
  <img src="images/eps-fallback-call-flow.png" alt="5G EPS Fallback Call Flow" width="100%">
</p>

The call flow begins when the UE initiates or receives a voice call while connected to the 5G Standalone network. The AMF determines that voice service must be provided through LTE and triggers the EPS Fallback procedure. The UE is handed over or redirected to LTE, where it attaches to the EPC, registers with IMS if required, and establishes the VoLTE session. After the voice call is completed, the UE may return to the 5G NR network according to the operator's mobility policy.

---

# Step-by-Step Procedure

## Step 1 – Voice Call Trigger

### Purpose

The EPS Fallback procedure begins when a User Equipment (UE) connected to a 5G Standalone (5G SA) network requires a voice service that cannot be supported over the current NR connection. This situation typically occurs when the operator has deployed 5G SA for data services while continuing to provide voice services through the existing LTE/EPC network using Voice over LTE (VoLTE). Instead of establishing the voice session directly over NR using Voice over New Radio (VoNR), the network initiates an EPS Fallback procedure to transfer the UE to LTE.

### Trigger Conditions

EPS Fallback may be triggered under the following conditions:

- The UE initiates a Mobile Originated (MO) voice call.
- The UE receives a Mobile Terminated (MT) voice call.
- The serving network does not support VoNR.
- The UE does not support VoNR.
- Operator policy prefers VoLTE for voice services.
- IMS voice service is available only through the LTE/EPC network.

Before initiating the fallback, the UE remains registered with the 5G Core (5GC) and continues using the NR radio access network for data connectivity.

### Network Functions Involved

The following network functions participate during this initial stage of the procedure:

| Network Function | Role |
|------------------|------|
| UE | Initiates or receives the voice call request. |
| gNB | Receives the signalling from the UE and forwards it towards the AMF. |
| AMF | Processes the NAS signalling and determines whether EPS Fallback is required based on UE capabilities, network configuration, and operator policy. |

### Signalling Overview

For a Mobile Originated (MO) voice call, the UE sends NAS signalling towards the AMF through the serving gNB. The signalling is encapsulated within NGAP messages over the N2 interface. During this stage, the AMF evaluates whether the voice service can be established over the current NR connection. If VoNR is unavailable, the AMF prepares to initiate the EPS Fallback procedure.

For a Mobile Terminated (MT) voice call, the IMS network triggers paging towards the UE through the 5G Core. After the UE responds to the paging procedure, the AMF performs the same evaluation and determines whether the UE must be moved to LTE for voice service establishment.

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| Uu | Radio interface between the UE and the gNB. |
| N1 | NAS signalling exchanged between the UE and the AMF. |
| N2 | NGAP signalling exchanged between the gNB and the AMF. |

### Expected Result

At the end of this step, the AMF has received the voice service request and determined that the voice session cannot be established over the current 5G NR connection. The network is now ready to initiate the EPS Fallback procedure by instructing the UE to move from NR to LTE.


---

## Step 2 – Initial UE Message

### Purpose

Once the UE initiates a Mobile Originated (MO) voice call or responds to a Mobile Terminated (MT) paging request, the first signalling message is delivered from the NG-RAN to the Access and Mobility Management Function (AMF). This message informs the 5G Core that the UE requires a signalling connection to process the requested voice service.

The Initial UE Message is the first NGAP message exchanged between the gNB and the AMF for this procedure. It carries the NAS signalling received from the UE together with the necessary radio and location information required by the AMF to identify the subscriber and continue processing the request.

### Procedure

The UE sends a NAS Service Request or Registration-related NAS message to the serving gNB through the NR radio interface (Uu). The gNB encapsulates the NAS PDU inside an **NGAP Initial UE Message** and forwards it to the AMF over the N2 interface.

Along with the NAS message, the gNB includes additional information required by the AMF, such as:

- User Location Information (TAI and CGI)
- RAN UE NGAP ID
- Supported Tracking Area
- Establishment Cause
- NAS PDU
- Cell Identity
- PLMN Information

The AMF receives the Initial UE Message and establishes the NGAP UE context if one does not already exist. It then extracts the NAS signalling and begins processing the voice service request.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| UE | Sends the NAS signalling requesting voice service. |
| gNB | Encapsulates the NAS message inside an NGAP Initial UE Message and forwards it to the AMF. |
| AMF | Creates or updates the UE context and processes the received NAS signalling. |

### Messages Exchanged

| Source | Destination | Message |
|---------|-------------|---------|
| UE | gNB | NAS Service Request / NAS Signalling |
| gNB | AMF | NGAP Initial UE Message |

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| Uu | Radio signalling between the UE and the gNB. |
| N2 | NGAP signalling between the gNB and the AMF. |
| N1 | NAS signalling transported transparently through the gNB. |

### Expected Result

At the completion of this step, the AMF has successfully received the Initial UE Message together with the NAS signalling from the UE. The UE context is available in the 5G Core, allowing the AMF to evaluate subscriber capabilities, operator policies, and network configuration before deciding whether the voice service can continue over NR or whether an EPS Fallback procedure must be initiated.

---

## Step 3 – AMF Determines EPS Fallback

### Purpose

After receiving the Initial UE Message and the associated NAS signalling, the Access and Mobility Management Function (AMF) evaluates whether the requested voice service can be provided over the current 5G NR connection. The AMF considers the UE capabilities, network configuration, subscription information, and operator policies before determining the appropriate voice service procedure.

If Voice over New Radio (VoNR) is supported by both the UE and the network, the voice session can continue over the 5G Standalone network. If VoNR is unavailable or not permitted, the AMF decides to initiate the EPS Fallback procedure and prepares to move the UE from NR to LTE.

### Decision Criteria

The AMF evaluates several factors before making the EPS Fallback decision, including:

- UE support for VoNR.
- Availability of VoNR in the serving NR cell.
- Subscriber voice service profile.
- Operator policy for voice service handling.
- IMS voice capability.
- Availability of an LTE cell for fallback.
- Support for interworking with the EPC.

If one or more of these conditions indicate that voice service cannot be provided over NR, the AMF selects EPS Fallback as the preferred procedure.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| AMF | Evaluates the voice service request and determines whether EPS Fallback is required. |
| gNB | Waits for further instructions from the AMF. |
| UE | Remains connected to the serving NR cell while awaiting the network decision. |

### Internal Processing

During this stage, the AMF performs several internal operations, including:

- Processing the received NAS signalling.
- Checking the UE registration and mobility context.
- Verifying subscriber capabilities and subscription data.
- Applying operator policies for voice service.
- Determining whether EPS Fallback or VoNR should be used.

These operations are internal to the 5G Core and do not yet require additional signalling towards the UE.

### Expected Result

At the end of this step, the AMF has determined that the requested voice service cannot be established over the current 5G NR connection. The AMF is now ready to initiate the EPS Fallback procedure by instructing the gNB to prepare the UE for mobility towards the LTE network.

---

## Step 4 – Downlink NAS Transport (EPS Fallback Command)

### Purpose

After determining that the requested voice service cannot be supported over the current 5G NR connection, the AMF initiates the EPS Fallback procedure by sending signalling towards the serving gNB. This signalling informs the gNB that the UE must be prepared for mobility from NR to LTE so that the voice service can be established through the EPC and IMS network.

The AMF delivers the required NAS information to the gNB using the NGAP **Downlink NAS Transport** message over the N2 interface.

### Procedure

The AMF generates a Downlink NAS Transport message containing the NAS signalling required for the EPS Fallback procedure. The message is transmitted to the serving gNB together with the UE context already established during the previous signalling exchange.

After receiving the Downlink NAS Transport message, the gNB extracts the NAS PDU and prepares the radio procedures required to move the UE from the NR cell to the target LTE cell. Depending on the operator's deployment and network capabilities, the UE may either be redirected to LTE or handed over directly to LTE.

At this stage, the UE is still connected to the 5G NR cell and has not yet changed its serving radio access technology.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| AMF | Sends the Downlink NAS Transport message containing the NAS signalling for EPS Fallback. |
| gNB | Receives the NGAP message, extracts the NAS PDU, and prepares the radio mobility procedure. |
| UE | Continues to maintain the NR connection while awaiting the radio reconfiguration from the gNB. |

### Messages Exchanged

| Source | Destination | Message |
|---------|-------------|---------|
| AMF | gNB | NGAP Downlink NAS Transport |

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| N2 | NGAP signalling between the AMF and the gNB. |

### Expected Result

At the completion of this step, the serving gNB has received the EPS Fallback instruction from the AMF and is ready to initiate the necessary radio procedures to move the UE from the 5G NR cell to the LTE network for voice service establishment.

---

## Step 5 – RRC Reconfiguration

### Purpose

After receiving the EPS Fallback instruction from the AMF, the serving gNB initiates the radio access procedure required to move the UE from the 5G NR cell to an LTE cell. This is achieved by sending an RRC Reconfiguration message to the UE containing the information required to perform the mobility procedure.

The objective of this step is to ensure that the UE can leave the NR radio access network and establish a connection with the target LTE cell while maintaining service continuity.

### Procedure

The gNB prepares the radio configuration based on the target LTE cell selected for the fallback procedure. The target cell may be chosen using neighbour cell measurements, configured mobility parameters, and operator-defined mobility policies.

The gNB then transmits an **RRC Reconfiguration** message to the UE. This message contains the mobility configuration required for the UE to perform the transition from NR to LTE.

Depending on the network deployment and implementation, the UE may perform:

- Inter-RAT handover from NR to LTE, or
- Redirection from NR to LTE.

Both approaches ultimately move the UE to the LTE radio access network where voice service can be established through the EPC.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| gNB | Selects the target LTE cell and sends the RRC Reconfiguration message to the UE. |
| UE | Receives the RRC Reconfiguration message and prepares to move to the LTE radio access network. |
| AMF | Has already completed the EPS Fallback decision and waits for the mobility procedure to complete. |

### Messages Exchanged

| Source | Destination | Message |
|---------|-------------|---------|
| gNB | UE | RRC Reconfiguration |

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| Uu | Radio interface used to transmit the RRC Reconfiguration message between the gNB and the UE. |

### Expected Result

At the completion of this step, the UE has received the radio reconfiguration information required for EPS Fallback and is ready to leave the 5G NR cell. The next stage of the procedure is the actual transition to the LTE radio access network, where the UE establishes connectivity with the target eNB.

---

## Step 6 – NR to LTE Mobility

### Purpose

After receiving the RRC Reconfiguration message from the serving gNB, the UE executes the radio mobility procedure and transitions from the 5G NR cell to the target LTE cell. This step completes the radio access migration required for EPS Fallback and allows the UE to continue the voice service through the LTE/EPC network.

The objective of this stage is to establish a stable LTE radio connection while preserving service continuity and minimizing voice call setup delay.

### Procedure

The UE releases its active NR radio resources and begins the mobility procedure towards the target LTE cell identified by the serving gNB.

The UE synchronizes with the target LTE cell, performs the LTE random access procedure if required, and establishes the LTE Radio Resource Control (RRC) connection with the serving eNB.

Once the LTE radio connection is successfully established, the UE is attached to the LTE radio access network and is ready to continue the mobility procedures required by the Evolved Packet Core (EPC). Depending on the deployment architecture, the UE may perform a Tracking Area Update (TAU) using the N26 interface or execute a complete LTE Attach procedure if context transfer is not available.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| UE | Executes the mobility procedure and establishes the LTE radio connection. |
| gNB | Releases the NR radio resources after the UE leaves the serving NR cell. |
| eNB | Accepts the incoming UE and establishes the LTE radio connection. |

### Messages Exchanged

| Source | Destination | Message |
|---------|-------------|---------|
| UE | eNB | LTE Random Access Procedure |
| UE | eNB | RRC Connection Establishment |

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| Uu (NR) | Final signalling before leaving the NR cell. |
| Uu (LTE) | Establishes the new LTE radio connection with the target eNB. |

### Expected Result

At the completion of this step, the UE has successfully transitioned from the 5G NR radio access network to the LTE radio access network. The LTE radio connection is established, and the UE is ready to perform the required EPC procedures before the VoLTE call can be set up.

---

## Step 7 – LTE Attach / Tracking Area Update (TAU)

### Purpose

After successfully moving from the 5G NR cell to the LTE radio access network, the UE must establish or update its mobility context within the Evolved Packet Core (EPC). This enables the EPC to recognize the UE as being served by LTE and allows the IMS voice service to continue over the LTE network.

The procedure performed at this stage depends on the operator's deployment architecture and whether interworking between the 5G Core and EPC is supported through the N26 interface.

### Procedure

Once the LTE radio connection has been established, the UE communicates with the serving eNB, which forwards the NAS signalling to the Mobility Management Entity (MME).

If the network supports the **N26 interface** and the UE context is successfully transferred from the AMF to the MME, the UE normally performs a **Tracking Area Update (TAU)** procedure. The MME retrieves the UE context from the AMF, allowing the existing mobility and session information to be reused without requiring a complete LTE registration.

If the N26 interface is unavailable or context transfer cannot be completed, the UE performs a **full LTE Attach** procedure. During this process, the MME establishes a new mobility context, performs subscriber authentication if required, updates the HSS, and creates the default EPS bearer through the Serving Gateway (SGW) and PDN Gateway (PGW).

After the mobility procedure is completed successfully, the UE is fully registered with the EPC and is ready to establish the IMS voice session.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| UE | Initiates the TAU or LTE Attach procedure. |
| eNB | Transfers NAS signalling between the UE and the MME. |
| MME | Processes the TAU or Attach procedure and establishes the EPC mobility context. |
| AMF | Transfers the UE context to the MME when N26 interworking is available. |
| HSS | Provides subscriber information when required. |
| SGW | Establishes the user plane bearer. |
| PGW | Provides PDN connectivity and maintains the IP session. |

### Messages Exchanged

| Source | Destination | Message |
|---------|-------------|---------|
| UE | MME | Tracking Area Update Request or Attach Request |
| MME | AMF | Context Request / Context Response (N26, if applicable) |
| MME | HSS | Update Location Request / Answer (when required) |
| MME | SGW | Create Session Request / Response (for Attach) |
| MME | UE | TAU Accept or Attach Accept |

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| S1-MME | Control plane signalling between the eNB and the MME. |
| N26 | Context transfer between the AMF and the MME when supported. |
| S11 | Session management between the MME and the SGW. |
| S6a | Subscriber management between the MME and the HSS. |
| S5/S8 | Connectivity between the SGW and the PGW. |

### Expected Result

At the end of this step, the UE has successfully established its mobility context within the EPC. The LTE connection is fully operational, the required EPS bearer is available, and the network is ready to proceed with IMS signalling to establish the VoLTE voice call.

---

## Step 8 – IMS Registration

### Purpose

After successfully completing the LTE mobility procedure, the UE must be registered with the IP Multimedia Subsystem (IMS) before VoLTE services can be provided. IMS registration allows the network to authenticate the subscriber for multimedia services and enables SIP-based voice call establishment.

In many commercial networks, the UE may already have a valid IMS registration. In such cases, the existing registration is reused and a new registration procedure is not required. If no valid IMS registration exists, the UE performs the IMS registration procedure before initiating or accepting the voice call.

### Procedure

Once the LTE Attach or Tracking Area Update (TAU) procedure has been completed successfully, the UE verifies whether an active IMS registration is available.

If IMS registration is required, the UE establishes SIP signalling with the IMS Core using the dedicated IMS Access Point Name (APN). The IMS Core authenticates the subscriber and creates the IMS registration context.

A typical IMS registration consists of the following SIP message exchange:

1. SIP REGISTER
2. 401 Unauthorized (Authentication Challenge)
3. SIP REGISTER (with authentication credentials)
4. 200 OK

After the IMS registration procedure is completed successfully, the UE is authorized to establish VoLTE voice sessions through the IMS network.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| UE | Initiates IMS registration and processes SIP authentication. |
| P-CSCF | First point of contact for SIP signalling from the UE. |
| I-CSCF | Routes SIP requests within the IMS network. |
| S-CSCF | Performs subscriber registration and session control. |
| HSS | Provides subscriber authentication and service profile information. |

### Messages Exchanged

| Source | Destination | Message |
|---------|-------------|---------|
| UE | P-CSCF | SIP REGISTER |
| IMS Core | UE | 401 Unauthorized |
| UE | P-CSCF | SIP REGISTER (Authenticated) |
| IMS Core | UE | 200 OK |

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| LTE User Plane | Provides IP connectivity between the UE and the IMS network. |
| SIP | Session Initiation Protocol used for IMS signalling. |

### Expected Result

At the completion of this step, the UE has a valid IMS registration and is authorized to use VoLTE services. The IMS Core has successfully authenticated the subscriber and created the necessary registration context. The network is now ready to establish the SIP voice session for the incoming or outgoing call.

---

## Step 9 – VoLTE Call Establishment

### Purpose

After the UE has successfully registered with the IMS network, the voice session can be established using the Voice over LTE (VoLTE) service. At this stage, the IMS Core manages the SIP signalling required to create the end-to-end voice session between the calling and called subscribers.

Whether the call is Mobile Originated (MO) or Mobile Terminated (MT), the IMS network performs session control, negotiates media capabilities using SDP (Session Description Protocol), and establishes the RTP media path for voice communication.

### Procedure

For a Mobile Originated (MO) call, the UE initiates the voice session by sending a SIP INVITE message towards the IMS Core. The SIP INVITE contains the caller identity, destination information, and media capabilities supported by the UE.

The IMS Core processes the SIP INVITE, applies routing and service policies, and forwards the request towards the destination subscriber. During this process, several SIP responses are exchanged to indicate the progress of the call setup.

A typical SIP signalling sequence includes:

1. SIP INVITE
2. 100 Trying
3. 180 Ringing
4. 200 OK
5. ACK

After the SIP three-way handshake is completed, the IMS session is successfully established and both subscribers are ready to exchange voice traffic.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| UE | Initiates or receives the SIP voice session. |
| P-CSCF | First SIP contact point for the UE and forwards SIP messages. |
| I-CSCF | Routes SIP signalling within the IMS network when required. |
| S-CSCF | Performs session control, routing, and service logic. |
| IMS Application Server (optional) | Provides supplementary voice services such as call forwarding, voicemail, or conferencing. |

### Messages Exchanged

| Source | Destination | Message |
|---------|-------------|---------|
| UE | IMS Core | SIP INVITE |
| IMS Core | UE | 100 Trying |
| IMS Core | UE | 180 Ringing |
| IMS Core | UE | 200 OK |
| UE | IMS Core | ACK |

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| SIP | Session control signalling between the UE and the IMS Core. |
| LTE User Plane | Provides IP connectivity for IMS signalling and media transport. |

### Expected Result

At the completion of this step, the SIP session has been successfully established between the calling and called subscribers. The IMS Core has completed the call setup procedure, negotiated the media parameters, and prepared the network for real-time voice transmission.

---

## Step 10 – RTP Voice Session

### Purpose

After the SIP session has been successfully established, the IMS Core authorizes the media session and the voice traffic begins. Unlike the previous signalling procedures, this stage carries the actual voice packets exchanged between the calling and called subscribers.

The voice media is transported using the Real-time Transport Protocol (RTP), while the Real-time Transport Control Protocol (RTCP) is used to monitor media quality and transmission statistics throughout the call.

### Procedure

Following the successful exchange of the SIP INVITE, 100 Trying, 180 Ringing, 200 OK, and ACK messages, both UEs begin transmitting RTP packets through the LTE/EPC network.

The voice packets traverse the LTE radio interface, the Evolved Packet Core (EPC), and the IMS network before reaching the destination subscriber. During the session, the LTE network maintains the required Quality of Service (QoS) to ensure low latency, minimal packet loss, and high voice quality.

RTCP packets are exchanged periodically to provide feedback on packet delay, jitter, packet loss, and overall media quality. This information enables the network and endpoints to monitor and optimize voice performance during the active call.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| UE | Encodes, transmits, and receives RTP voice packets. |
| eNB | Provides LTE radio connectivity and schedules user-plane traffic. |
| SGW | Forwards user-plane packets within the EPC. |
| PGW | Provides IP connectivity between the EPC and external IMS services. |
| IMS Core | Controls the session while the media flows between the communicating endpoints. |

### Media Transport

The voice session is transported using the following protocols:

| Protocol | Purpose |
|----------|---------|
| RTP | Carries the real-time voice packets. |
| RTCP | Monitors media quality and transmission statistics. |
| UDP | Transport protocol used by RTP and RTCP. |
| IP | Provides end-to-end packet routing across the network. |

### Quality of Service (QoS)

To ensure high-quality voice communication, the LTE network allocates appropriate QoS resources for the VoLTE session. These QoS parameters provide:

- Low end-to-end latency.
- Minimal packet loss.
- Low jitter.
- Prioritized scheduling of voice traffic.
- Consistent voice quality throughout the call.

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| LTE Uu | Radio user-plane interface between the UE and the eNB. |
| S1-U | User-plane interface between the eNB and the SGW. |
| S5/S8 | User-plane interface between the SGW and the PGW. |
| SGi | Interface connecting the EPC to external IMS services. |

### Expected Result

At the completion of this step, the voice conversation is fully established. RTP packets are exchanged continuously between the communicating subscribers while the LTE network maintains the required QoS to provide a stable and high-quality VoLTE voice service until one of the users terminates the call.

---

## Step 11 – Voice Call Release

### Purpose

When either the calling or called subscriber terminates the voice conversation, the IMS network initiates the call release procedure. During this stage, the SIP session is released, the media stream is terminated, and the LTE network frees the radio and core network resources that were allocated for the VoLTE call.

The objective of this step is to gracefully terminate the voice session while ensuring that all network resources associated with the call are properly released.

### Procedure

When one of the subscribers ends the call, the UE sends a SIP **BYE** message towards the IMS Core. The IMS network forwards the request to the remote subscriber and terminates the active SIP session.

The receiving UE responds with a **200 OK** message to acknowledge the successful release of the session.

After the SIP session is released:

- RTP and RTCP media transmission stops.
- The dedicated VoLTE bearer used for voice traffic is released.
- Temporary radio resources allocated for the call are removed.
- The UE remains attached to the LTE network unless further mobility procedures are initiated.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| UE | Initiates or receives the SIP BYE message and terminates the voice session. |
| P-CSCF | Forwards SIP session release signalling. |
| I-CSCF | Routes SIP signalling when required. |
| S-CSCF | Controls the release of the IMS session. |
| EPC | Releases dedicated bearer resources associated with the VoLTE call. |

### Messages Exchanged

| Source | Destination | Message |
|---------|-------------|---------|
| UE | IMS Core | SIP BYE |
| IMS Core | UE | 200 OK |

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| SIP | Releases the IMS voice session. |
| LTE User Plane | Stops RTP media transmission after the session ends. |

### Expected Result

At the completion of this step, the VoLTE call has been successfully terminated. All SIP signalling has completed, the RTP media session has ended, and the dedicated resources allocated for the voice service have been released. The UE remains connected to the LTE network and is ready for subsequent mobility procedures.

---

## Step 12 – Return to 5G NR

### Purpose

After the VoLTE call has been successfully terminated, the UE continues to operate on the LTE network. Depending on the operator's mobility policy, radio conditions, and network deployment, the UE may remain on LTE for a period of time or be moved back to the 5G NR network to resume 5G data services.

The objective of this step is to restore the UE to its preferred radio access technology while maintaining service continuity and optimizing network resource utilization.

### Procedure

Once the dedicated VoLTE bearer has been released, the UE remains attached to the LTE network using the default EPS bearer. The LTE eNB continues to serve the UE for both mobility management and packet data services.

The UE continuously performs neighbor cell measurements according to the configured mobility parameters. If a suitable 5G NR cell is detected and the operator's mobility policy allows a return to NR, the network initiates the mobility procedure.

The transition back to 5G may occur through:

- Idle mode cell reselection from LTE to NR.
- Connected mode mobility procedures, depending on the deployment and supported features.
- Operator-specific mobility optimization policies.

Once the UE successfully reconnects to the 5G NR network, packet data services continue through the 5G Core (5GC), restoring the subscriber to normal 5G Standalone operation.

### Network Functions Involved

| Network Function | Role |
|------------------|------|
| UE | Performs neighbor cell measurements and returns to the 5G NR network when appropriate. |
| eNB | Continues serving the UE while on LTE and supports mobility towards NR. |
| gNB | Accepts the returning UE and re-establishes NR connectivity. |
| AMF | Resumes mobility management after the UE reconnects to the 5G Core. |

### Mobility Considerations

The return to 5G NR depends on several factors, including:

- Availability of NR coverage.
- Operator mobility configuration.
- Signal quality measurements.
- UE mobility state.
- Network load balancing policies.

Some operators immediately return the UE to NR after the call, while others intentionally keep the UE on LTE for a configurable period to reduce unnecessary inter-RAT mobility.

### Interfaces Used

| Interface | Purpose |
|-----------|---------|
| LTE Uu | Maintains LTE connectivity until mobility occurs. |
| NR Uu | Establishes the new NR radio connection after mobility. |
| N2 | Restores signalling between the gNB and the AMF after the UE returns to the 5GC. |

### Expected Result

At the completion of this step, the EPS Fallback procedure has finished successfully. The voice session has been completed over the LTE/EPC network, and the UE has either returned to the 5G NR network or remains on LTE according to the operator's mobility policy. The subscriber is ready for subsequent voice or data services.

---

# Message Sequence Summary

The following table summarizes the major signalling messages exchanged during the EPS Fallback procedure.

| Step | Source | Destination | Protocol | Message |
|------|--------|-------------|----------|---------|
| 1 | UE | gNB | NAS | Voice Service Request |
| 2 | gNB | AMF | NGAP | Initial UE Message |
| 3 | AMF | gNB | NGAP | Downlink NAS Transport |
| 4 | gNB | UE | RRC | RRC Reconfiguration |
| 5 | UE | eNB | LTE RRC | Random Access / RRC Connection |
| 6 | UE | MME | NAS | Tracking Area Update Request or Attach Request |
| 7 | MME | AMF | N26 | UE Context Transfer (if supported) |
| 8 | MME | UE | NAS | TAU Accept or Attach Accept |
| 9 | UE | IMS | SIP | REGISTER (if required) |
| 10 | IMS | UE | SIP | 200 OK |
| 11 | UE | IMS | SIP | INVITE |
| 12 | IMS | UE | SIP | 100 Trying |
| 13 | IMS | UE | SIP | 180 Ringing |
| 14 | IMS | UE | SIP | 200 OK |
| 15 | UE | IMS | SIP | ACK |
| 16 | UE ↔ Remote UE | RTP | Voice Media |
| 17 | UE | IMS | SIP | BYE |
| 18 | IMS | UE | SIP | 200 OK |
| 19 | UE | gNB | Mobility | Return to NR (Operator Policy) |

---

# Network Functions

The following network functions participate in the EPS Fallback procedure. Each component has a specific responsibility to ensure that the UE can successfully move from the 5G Standalone network to the LTE/EPC network and establish the VoLTE voice session.

| Network Function | Role in EPS Fallback |
|------------------|----------------------|
| **UE (User Equipment)** | Initiates or receives the voice call, performs the mobility procedure from NR to LTE, registers with IMS if required, establishes the VoLTE call, exchanges RTP voice packets, and returns to the 5G NR network after the call if permitted by the operator. |
| **gNB (Next Generation NodeB)** | Provides 5G NR radio access, forwards NAS signalling to the AMF using NGAP, receives the EPS Fallback instruction, sends the RRC Reconfiguration message, and initiates the mobility towards LTE. |
| **AMF (Access and Mobility Management Function)** | Processes NAS signalling, evaluates subscriber capabilities and operator policies, determines whether EPS Fallback is required, coordinates mobility, and transfers the UE context to the MME when N26 interworking is supported. |
| **eNB (Evolved NodeB)** | Provides LTE radio access after the UE leaves the NR cell, establishes the LTE RRC connection, and forwards NAS signalling to the MME through the S1-MME interface. |
| **MME (Mobility Management Entity)** | Manages LTE mobility procedures, processes the Tracking Area Update (TAU) or LTE Attach procedure, retrieves the UE context from the AMF when N26 is available, and coordinates EPC signalling. |
| **HSS (Home Subscriber Server)** | Stores subscriber information, authentication data, and service profiles used during LTE Attach or IMS registration when required. |
| **SGW (Serving Gateway)** | Handles user-plane packet forwarding within the EPC and manages the EPS bearer associated with the VoLTE session. |
| **PGW (PDN Gateway)** | Provides connectivity between the EPC and external IP networks, maintains the UE's IP session, and routes traffic towards the IMS network. |
| **IMS Core** | Authenticates IMS subscribers, processes SIP signalling, establishes and manages the VoLTE call, and releases the voice session when the call ends. |

## Summary

During the EPS Fallback procedure, the 5G Core manages the decision to move the UE from NR to LTE, the EPC provides mobility and packet data connectivity after the transition, and the IMS Core establishes the VoLTE voice session. Together, these network functions ensure uninterrupted voice service while allowing the subscriber to continue using a 5G Standalone network for data services whenever possible.

---

# Interfaces Used

The EPS Fallback procedure involves multiple control-plane and user-plane interfaces across the 5G Core (5GC), Evolved Packet Core (EPC), LTE/NR radio access networks, and the IMS network. These interfaces enable signalling exchange, mobility management, bearer establishment, and voice media transport.

| Interface | Network Elements | Protocol | Purpose |
|-----------|------------------|----------|---------|
| **Uu (NR)** | UE ↔ gNB | NR RRC | Carries NR radio signalling before the fallback procedure begins. |
| **Uu (LTE)** | UE ↔ eNB | LTE RRC | Provides LTE radio connectivity after the UE moves from NR to LTE. |
| **N1** | UE ↔ AMF | NAS | Transports NAS signalling between the UE and the 5G Core. |
| **N2** | gNB ↔ AMF | NGAP | Carries signalling between the NG-RAN and the AMF, including the Initial UE Message and Downlink NAS Transport. |
| **N26** | AMF ↔ MME | 5GC/EPC Interworking | Transfers UE mobility context between the 5GC and EPC when supported. |
| **S1-MME** | eNB ↔ MME | S1AP | Exchanges LTE control-plane signalling during TAU or Attach. |
| **S11** | MME ↔ SGW | GTPv2-C | Creates, modifies, and releases EPS bearers within the EPC. |
| **S6a** | MME ↔ HSS | Diameter | Retrieves subscriber authentication data and mobility information. |
| **S5/S8** | SGW ↔ PGW | GTPv2-C / GTP-U | Provides user-plane connectivity between the Serving Gateway and PDN Gateway. |
| **SGi** | PGW ↔ IMS / External IP Network | IP | Connects the EPC to external IMS services and IP networks. |
| **SIP** | UE ↔ IMS Core | SIP | Performs IMS registration and VoLTE call establishment and release. |
| **RTP** | UE ↔ Remote UE | RTP | Carries the real-time voice media during the active call. |
| **RTCP** | UE ↔ Remote UE | RTCP | Reports media quality statistics such as packet loss, jitter, and latency. |

## Interface Summary

During the initial phase of the procedure, the UE communicates with the AMF using the **N1** and **N2** interfaces while remaining connected to the 5G NR radio access network over **Uu (NR)**. After the AMF initiates EPS Fallback, the UE transitions to LTE and begins using **Uu (LTE)** and **S1-MME** for LTE mobility procedures.

Within the EPC, the **S11**, **S6a**, and **S5/S8** interfaces establish the subscriber's mobility context and packet data connectivity. Once LTE connectivity is available, the UE communicates with the IMS Core using **SIP** signalling to establish the VoLTE call, while the actual voice media is transported using **RTP** and monitored through **RTCP**.

---

# Failure Scenarios

Although EPS Fallback is a standardized procedure, several conditions may prevent the UE from successfully moving from the 5G NR network to LTE or completing the VoLTE call establishment. The following table summarizes some of the most common failure scenarios observed in commercial mobile networks.

| Failure Scenario | Description | Possible Cause |
|------------------|-------------|----------------|
| **VoNR Not Supported** | The network determines that voice service cannot be provided over the current NR connection. | VoNR is not deployed, disabled, or unsupported by the UE. |
| **EPS Fallback Decision Failure** | The AMF cannot initiate the fallback procedure. | Invalid subscriber profile, unsupported configuration, or policy restrictions. |
| **NGAP Signalling Failure** | The Downlink NAS Transport or related NGAP signalling is unsuccessful. | N2 signalling failure, gNB software issue, or AMF communication problem. |
| **RRC Mobility Failure** | The UE cannot complete the transition from NR to LTE. | Poor radio conditions, missing neighbor relation, or incorrect mobility configuration. |
| **LTE Cell Unavailable** | No suitable LTE cell is available for fallback. | Coverage issue, neighboring LTE cell outage, or mobility restrictions. |
| **Tracking Area Update (TAU) Failure** | The UE cannot update its mobility context in the EPC. | Invalid Tracking Area, authentication failure, or MME rejection. |
| **LTE Attach Failure** | The UE fails to register with the EPC. | Subscriber restrictions, authentication failure, or EPC signalling problems. |
| **N26 Context Transfer Failure** | UE context cannot be transferred between the AMF and MME. | N26 interface unavailable, context mismatch, or interoperability issue. |
| **IMS Registration Failure** | The UE cannot register with the IMS network. | IMS configuration error, authentication failure, or IP connectivity issue. |
| **SIP Session Establishment Failure** | The VoLTE call cannot be established. | SIP timeout, routing failure, codec negotiation issue, or IMS service problem. |
| **Dedicated Bearer Establishment Failure** | The required bearer for VoLTE cannot be created. | EPC resource limitation, QoS configuration issue, or bearer setup failure. |
| **Voice Quality Degradation** | The call is established but voice quality is poor. | Packet loss, excessive delay, jitter, congestion, or radio interference. |

## Operational Considerations

When troubleshooting EPS Fallback issues, engineers should identify the stage at which the procedure fails before analyzing logs. The signalling sequence described in the previous sections provides a structured approach for isolating the problem.

For example:

- If the UE never leaves the NR cell, investigate the AMF decision, NGAP signalling, and RRC mobility procedures.
- If the UE reaches LTE but the call is not established, analyze the TAU/Attach procedure, IMS registration, and SIP signalling.
- If the call is established but users report poor voice quality, investigate RTP media flow, QoS configuration, radio conditions, and EPC user-plane performance.

By correlating signalling traces from the UE, gNB, AMF, eNB, MME, EPC, and IMS Core, engineers can efficiently identify the root cause and minimize service disruption.

---

# Troubleshooting Guide

When an EPS Fallback procedure fails, the objective is to identify the exact stage where the signalling stops. Since the procedure spans the 5G Core (5GC), Evolved Packet Core (EPC), LTE/NR Radio Access Networks, and the IMS Core, troubleshooting should follow the signalling sequence described in this document.

The following table provides common investigation points for each stage of the procedure.

| Stage | Components to Verify | What to Check |
|------|----------------------|---------------|
| Voice Call Trigger | UE, gNB | Verify that the UE initiates or receives the voice call request correctly. Confirm UE capabilities and subscriber provisioning. |
| AMF Decision | AMF | Verify that the AMF selects EPS Fallback according to subscriber profile, UE capability, and operator policy. |
| NGAP Signalling | gNB, AMF | Confirm successful exchange of Initial UE Message, Downlink NAS Transport, and related NGAP signalling. |
| RRC Mobility | UE, gNB | Verify that the RRC Reconfiguration message is transmitted successfully and that the UE executes the NR-to-LTE mobility procedure. |
| LTE Access | UE, eNB | Confirm successful LTE Random Access and RRC Connection Establishment. |
| TAU / Attach | eNB, MME | Verify successful Tracking Area Update or LTE Attach. Check authentication results and mobility management procedures. |
| N26 Context Transfer | AMF, MME | Confirm successful UE Context Request and Context Response if N26 interworking is deployed. |
| EPC Bearer Setup | MME, SGW, PGW | Verify successful bearer creation, IP address allocation, and session establishment. |
| IMS Registration | UE, IMS Core | Verify successful SIP REGISTER procedure and subscriber authentication. |
| SIP Call Setup | UE, IMS Core | Confirm successful SIP INVITE, 100 Trying, 180 Ringing, 200 OK, and ACK message exchange. |
| RTP Voice Session | UE, EPC | Verify that RTP packets are exchanged correctly and that QoS parameters are maintained throughout the call. |
| Call Release | UE, IMS Core | Confirm successful SIP BYE and 200 OK exchange and verify that bearer resources are released. |

## Recommended Trace Collection

For efficient troubleshooting, collect traces from all network domains involved in the procedure.

| Network Element | Recommended Trace |
|-----------------|-------------------|
| UE | NAS, RRC, SIP, and application logs |
| gNB | NGAP signalling, RRC messages, and radio events |
| AMF | NAS processing, NGAP signalling, and mobility decisions |
| eNB | S1AP signalling, LTE RRC messages, and mobility events |
| MME | TAU, Attach, authentication, bearer management, and S11 signalling |
| SGW / PGW | GTP-C, GTP-U, bearer creation, and user-plane statistics |
| IMS Core | SIP signalling, registration procedures, and session control logs |
| HSS | Subscriber authentication and profile retrieval logs |

## Best Practices

When analyzing an EPS Fallback issue:

- Verify that the UE and network both support EPS Fallback.
- Confirm that VoNR is unavailable or intentionally disabled, requiring fallback to LTE.
- Follow the signalling sequence step by step rather than troubleshooting multiple stages simultaneously.
- Correlate timestamps across UE, RAN, Core, EPC, and IMS logs.
- Verify that the UE successfully transitions to LTE before investigating IMS signalling.
- Confirm successful TAU or LTE Attach before analyzing SIP messages.
- Validate RTP media flow separately from SIP signalling when investigating voice quality issues.
- Compare successful and failed traces to identify missing or unexpected signalling messages.

Following a structured troubleshooting approach significantly reduces fault isolation time and improves the accuracy of root cause analysis.

---

# References

The EPS Fallback procedure described in this document is based on the following 3GPP technical specifications and industry standards.

| Specification | Title | Description |
|--------------|-------|-------------|
| **3GPP TS 23.501** | System Architecture for the 5G System (5GS) | Defines the overall 5G Core architecture, network functions, and service framework. |
| **3GPP TS 23.502** | Procedures for the 5G System (5GS) | Defines the signalling procedures for Registration, Mobility, PDU Sessions, and EPS Fallback. |
| **3GPP TS 24.501** | Non-Access-Stratum (NAS) Protocol for the 5G System | Specifies NAS signalling between the UE and the AMF. |
| **3GPP TS 38.300** | NR and NG-RAN Overall Description | Provides the overall architecture and operation of the 5G Radio Access Network. |
| **3GPP TS 38.331** | NR Radio Resource Control (RRC) Protocol | Defines NR RRC procedures, including radio mobility and reconfiguration. |
| **3GPP TS 38.413** | NGAP (NG Application Protocol) | Defines signalling between the gNB and the AMF over the N2 interface. |
| **3GPP TS 36.300** | E-UTRAN Overall Description | Describes the LTE radio access network architecture. |
| **3GPP TS 36.331** | E-UTRA Radio Resource Control (RRC) Protocol | Defines LTE RRC procedures used after the UE moves to LTE. |
| **3GPP TS 36.413** | S1 Application Protocol (S1AP) | Defines signalling between the eNB and the MME. |
| **3GPP TS 29.274** | GTPv2-C Protocol | Specifies control-plane signalling between EPC nodes for session management. |
| **3GPP TS 29.060** | GPRS Tunnelling Protocol (GTP) | Defines user-plane packet transport within the EPC. |
| **3GPP TS 29.272** | Mobility Management Entity (MME) and HSS Diameter Interfaces | Specifies Diameter procedures over the S6a interface. |
| **3GPP TS 24.229** | IP Multimedia Call Control Protocol | Defines SIP-based IMS registration and VoLTE call establishment procedures. |
| **RFC 3261** | SIP: Session Initiation Protocol | Defines the SIP signalling protocol used by IMS. |
| **RFC 3550** | RTP: A Transport Protocol for Real-Time Applications | Defines RTP and RTCP for real-time voice transport. |

## Additional Reading

Readers interested in related 5G Core procedures are encouraged to review the accompanying documents in this repository, including Registration, PDU Session Establishment, Service Request, N2 Handover, UE Context Release, and UE Deregistration. Together, these procedures provide a comprehensive understanding of signalling and mobility within the 5G System.

---

# Related Procedures

The EPS Fallback procedure is closely related to several other LTE and 5G Core signalling procedures available in this repository. Reviewing these documents provides a broader understanding of mobility management, session management, and voice service continuity across the 5G and LTE networks.

| Procedure | Description |
|-----------|-------------|
| **5G Registration** | Initial UE registration with the 5G Core (5GC), including authentication, security establishment, and registration completion. |
| **PDU Session Establishment** | Establishes the user-plane connectivity between the UE and the Data Network (DN) through the 5G Core. |
| **Service Request** | Reactivates signalling and user-plane resources when the UE returns from Idle mode. |
| **N2 Handover** | Connected-mode mobility procedure between gNBs while maintaining service continuity within the 5G Core. |
| **UE Context Release** | Releases UE signalling resources while preserving the UE registration context within the 5GC. |
| **UE Deregistration** | Removes the UE registration and mobility context from the 5G Core. |
| **LTE Tracking Area Update (TAU)** | Updates the UE location within the LTE/EPC network after mobility. |
| **LTE Attach Procedure** | Registers the UE with the Evolved Packet Core (EPC) and establishes the default EPS bearer. |
| **VoLTE Call Flow** *(Future)* | Describes end-to-end VoLTE call establishment using the LTE/EPC and IMS networks. |
| **Voice over New Radio (VoNR)** *(Future)* | Explains native voice service establishment over the 5G Standalone network without EPS Fallback. |

## Learning Path

For readers who are new to 5G Core signalling, the recommended learning sequence is:

1. 5G Registration
2. PDU Session Establishment
3. Service Request
4. N2 Handover
5. EPS Fallback
6. UE Context Release
7. UE Deregistration

Following this sequence provides a structured understanding of how a UE registers with the network, establishes data connectivity, performs mobility, falls back to LTE for voice services when required, and finally releases or removes its registration context.
