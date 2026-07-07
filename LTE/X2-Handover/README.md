
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

Seamless inter-eNB handover using the X2 interface without involving the MME in the radio handover decision. The MME participates only during the Path Switch procedure after the UE has successfully attached to the target eNB.

📖 **Procedure:** [LTE X2 Handover](LTE/X2-Handover/README.md)

![LTE X2 Handover Call Flow](LTE/X2-Handover/x2-handover-call-flow.png)

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

## 1. Measurement Report (UE → Source eNodeB)

The LTE X2 Handover procedure begins when the User Equipment (UE) continuously measures the radio quality of its serving cell and configured neighboring cells. Based on the configured measurement events, the UE sends an **RRC Measurement Report** to the Source eNodeB whenever the defined handover criteria are satisfied.

The Measurement Report provides the Source eNodeB with the radio measurements required to evaluate whether a handover should be initiated. Using this information, the Source eNodeB determines if the target cell offers better radio conditions and whether an X2 Handover is appropriate.

The Measurement Report typically contains:

* Measurement Event (A1, A2, A3, A4, or A5)
* Serving Cell Measurements
* Neighbor Cell Measurements
* Physical Cell Identity (PCI)
* RSRP (Reference Signal Received Power)
* RSRQ (Reference Signal Received Quality)
* Measurement Object Identifier
* Reporting Configuration Identifier

**Purpose**

* Report the radio quality of the serving and neighboring cells.
* Assist the Source eNodeB in mobility decision making.
* Trigger the X2 Handover procedure when handover conditions are met.
* Improve mobility performance while maintaining service continuity.

**Key Point**

The UE only performs radio measurements and reports the results. The actual handover decision is always made by the **Source eNodeB** based on the reported measurements and operator mobility policies.

**Common Troubleshooting**

* Missing or delayed Measurement Reports.
* Incorrect measurement configuration.
* Neighbor cell missing from the neighbor relation table.
* Incorrect PCI configuration.
* Poor RSRP or RSRQ measurements.
* Measurement reports not triggering the expected handover event.

## 2. Handover Request (Source eNodeB → Target eNodeB)

After determining that a handover is required based on the received Measurement Report, the **Source eNodeB** initiates the X2 Handover procedure by sending an **X2AP Handover Request** message to the **Target eNodeB** over the **X2 interface**.

The Handover Request transfers the UE context and bearer information to the Target eNodeB, allowing it to prepare the required radio resources before the UE arrives. The Target eNodeB performs admission control, reserves the necessary resources, and verifies whether it can successfully accept the incoming handover.

The Handover Request message typically contains:

* UE X2AP ID
* Cause
* UE Context Information
* Security Context
* E-RABs to be Setup List
* E-RAB QoS Parameters
* UE Aggregate Maximum Bit Rate (UE-AMBR)
* KeNB Security Key
* Serving PLMN Information
* UE Capability Information
* Source Cell ECGI

**Purpose**

* Request the Target eNodeB to prepare for the incoming handover.
* Transfer the UE context and security information.
* Transfer EPS bearer and QoS information.
* Reserve radio resources in the Target eNodeB.
* Initiate admission control before handover execution.

**Key Point**

The UE is **still connected to the Source eNodeB** during this stage. The Target eNodeB only prepares the required resources and does not communicate directly with the UE until the handover command is issued.

**Common Troubleshooting**

* X2 interface communication failure.
* Target eNodeB rejects the Handover Request.
* Admission control failure due to insufficient radio resources.
* Missing or invalid UE context information.
* Security context mismatch.
* Incorrect E-RAB or QoS parameters.
* Neighbor relation not configured correctly.

## 3. Handover Request Acknowledge (Target eNodeB → Source eNodeB)

After successfully performing admission control and reserving the required radio resources, the **Target eNodeB** sends an **X2AP Handover Request Acknowledge** message to the **Source eNodeB** over the **X2 interface**.

The Handover Request Acknowledge confirms that the Target eNodeB has accepted the handover request and is fully prepared to receive the UE. The message also provides the information required by the Source eNodeB to generate the **RRC Connection Reconfiguration (Handover Command)** that will instruct the UE to move to the target cell.

The Handover Request Acknowledge message typically contains:

* UE X2AP ID
* E-RABs Admitted List
* E-RABs Not Admitted List (if applicable)
* Target-to-Source Transparent Container
* Criticality Diagnostics (optional)

**Purpose**

* Confirm successful admission of the incoming UE.
* Indicate that radio resources have been successfully reserved.
* Provide the Target-to-Source Transparent Container for the handover command.
* Allow the Source eNodeB to proceed with handover execution.

**Key Point**

