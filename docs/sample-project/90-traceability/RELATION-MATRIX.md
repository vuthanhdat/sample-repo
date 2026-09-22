# Relation Matrix

ProjectTemplate có thể validate source/target types theo matrix này.

| From | Relation | To | Cardinality / Note |
|---|---|---|---|
| Goal | `decomposes-to` | BusinessFlow / Feature | many |
| BusinessFlow | `decomposes-to` | Feature / Requirement | many |
| Feature | `decomposes-to` | Requirement | many |
| Requirement | `accepted-by` | AcceptanceCriterion | 1..many |
| Requirement | `governed-by` | BusinessRule | 0..many |
| Requirement | `satisfied-by` | DesignDecision/Specification | 1..many |
| CommonDesign | `applies-to` | FeatureDesign / DeliverableType / Project | many |
| DesignDecision | `introduces` | Deliverable | many |
| DesignSpecification | `specifies` | Deliverable | 1..many |
| Task | `implements` | Deliverable | many |
| Verification | `verifies` | Requirement / Deliverable / Design | many |
| ChangeRequest | `impacts` | Any traceable object | potential impact |

Reverse relations are derived.