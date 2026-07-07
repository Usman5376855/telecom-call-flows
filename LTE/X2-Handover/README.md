
# LTE X2 Handover Procedure

## Overview

The LTE X2 Handover procedure enables a User Equipment (UE) to move from one eNodeB to another while maintaining an active connection and ongoing data sessions. Unlike an S1 Handover, the source and target eNodeBs communicate directly over the **X2 interface**, allowing faster mobility with reduced signaling through the core network.

The handover is typically triggered when the target cell provides better radio conditions than the serving cell. During the procedure, the source eNodeB coordinates with the target eNodeB to transfer the UE context, prepare radio resources, and forward buffered data. After the UE successfully accesses the target cell, the Mobility Management Entity (MME) updates the user-plane path through the Serving Gateway (SGW) using the Path Switch procedure.

The X2 Handover procedure minimizes service interruption, preserves ongoing user sessions, and provides seamless mobility for applications such as VoLTE, video streaming, and real-time data services.

---

## Purpose

The LTE X2 Handover procedure allows the network to:

* Maintain uninterrupted connectivity during UE mobility.
* Transfer the UE from the source eNodeB to the target eNodeB.
* Minimize handover interruption time.
* Preserve ongoing voice and data sessions.
* Update the user-plane path after successful handover.
* Improve mobility performance by utilizing the direct X2 interface.

---

## Preconditions

Before the X2 Handover procedure can occur:

* The UE is in **ECM-CONNECTED** state.
* An active RRC connection exists between the UE and the source eNodeB.
* Active EPS bearer(s) are established.
* An operational X2 interface exists between the source and target eNodeBs.
* Neighbor cell relationships have been configured.
* The target cell is capable of accepting the handover.

  ---

## Network Elements Involved

* User Equipment (UE)
* Source eNodeB
* Target eNodeB
* Mobility Management Entity (MME)
* Serving Gateway (SGW)

  ---

## Call Flow

> *(X2 Handover call flow diagram will be added in the next step.)*

---

## Handover Triggers

The LTE X2 Handover procedure may be triggered for several reasons, including:

* Better radio signal quality from a neighboring cell.
* Degradation of the serving cell signal.
* High interference in the serving cell.
* Cell load balancing.
* Capacity optimization.
* Mobility robustness optimization (MRO).
* Coverage optimization.
* High-speed UE mobility.

  ---

## Message Sequence

The following signaling messages describe the complete LTE X2 Handover procedure.

