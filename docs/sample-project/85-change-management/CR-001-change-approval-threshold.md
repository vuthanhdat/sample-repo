---
id: CR-001
type: ChangeRequest
status: Proposed
version: 1
title: Change high-value Order approval threshold
relations:
  changes:
    - BR-ORD-001
    - CFG-DES-001
  impacts:
    - REQ-ORD-002
    - DES-ORD-001
    - API-ORD-001
    - SCR-ORD-001
    - TEST-ORD-001
    - SEC-TEST-001
---

# CR-001 — Change High-value Order Approval Threshold

## Change request

Business requests changing `order.approval.highValueThreshold` from `50,000,000 VND` to `100,000,000 VND` effective at an approved date/time.

## Reason

Current threshold causes excessive manual approval volume relative to updated operating policy.

## Changed canonical objects

- `BR-ORD-001` — threshold semantics/value policy.
- `CFG-DES-001` — configuration baseline/effective-value handling if required.

## Candidate impact analysis

| Object | Candidate impact | Proposed disposition |
|---|---|---|
| `REQ-ORD-002` | Requirement text may state only “high-value”; behavior still applies | Review Required |
| `DES-ORD-001` | Solution architecture unchanged | No Change Required after review |
| `API-DES-001` / `API-ORD-001` | Contract unchanged; runtime config changes | Revalidation Required |
| `SCR-DES-001` / `SCR-ORD-001` | UI behavior derives from backend state | Revalidation Required |
| `TEST-ORD-001` | Boundary-value cases need new expected threshold | Update Required |
| `SEC-TEST-001` | Permission model unchanged | No Change Required after review |
| `ROADMAP-001` | No milestone structure impact expected | No Change Required |

## Implementation/change tasks

After approval, create explicit task(s) to update configuration baseline/test cases and deploy effective change. Do not silently edit baseline documents in place.

## Verification

Boundary cases below/at/above new threshold must pass, existing Order decisions must retain historical interpretation/audit context, and no API schema change is expected.

## Change closure evidence

Closure requires approved impact dispositions, object versions updated where needed, deployment/config revision, and linked successful verification runs.
