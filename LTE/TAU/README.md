
# LTE Tracking Area Update (TAU)

## Overview

Tracking Area Update (TAU) is an LTE Mobility Management procedure that allows the User Equipment (UE) to inform the Mobility Management Entity (MME) that it has entered a new Tracking Area (TA) or to periodically confirm its presence within the network.

The TAU procedure ensures that the network always maintains an up-to-date location of the UE while it remains in ECM-IDLE state. This enables efficient paging for incoming services, supports seamless mobility across Tracking Areas, and minimizes unnecessary signaling.

Depending on the mobility scenario, the TAU procedure may also be used to update the UE's security context, EPS bearer information, Tracking Area List (TAL), or other mobility-related parameters.

This document explains the LTE Tracking Area Update procedure, signaling sequence, protocol messages, important Information Elements (IEs), timers, common failure scenarios, and troubleshooting considerations based on 3GPP specifications.

## Purpose

The primary objectives of the Tracking Area Update (TAU) procedure are:

- Inform the network that the UE has moved to a new Tracking Area (TA).
- Update the UE's location in the Mobility Management Entity (MME).
- Enable the network to accurately page the UE for incoming services while it is in ECM-IDLE state.
- Refresh the UE's registration through periodic TAU, even when no mobility has occurred.
- Update mobility-related information such as the Tracking Area List (TAL), security context, EPS bearer context, and UE capabilities when required.
- Maintain seamless mobility and service continuity as the UE moves within the LTE network.

  ## Types of TAU

The LTE network supports several types of Tracking Area Update procedures depending on the reason for the update.

| TAU Type | Description |
|----------|-------------|
| Normal TAU | Triggered when the UE enters a Tracking Area that is not included in its current Tracking Area List (TAL). |
| Periodic TAU | Triggered when the T3412 periodic update timer expires, allowing the UE to confirm its presence without changing location. |
| Combined TA/LA Update | Performed when combined EPS/IMSI attach is supported, allowing simultaneous Tracking Area Update (LTE) and Location Area Update (CS domain). |
| MME Change TAU | Occurs when the UE moves into an area served by a different MME, requiring context transfer between MMEs. |

> **Engineer Note**
>
> In operational LTE networks, **Periodic TAU** is one of the most commonly observed TAU procedures because it is driven by the T3412 timer. **Normal TAU** is typically seen when a subscriber crosses Tracking Area boundaries. **MME Change TAU** is especially important during troubleshooting because it involves S10 context transfer and may reveal issues related to MME connectivity, context synchronization, or subscriber data consistency.