The **Target-to-Source Transparent Container** carries the target cell's RRC configuration information. The Source eNodeB does not interpret this information; it simply includes the container in the **RRC Connection Reconfiguration (Handover Command)** sent to the UE.

**Common Troubleshooting**

* Handover Request Acknowledge not received by the Source eNodeB.
* Admission control failure due to insufficient target cell resources.
* Missing or invalid Target-to-Source Transparent Container.
* E-RAB admission failure.
* X2 interface timeout or packet loss.
* Target eNodeB unable to allocate the required radio resources.
## 4. RRC Connection Reconfiguration (Handover Command) (Source eNodeB → UE)

After receiving the **Handover Request Acknowledge** from the Target eNodeB, the **Source eNodeB** initiates the handover execution phase by sending an **RRC Connection Reconfiguration** message, commonly referred to as the **Handover Command**, to the UE over the radio interface.

The Handover Command instructs the UE to disconnect from the Source eNodeB and establish a connection with the Target eNodeB. The message contains the Target-to-Source Transparent Container received from the Target eNodeB, which includes all the information the UE requires to access the target cell.

Upon receiving the Handover Command, the UE synchronizes with the target cell, terminates communication with the Source eNodeB, and begins the handover execution process.

The RRC Connection Reconfiguration (Handover Command) typically contains:

* Mobility Control Information
* Target-to-Source Transparent Container
* Target Cell Identity
* Target Cell Frequency Information
* RRC Configuration Parameters
* Security Configuration
* Radio Bearer Configuration

**Purpose**

* Instruct the UE to perform handover to the Target eNodeB.
* Deliver the Target-to-Source Transparent Container.
* Provide the radio configuration required by the target cell.
* Begin the handover execution phase.

**Key Point**

Once the UE receives the **Handover Command**, it immediately leaves the Source eNodeB and attempts to access the Target eNodeB. During this period, there is a brief interruption of user traffic until the UE successfully completes synchronization and the Random Access Procedure in the target cell.

**Common Troubleshooting**

* Handover Command not received by the UE.
* Radio link failure before handover execution.
* Corrupted or invalid Target-to-Source Transparent Container.
* Incorrect target cell configuration.
* UE unable to synchronize with the target frequency.
* Handover preparation timeout.

## 5. Random Access Procedure (UE → Target eNodeB)

After receiving the **RRC Connection Reconfiguration (Handover Command)**, the UE disconnects from the Source eNodeB and synchronizes with the Target eNodeB. To establish communication with the target cell, the UE performs a **Contention-Free Random Access Procedure** over the LTE radio interface.

During this procedure, the UE transmits the dedicated Random Access Preamble assigned by the Target eNodeB during the handover preparation phase. The Target eNodeB responds by allocating uplink resources and completing the synchronization process. Once the Random Access Procedure is successfully completed, the UE establishes the radio connection with the Target eNodeB and becomes the serving cell for subsequent communication.

The Random Access Procedure typically includes:

* Dedicated Random Access Preamble
* Random Access Response (RAR)
* Timing Advance
* Uplink Resource Grant
* Temporary C-RNTI (when applicable)

**Purpose**

* Synchronize the UE with the Target eNodeB.
* Establish the uplink radio connection.
* Complete the radio access portion of the handover.
* Enable the Target eNodeB to begin serving the UE.

**Key Point**

Unlike the contention-based Random Access Procedure used during initial access, the X2 Handover uses a **Contention-Free Random Access Procedure** with a dedicated preamble. This minimizes access delay and significantly improves handover performance.

**Common Troubleshooting**

* Random Access timeout.
* Random Access Response (RAR) not received.
* Incorrect Timing Advance.
* UE unable to synchronize with the Target eNodeB.
* Radio interference causing Random Access failure.
* Handover Failure due to unsuccessful Random Access.

## 6. SN Status Transfer (Source eNodeB → Target eNodeB)

After the UE successfully accesses the Target eNodeB through the **Contention-Free Random Access Procedure**, the **Source eNodeB** sends an **X2AP SN Status Transfer** message to the **Target eNodeB** over the **X2 interface**.

The SN Status Transfer message carries the **PDCP Sequence Number (SN) status** for each active Data Radio Bearer (DRB). This information enables the Target eNodeB to determine which PDCP packets have already been transmitted, acknowledged, or remain pending. By synchronizing the PDCP state between the two eNodeBs, packet loss and duplicate packet delivery are minimized during the handover.

The SN Status Transfer message typically contains:

* UE X2AP ID
* PDCP Sequence Number Status
* Hyper Frame Number (HFN)
* DRB Status Information
* Receive Status of PDCP SDUs

**Purpose**

