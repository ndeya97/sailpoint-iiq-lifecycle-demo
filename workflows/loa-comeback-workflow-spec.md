# LOA & ComeBack Workflows — Technical Specification

> Directly implemented at BNP Paribas (via Synetis, Oct 2023 – Feb 2024)

## Context
In a regulated banking environment, employees may temporarily leave 
(parental leave, medical leave, sabbatical) without losing their identity 
in the system. The LOA/ComeBack workflow pair handles this lifecycle 
edge case while maintaining compliance and audit trail integrity.

---

## Part 1 — Leave of Absence (LOA)

### Trigger
HR CSV update: specific field indicates LOA status  
(e.g. `employment_status = LOA`)  
Detected during nightly aggregation run.

### Steps

**1. Identity flag**
- Set custom attribute `isOnLeave = true` on the IIQ Identity object
- Preserve identity — do NOT delete or fully deprovision
- Log event timestamp in audit trail

**2. Access suspension**
- Disable all linked accounts (AD, LDAP, application accounts)
- Suspend active role assignments without removing them
- Access entitlements are frozen, not revoked

**3. Certification handling**
- Pause any pending access certification tasks for this identity
- Manager notified of LOA status via EmailTemplate (Velocity)

**4. Key difference from Leaver**
- Accounts are **suspended**, not deleted
- Role assignments are **preserved**, not revoked
- Identity remains fully intact for ComeBack reactivation

### EmailTemplate (Velocity) — LOA notification
```xml
<EmailTemplate name="LOA Notification">
  <Body>
    Dear $manager.firstname,

    This is to inform you that $identity.firstname $identity.lastname
    is now on Leave of Absence effective $event.date.

    Their access has been temporarily suspended.
    No action is required from you at this time.

    Regards,
    IAM Team
  </Body>
</EmailTemplate>
```

---

## Part 2 — ComeBack

### Trigger
HR CSV update: `employment_status` returns to `active`  
Detected during nightly aggregation run.

### Steps

**1. Identity reactivation**
- Set `isOnLeave = false` on the Identity object
- Reactivate all previously suspended accounts

**2. Access restoration**
- Re-enable frozen role assignments
- Re-provision entitlements based on current role profile
- If role has changed during LOA period → trigger Mover logic

**3. Access review**
- Optional certification campaign triggered for manager review
- Validates that restored access still matches least-privilege policy

**4. Notification**
- Manager and employee notified via EmailTemplate (Velocity)

### EmailTemplate (Velocity) — ComeBack notification
```xml
<EmailTemplate name="ComeBack Notification">
  <Body>
    Dear $manager.firstname,

    $identity.firstname $identity.lastname has returned from 
    Leave of Absence effective $event.date.

    Their access has been restored based on their current role profile.
    Please review their entitlements if any role changes occurred 
    during their absence.

    Regards,
    IAM Team
  </Body>
</EmailTemplate>
```

---

## Key Design Decisions

| Decision | Reason |
|---|---|
| Suspend, don't revoke | Faster ComeBack — no full reprovisioning needed |
| Preserve role assignments | Avoids access gaps if return is quick |
| Trigger Mover if role changed | Ensures least-privilege on return |
| EmailTemplate via Velocity | Consistent with BNP Paribas notification framework |

---

## BeanShell rule involved
`/rules/correlation-rule.bsh` — used during aggregation to detect  
status change from `active` → `LOA` → `active`
