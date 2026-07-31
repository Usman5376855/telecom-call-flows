
# Case Study 001: S11 Create Bearer Request Rejection Due to PGW FQDN Resolution Mismatch

## Overview

This case study describes a real LTE/EPC troubleshooting scenario where dedicated bearer establishment failed due to incorrect PGW Fully Qualified Domain Name (FQDN) resolution. Although the UE successfully completed the Attach procedure and established the default bearer, the dedicated bearer could not be created because the Serving Gateway (SGW) selected an incorrect PGW destination.

All operator-specific information has been removed to protect customer confidentiality.

---

## Technologies

- LTE EPC
- Huawei Packet Core
- MME
- SGW
- PGW
- DNS
- PCRF
- GTPv2-C
- S11 Interface

---

## Network Architecture
<p align="center">
  <img src="../../Images/Case-Studies/case001-pgw-fqdn-resolution-mismatch.png"
       alt="S11 Create Bearer Failure due to PGW FQDN Resolution Mismatch"
       width="1000">
</p>

<p align="center">
<b>Figure 1.</b> LTE EPC Network Architecture, Dedicated Bearer Failure caused by incorrect PGW FQDN resolution, and successful service restoration after DNS correction.
</p>

## Problem Description

Some LTE subscribers were unable to establish dedicated bearers after successful network attachment.

The following procedures completed successfully:

- Attach Procedure
- Authentication
- Security Mode Control
- Initial Context Setup
- Default EPS Bearer Establishment

However, dedicated bearer creation failed during the Create Bearer procedure.

---

## Customer Impact

Observed behaviour included:

- Dedicated bearer establishment failures
- VoLTE bearer activation failures
- Reduced service quality for affected subscribers
- Increased bearer setup failure KPIs

---

## Initial Investigation

The EPC KPIs showed an abnormal increase in S11 Create Bearer Request failures.

Protocol traces confirmed:

- UE Attach completed successfully.
- Default bearer established successfully.
- PCRF installed PCC rules.
- SGW initiated Create Bearer Request.
- Create Bearer procedure failed.

---

## Root Cause Analysis

Detailed signalling analysis showed that the PGW FQDN configured in the network resolved to an unexpected destination.

As a result:

- SGW selected an incorrect PGW endpoint.
- The Create Bearer Request could not be processed successfully.
- Dedicated bearer establishment failed.

The issue was traced to inconsistent DNS configuration across Packet Core nodes.

---

## Solution

The engineering team performed the following actions:

1. Verified PGW FQDN configuration.
2. Reviewed DNS records.
3. Corrected inconsistent FQDN resolution.
4. Synchronized DNS configuration across EPC nodes.
5. Re-tested dedicated bearer establishment.

---

## Verification

Following the DNS correction:

- Create Bearer Requests completed successfully.
- Dedicated bearer establishment returned to normal.
- VoLTE bearer activation succeeded.
- Bearer setup KPIs recovered.

---

## Lessons Learned

- Always verify DNS consistency when troubleshooting bearer establishment failures.
- Validate FQDN resolution during network commissioning.
- Include DNS verification in EPC health checks.
- Capture S11 signalling before modifying network configuration.
- Compare successful and failed traces to isolate protocol differences.

---

## Key Takeaways

This case demonstrates that not all bearer establishment failures originate from signalling protocol issues. Infrastructure components such as DNS can directly affect EPC signalling behaviour and should always be included in end-to-end troubleshooting.

---

## References

1. 3GPP TS 23.401 – General Packet Radio Service (GPRS) Enhancements for Evolved Universal Terrestrial Radio Access Network (E-UTRAN)

2. 3GPP TS 29.274 – GPRS Tunnelling Protocol Version 2 (GTPv2-C)

3. 3GPP TS 29.212 – Policy and Charging Control (PCC)
