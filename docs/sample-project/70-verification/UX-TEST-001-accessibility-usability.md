---
id: UX-TEST-001
type: VerificationDefinition
subtype: UsabilityAccessibility
status: Baseline
version: 1
title: Core Order UI usability and accessibility verification
relations:
  verifies:
    - NFR-UX-001
    - SCR-ORD-001
---

# UX-TEST-001 — Usability & Accessibility Verification

## Required checks

1. Core Order Detail flow can be operated with keyboard for required actions.
2. Interactive controls expose meaningful accessible names/roles.
3. Order status, warning and error are understandable without color alone.
4. Focus behavior remains usable after dialogs, command result and validation errors.
5. Loading, empty, conflict and dependency-failure states are distinguishable and actionable.
6. Automated accessibility scan has no unapproved critical violations on the core screen.
7. Supported Chrome/Edge baseline is exercised for the critical flow.

## Manual review

Automated scanner is not sufficient. Release review includes keyboard/focus/status semantics on representative paths.

## Evidence

Target frontend revision, browser versions, automated scan output and manual checklist result are attached to the verification run.
