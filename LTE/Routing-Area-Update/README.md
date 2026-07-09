
## Overview

The Routing Area Update (RAU) procedure enables a User Equipment (UE) to inform the Serving GPRS Support Node (SGSN) that it has moved to a new Routing Area (RA) within the GPRS/UMTS Packet Switched (PS) domain.

The procedure ensures that the network maintains the UE's current location information, allowing packet data services to continue without interruption. During the RAU procedure, the SGSN updates the UE's routing information, maintains mobility management context, and coordinates with other core network entities when required.

Depending on the mobility scenario, the RAU procedure may involve only the serving SGSN or require context transfer between different SGSNs.
## Purpose

The Routing Area Update (RAU) procedure is performed to:

- Update the UE's location within the Packet Switched (PS) domain.
- Notify the SGSN when the UE enters a new Routing Area.
- Maintain accurate mobility management information.
- Ensure continued packet data connectivity during mobility.
- Transfer UE context when the serving SGSN changes.
- Update subscriber location information in the HLR when required.
  ## Network Elements

| Network Element | Function |
|----------------|----------|
| UE | Detects Routing Area change and initiates the RAU procedure. |
| UTRAN / GERAN | Provides radio access for Packet Switched services. |
| Serving SGSN | Processes the Routing Area Update and manages UE mobility. |
| New SGSN | Receives the UE context when inter-SGSN mobility occurs. |
| GGSN | Maintains Packet Data Protocol (PDP) contexts for data sessions. |
| HLR | Updates subscriber location information when required. |

## Preconditions

Before a Routing Area Update can be performed, the following conditions must be satisfied:

- The UE is attached to the GPRS Packet Switched domain.
- At least one PDP Context is active or the UE remains PS attached.
- The UE is operating in GERAN or UTRAN coverage.
- A valid Routing Area Identity (RAI) is broadcast by the serving cell.
- The UE detects that the Routing Area has changed or another RAU trigger occurs.

  ## Procedure Flow

### Step 1 – Routing Area Change Detection

The UE continuously monitors the Routing Area Identity (RAI) broadcast by the serving cell. When the UE detects that it has entered a new Routing Area, or another Routing Area Update trigger occurs, it initiates the RAU procedure.

---

### Step 2 – Routing Area Update Request

The UE sends a **Routing Area Update Request** message to the serving SGSN through the GERAN or UTRAN access network.

The request includes the UE's identity, the old Routing Area Identity (RAI), the update type, and other mobility management information required for the procedure.

---

### Step 3 – UE Context Retrieval (If Required)

If the UE has moved to a different SGSN, the new SGSN requests the UE context from the previous SGSN.

The previous SGSN transfers the mobility management and PDP context information, allowing the new SGSN to continue serving the UE without re-establishing existing packet data sessions.

---

### Step 4 – Subscriber Information Update

If required, the new SGSN updates the subscriber's location in the HLR.

The HLR acknowledges the location update and provides the subscriber profile and subscription information to the serving SGSN.

---

### Step 5 – Security Procedures

If required, the SGSN performs authentication and security procedures before completing the Routing Area Update.

These procedures ensure that the UE is authorized to access Packet Switched services.

---

### Step 6 – PDP Context Update

If the serving SGSN changes, the new SGSN updates the GGSN to associate the active PDP contexts with the new serving SGSN.

This ensures that user-plane traffic is correctly routed without requiring the UE to reactivate its PDP contexts.

---

### Step 7 – Routing Area Update Accept

After successfully completing all required mobility, security, and context update procedures, the SGSN sends a **Routing Area Update Accept** message to the UE.

The message confirms successful registration in the new Routing Area and may include an updated Routing Area Identity or temporary identifiers.

---

### Step 8 – Routing Area Update Complete

The UE responds with a **Routing Area Update Complete** message.

The procedure is completed, and the UE continues normal Packet Switched services under the new Routing Area.

