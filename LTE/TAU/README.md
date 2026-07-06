
# LTE Tracking Area Update (TAU)

## Overview

Tracking Area Update (TAU) is an LTE Mobility Management procedure that allows the User Equipment (UE) to inform the Mobility Management Entity (MME) that it has entered a new Tracking Area (TA) or to periodically confirm its presence within the network.

The TAU procedure ensures that the network always maintains an up-to-date location of the UE while it remains in ECM-IDLE state. This enables efficient paging for incoming services, supports seamless mobility across Tracking Areas, and minimizes unnecessary signaling.

Depending on the mobility scenario, the TAU procedure may also be used to update the UE's security context, EPS bearer information, Tracking Area List (TAL), or other mobility-related parameters.

This document explains the LTE Tracking Area Update procedure, signaling sequence, protocol messages, important Information Elements (IEs), timers, common failure scenarios, and troubleshooting considerations based on 3GPP specifications.
