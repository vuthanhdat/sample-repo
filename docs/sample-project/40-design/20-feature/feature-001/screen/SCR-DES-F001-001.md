---
id: SCR-DES-F001-001
type: DesignSpecification
subtype: Screen
feature: FEATURE-001
status: Draft
version: 1
---
# SCR-DES-F001-001 — Feature 001 Screen Specification

Inherits `FE-ARCH-001`, `UI-STD-001`, `AUTH-DES-001`.

## Screen Structure
- Header / context
- Main information area
- Actions
- List/table/form sections as applicable

## Field / Action Matrix
| Item | Type | Required | Source | Permission | Validation |
|---|---|---|---|---|---|
| `<item>` | `<field/action>` | `<yes/no>` | `<API/state>` | `<permission>` | `<rule>` |

## States
Define loading, empty, error, permission denied, validation and success states.

## Relations
- `specifies → SCR-F001-001`
- `uses → API-F001-001`