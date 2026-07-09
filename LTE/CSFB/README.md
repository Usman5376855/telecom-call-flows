
## Procedure Flow

### Step 1 – UE Attached to LTE

The UE is successfully attached to the LTE network through a Combined EPS/IMSI Attach procedure. The MME maintains both EPS mobility management and an SGs association with the MSC Server, enabling the UE to receive Circuit Switched services while remaining camped on LTE.

---

### Step 2 – CS Voice Service Triggered

A Circuit Switched voice service is initiated by either:

- **Mobile Originated (MO):** The UE initiates a voice call.
- **Mobile Terminated (MT):** The MSC Server receives an incoming call for the UE.

The network determines that the requested service requires Circuit Switched Fallback because VoLTE is unavailable, disabled, or not selected.

---

### Step 3 – CSFB Procedure Initiation

The CSFB procedure is initiated according to the call direction:

- For an **MO call**, the UE sends an **Extended Service Request** with the CSFB service type to the MME.
- For an **MT call**, the MSC Server sends an **SGsAP Paging Request** to the MME. The MME pages the UE over LTE, and the UE responds with an **Extended Service Request** indicating CS Fallback.

The MME validates the request and prepares the UE for mobility to the legacy Circuit Switched network.

---

### Step 4 – Target Radio Access Selection

The MME sends an S1-AP **UE Context Modification Request** to the serving eNodeB, indicating that Circuit Switched Fallback is required.

The eNodeB selects the target legacy Radio Access Technology (RAT) based on configured inter-RAT neighbor information and current radio conditions. The target network may be:

- UTRAN (3G)
- GERAN (2G)

---

### Step 5 – LTE to CS Network Mobility

The serving eNodeB transfers the UE from LTE to the selected legacy network using one of the following methods:

- **Redirection**, where the UE is instructed to reselect the target RAT.
- **PS Handover**, if inter-RAT handover is supported.

The UE establishes a radio connection with the target GERAN or UTRAN cell.

---

### Step 6 – Circuit Switched Call Establishment

After successfully accessing the legacy network, the UE performs the required Circuit Switched signaling with the MSC Server.

The MSC Server completes call setup, and the voice call is established over the Circuit Switched domain.

---

### Step 7 – Voice Call in Progress

During the active call:

- Voice traffic is carried through the Circuit Switched network.
- LTE packet data services are suspended or managed according to network capabilities.
- Mobility is controlled by the legacy Radio Access Network until the call is released.

---

### Step 8 – Return to LTE

When the voice call ends, the UE releases the Circuit Switched connection and returns to LTE using the normal mobility procedures.

Depending on the network configuration, the UE may:

- Reselect an LTE cell.
- Perform a Tracking Area Update (TAU), if required.
- Resume EPS packet data services.