* Transfer the PDCP sequence number status to the Target eNodeB.
* Maintain user-plane packet continuity during handover.
* Prevent duplicate packet delivery.
* Minimize packet loss for ongoing services such as VoLTE and video streaming.

**Key Point**

The **SN Status Transfer** is an **X2AP** procedure that synchronizes the PDCP state between the Source and Target eNodeBs. It is particularly important for maintaining seamless user-plane continuity during mobility.

**Common Troubleshooting**

* SN Status Transfer message not received.
* PDCP sequence number mismatch.
* Packet duplication after handover.
* Packet loss caused by incomplete PDCP synchronization.
* X2 interface delay affecting status transfer.
* User-plane interruption due to incorrect PDCP state synchronization.

## 6. SN Status Transfer (Source eNodeB → Target eNodeB)

After the UE successfully accesses the Target eNodeB through the **Contention-Free Random Access Procedure**, the **Source eNodeB** sends an **X2AP SN Status Transfer** message to the **Target eNodeB** over the **X2 interface**.

The SN Status Transfer message carries the **PDCP Sequence Number (SN) status** for each active Data Radio Bearer (DRB). This information enables the Target eNodeB to determine which PDCP packets have already been transmitted, acknowledged, or remain pending. By synchronizing the PDCP state between the two eNodeBs, packet loss and duplicate packet delivery are minimized during the handover.

The SN Status Transfer message typically contains:

* UE X2AP ID
* PDCP Sequence Number Status
* Hyper Frame Number (HFN)
* DRB Status Information
* Receive Status of PDCP SDUs

**Purpose**

* Transfer the PDCP sequence number status to the Target eNodeB.
* Maintain user-plane packet continuity during handover.
* Prevent duplicate packet delivery.
* Minimize packet loss for ongoing services such as VoLTE and video streaming.

**Key Point**

The **SN Status Transfer** is an **X2AP** procedure that synchronizes the PDCP state between the Source and Target eNodeBs. It is particularly important for maintaining seamless user-plane continuity during mobility.

**Common Troubleshooting**

* SN Status Transfer message not received.
* PDCP sequence number mismatch.
* Packet duplication after handover.
* Packet loss caused by incomplete PDCP synchronization.
* X2 interface delay affecting status transfer.
* User-plane interruption due to incorrect PDCP state synchronization.

## 8. Modify Bearer Request (MME → SGW)

After receiving the **Path Switch Request** from the Target eNodeB, the **Mobility Management Entity (MME)** updates the user-plane path by sending a **GTPv2-C Modify Bearer Request** message to the **Serving Gateway (SGW)** over the **S11 interface**.

The Modify Bearer Request informs the SGW that the UE has successfully completed the handover and that all downlink user-plane traffic must now be forwarded to the Target eNodeB. The message includes the new downlink tunnel information (F-TEID) received from the Target eNodeB, enabling the SGW to establish the updated GTP-U tunnel.

The Modify Bearer Request message typically contains:

* EPS Bearer ID (EBI)
* Sender F-TEID for Control Plane
* User Plane F-TEID of the Target eNodeB
* Indication Flags (when applicable)
* User Location Information (optional)

**Purpose**

* Update the user-plane path following successful handover.
* Establish the new GTP-U tunnel towards the Target eNodeB.
* Redirect downlink traffic from the Source eNodeB to the Target eNodeB.
* Maintain uninterrupted user-plane communication.

**Key Point**

The **Modify Bearer Request** is the key **GTPv2-C** signaling message that completes the user-plane path switch within the EPC. Until the SGW processes this request, downlink packets may still be directed toward the Source eNodeB.

**Common Troubleshooting**

* Modify Bearer Request not received by the SGW.
* Invalid or missing F-TEID information.
* S11 interface communication failure.
* SGW unable to update the bearer context.
* Incorrect EPS Bearer ID (EBI).
* GTPv2-C timeout or retransmissions.
* Downlink traffic continuing towards the Source eNodeB after handover.

## 9. Modify Bearer Response (SGW → MME)

After successfully updating the bearer context and redirecting the user-plane tunnel towards the Target eNodeB, the **Serving Gateway (SGW)** sends a **GTPv2-C Modify Bearer Response** message to the **Mobility Management Entity (MME)** over the **S11 interface**.

The Modify Bearer Response confirms that the SGW has successfully updated the GTP-U tunnel endpoint for the UE and that all downlink user traffic will now be forwarded to the Target eNodeB. Upon receiving this confirmation, the MME proceeds to complete the Path Switch procedure with the Target eNodeB.

The Modify Bearer Response message typically contains:

* Cause Value
* EPS Bearer Context Status
* Recovery Information (when applicable)
* Protocol Configuration Options (optional)

**Purpose**

