# SailPoint IIQ — Identity Lifecycle Demo

## Overview

This project documents a simplified IAM lifecycle based on hands-on experience 
with SailPoint IdentityIQ at **BNP Paribas** (via Synetis) — one of Europe's 
largest regulated banking environments.

It covers the full employee lifecycle: Joiner, Mover, Leaver, Leave of Absence, 
and ComeBack scenarios, reflecting real enterprise conditions.

---

## Lifecycle Flow

*(Architecture diagram — coming soon)*

---

## Scenarios Covered

### Joiner
- HR CSV onboarding with identity attribute mapping
- Correlation rules linking accounts to identities
- Role assignment and automated provisioning to target systems

### Mover
- Department or role change triggers access update
- Existing access reviewed and adjusted to new profile

### Leaver
- Access revocation across all provisioned systems
- Account suspension or removal

### Leave of Absence (LOA)
- Temporary access suspension while preserving identity
- Automated workflow triggered by HR event

### ComeBack
- Access restoration after return from leave
- Re-provisioning based on current role and entitlements

### Access Certification
- Manager review campaigns to validate active entitlements
- Least-privilege enforcement in a regulated banking context

---

## My Contribution at BNP Paribas

- Contributed to the configuration of Joiner / Mover / Leaver / LOA / ComeBack workflows in IIQ
- Wrote BeanShell aggregation rules for identity data processing
- Set up CSV onboarding and identity model configuration
- Ran access certification campaigns (mover scenarios)
- Performed unit testing and UAT support
- Customized notification templates (EmailTemplate) using Velocity
- Customized IdentityIQ UI (branding, HTML/CSS) to align with client standards
- Produced technical documentation (admin guides, spec updates)

---

## Technologies

- SailPoint IdentityIQ · BeanShell · XML configuration
- Identity lifecycle (JML) · Access certification · Provisioning
- CSV aggregation · Correlation rules

---

## Note
This project is a simplified representation of real-world IAM implementations 
and is intended to highlight key lifecycle concepts rather than full system complexity.
---

## Current Focus

Upskilling on **SailPoint Identity Security Cloud (ISC)** — 
cloud-native identity governance and lifecycle management.
