# LTE Service Request

## Overview

The LTE Service Request procedure is a mobility management procedure used to transition a User Equipment (UE) from **ECM-IDLE** to **ECM-CONNECTED** state when the UE needs to send or receive user data.

Unlike the Attach procedure, the Service Request does not create a new EPS session or establish new default bearers. Instead, it resumes existing signaling and user-plane resources, allowing the UE to continue communication using previously established EPS bearer contexts.

The procedure is commonly triggered by uplink data from the UE or by downlink data received from the network, making it one of the most frequently observed procedures in commercial LTE networks.

This document explains the LTE Service Request procedure, signaling sequence, protocol messages, important Information Elements (IEs), timers, common failure scenarios, and troubleshooting considerations based on 3GPP specifications.

## Purpose

The primary objectives of the LTE Service Request procedure are:

- Resume communication for an idle UE without performing a new Attach.
- Transition the UE from ECM-IDLE to ECM-CONNECTED state.
- Re-establish S1 signaling between the UE, eNodeB, and MME.
- Reactivate existing user-plane bearers.
- Enable uplink and downlink packet transmission.
- Reduce signaling overhead while maintaining efficient resource utilization.
## When is Service Request Triggered?

The Service Request procedure may be initiated under the following conditions:

### 1. Mobile Originated Data

The UE has uplink data to transmit while it is in ECM-IDLE state.

### 2. Mobile Terminated Data

The network receives downlink data for an idle UE. The MME pages the UE, and after responding to the page, the UE initiates the Service Request procedure.

### 3. VoLTE Call Establishment

Before SIP signaling and voice bearer activation, the UE performs a Service Request to resume connectivity.

### 4. SMS over NAS

Some SMS procedures require the UE to establish signaling resources before message delivery.

### 5. Other NAS Signaling

The UE may initiate a Service Request before NAS procedures that require an active S1 connection.
> **Engineer Note**
>
> Service Request is one of the most common procedures observed in MME traces. Unlike the Attach procedure, it does not create new EPS bearer contexts. Instead, it restores the existing connection, allowing rapid data transfer while minimizing signaling overhead. In live networks, Service Request procedures are frequently triggered by smartphone applications generating background traffic.