* Confirm successful bearer modification in the SGW.
* Notify the MME that the user-plane path has been updated.
* Complete the EPC bearer update procedure.
* Allow the MME to continue the Path Switch procedure.

**Key Point**

The **Modify Bearer Response** confirms that the **user-plane path has been successfully switched** to the Target eNodeB. From this point onward, downlink packets are delivered through the new GTP-U tunnel, completing the user-plane transition.

**Common Troubleshooting**

* Modify Bearer Response timeout.
* SGW returns an unsuccessful Cause value.
* Bearer context update failure.
* GTPv2-C communication failure on the S11 interface.
* Incorrect bearer information in the response.
* Downlink traffic not redirected after successful response.

---

## 10. Path Switch Request Acknowledge (MME → Target eNodeB)

After receiving the **Modify Bearer Response** from the SGW, the **Mobility Management Entity (MME)** sends an **S1AP Path Switch Request Acknowledge** message to the **Target eNodeB** over the **S1-MME interface**.

The Path Switch Request Acknowledge confirms that the EPC has successfully updated the bearer context and switched the user-plane path to the Target eNodeB. The Target eNodeB can now consider the handover fully completed and continue serving the UE without relying on the Source eNodeB for user-plane forwarding.

The Path Switch Request Acknowledge message typically contains:

* MME UE S1AP ID
* eNB UE S1AP ID
* E-RABs Switched List
* E-RAB Release List (when applicable)
* Criticality Diagnostics (optional)

**Purpose**

* Confirm successful completion of the Path Switch procedure.
* Inform the Target eNodeB that the EPC bearer update has completed.
* Complete the control-plane update following handover.
* Allow the Target eNodeB to finalize the mobility procedure.

**Key Point**

The **Path Switch Request Acknowledge** indicates that both the **control plane and user plane** have been successfully updated. At this stage, the Target eNodeB becomes the sole serving eNodeB for the UE.

**Common Troubleshooting**

* Path Switch Request Acknowledge not received by the Target eNodeB.
* S1AP communication failure.
* MME unable to complete the bearer update.
* Incorrect E-RAB information.
* Path Switch procedure timeout.
* Control-plane synchronization failure between the MME and Target eNodeB.

## 11. UE Context Release (Target eNodeB → Source eNodeB)

After receiving the **Path Switch Request Acknowledge** from the MME, the **Target eNodeB** sends an **X2AP UE Context Release** message to the **Source eNodeB** over the **X2 interface**.

The UE Context Release message informs the Source eNodeB that the handover has been completed successfully and that it may safely release all remaining resources associated with the UE. Upon receiving this message, the Source eNodeB removes the UE context, releases the radio resources, and terminates any remaining user-plane forwarding related to the handover.

The UE Context Release message typically contains:

* UE X2AP ID
* Cause Value
* Release Information (when applicable)

**Purpose**

* Release the UE context from the Source eNodeB.
* Free radio and signaling resources.
* Complete the X2 Handover procedure between the two eNodeBs.
* Remove temporary forwarding resources.

**Key Point**

The **Source eNodeB no longer serves the UE** after receiving the UE Context Release message. All signaling and user-plane communication now occur exclusively through the Target eNodeB.

**Common Troubleshooting**

* UE Context Release message not received by the Source eNodeB.
* Source eNodeB unable to remove the UE context.
* X2 interface communication failure.
* Resource leakage caused by incomplete context release.
* Stale UE context remaining in the Source eNodeB.

---

## 12. End Marker (SGW → Source eNodeB) *(Optional)*

Following the successful bearer modification, the **Serving Gateway (SGW)** may send a **GTP-U End Marker** packet to the **Source eNodeB** over the user-plane tunnel.

The End Marker indicates that no additional downlink user packets will be transmitted through the old GTP-U tunnel. It allows the Source eNodeB to flush any remaining buffered packets and complete the transition of user-plane traffic to the Target eNodeB.

The End Marker typically contains:

* GTP-U Header
* Tunnel Endpoint Identifier (TEID)
* End Marker Indication

**Purpose**

* Indicate completion of the old user-plane tunnel.
* Flush any remaining buffered packets.
* Prevent duplicate packet forwarding.
* Finalize the user-plane transition.

**Key Point**

The **End Marker** is a **GTP-U** message and is **optional**. Its use depends on the network implementation and vendor behavior. Some deployments rely on the End Marker to minimize packet loss during handover, while others achieve the same objective through alternative buffering and forwarding mechanisms.

**Common Troubleshooting**

* End Marker not received by the Source eNodeB.
* Buffered packets discarded prematurely.
* Duplicate packet delivery during handover.
* Delayed release of the old GTP-U tunnel.
* Packet loss caused by incomplete user-plane synchronization.


