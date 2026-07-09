
## Overview

Single Radio Voice Call Continuity (SRVCC) is a mobility procedure that enables an ongoing VoLTE call to be seamlessly transferred from the LTE packet-switched domain to a legacy Circuit Switched (CS) network, such as UTRAN (3G) or GERAN (2G), when LTE coverage becomes insufficient.

The procedure preserves voice service continuity by coordinating signaling between the LTE Evolved Packet Core (EPC), IMS Core, MSC Server, and the target legacy radio access network. During the handover, the voice bearer is transferred from the IMS domain to the Circuit Switched domain without requiring the user to re-establish the call.

SRVCC is standardized by 3GPP to ensure uninterrupted voice service during inter-RAT mobility for networks supporting VoLTE.

## Purpose

The SRVCC procedure is performed to:

- Maintain continuity of an active VoLTE call during inter-RAT mobility.
- Transfer an ongoing IMS voice session from LTE to the Circuit Switched domain.
- Minimize voice interruption during LTE coverage loss.
- Preserve the user experience by avoiding call drops.
- Enable seamless mobility between LTE and legacy 2G/3G networks.

- ## Network Elements

| Network Element | Function |
|----------------|----------|
| UE | Maintains the active VoLTE call and performs the SRVCC handover. |
| eNodeB | Detects deteriorating radio conditions and initiates SRVCC. |
| MME | Coordinates the SRVCC procedure within the EPC. |
| MSC Server | Controls the Circuit Switched voice call after handover. |
| IMS Core | Manages the VoLTE session before SRVCC. |
| SGW | Maintains the user-plane bearer during handover preparation. |
| UTRAN / GERAN | Provides the target Circuit Switched radio access network. |

## Preconditions

Before the SRVCC procedure can be performed, the following conditions must be satisfied:

- The UE is registered on the LTE network.
- An active VoLTE call is established through the IMS Core.
- SRVCC is supported by the UE and the network.
- Neighbor relations between LTE and the target UTRAN or GERAN cells are configured.
- The MSC Server and EPC support SRVCC procedures.
- Suitable target 2G or 3G coverage is available.

- ## Procedure Flow

### Step 1 – Active VoLTE Call

The UE is registered on the LTE network and an active VoLTE call is established through the IMS Core. Voice traffic is carried over the LTE packet-switched domain using dedicated EPS bearers.

---

### Step 2 – LTE Coverage Degradation

During the active call, the eNodeB continuously monitors radio measurements reported by the UE. When LTE radio conditions deteriorate and a suitable UTRAN or GERAN target cell is available, the eNodeB determines that SRVCC handover is required.

---

### Step 3 – SRVCC Handover Preparation

The serving eNodeB sends a **Handover Required** message to the MME, indicating that SRVCC is requested.

The MME coordinates the handover by communicating with the MSC Server, which prepares the target Circuit Switched network to receive the ongoing voice call.

---

### Step 4 – Target Circuit Switched Resource Preparation

The MSC Server reserves the required Circuit Switched resources in the target UTRAN or GERAN network.

The target Radio Access Network allocates the necessary radio resources and confirms that it is ready to accept the UE.

---

### Step 5 – IMS Session Transfer

While the target Circuit Switched resources are being prepared, the IMS Core and MSC Server coordinate the transfer of the active voice session from the IMS domain to the Circuit Switched domain.

This ensures that the voice call remains continuous during the radio access transition.

---

### Step 6 – Inter-RAT Handover Execution

The MME instructs the serving eNodeB to execute the SRVCC handover.

The eNodeB sends the handover command to the UE, which synchronizes with the target UTRAN or GERAN cell and establishes the Circuit Switched radio connection.

---

### Step 7 – Voice Call Continuity

After successfully accessing the target Circuit Switched network, the voice bearer is transferred from the LTE packet-switched domain to the Circuit Switched domain.

The user continues the voice call without re-establishing the session.

---

### Step 8 – LTE Resource Release

Once the Circuit Switched call is successfully established, the EPC releases the LTE radio and packet-switched resources associated with the VoLTE session.

The UE remains connected to the legacy network until the voice call ends or normal mobility procedures return the UE to LTE.

## Call Flow Diagram

The following diagram illustrates the signaling flow for the Single Radio Voice Call Continuity (SRVCC) procedure. It shows how an ongoing VoLTE call is transferred from the LTE packet-switched domain to the legacy Circuit Switched network while maintaining voice continuity.

![SRVCC Call Flow](images/srvcc-call-flow.png)

