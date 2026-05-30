---
name: analyse-package-coupling
description: "Martin's package principles hold that packages must be cohesive (CCP, CRP, REP), acyclic (ADP), and arranged so dependencies flow toward stability (SDP, SAP) — violating any of these makes packages hard to test, deploy, or reuse without cascading effects. Apply them to a Python package to surface dependency cycles, stability inversions, cohesion mismatches, and abstraction failures. Use when the user wants to audit package boundaries, understand why changes ripple unexpectedly, or evaluate whether a package is safe to extract as a library."
---

# Martin's Package Principles

Six principles govern what belongs in a package and how packages should depend on each other; violating them predicts where changes will hurt most.

## Checklist

### Cohesion principles — what belongs in a package

- [ ] **REP — Release Equivalence** — Flag packages that mix stable utility modules (high fan-in, infrequently changed) with volatile domain modules (low fan-in, frequently changed); they cannot be released as a coherent unit. Split into a stable library package and a volatile application package.
- [ ] **CCP — Common Closure** — Using `git log --follow` across packages, flag classes that change in the same commits but live in different packages. Consolidate them; if git history is unavailable, treat heavy mutual imports across package boundaries as a proxy for co-change.
- [ ] **CRP — Common Reuse** — Flag packages where importers consistently use only a narrow subset of the public surface (check `__all__` or top-level names vs. what importers actually bind). Split along usage boundaries so no importer is forced to depend on things it doesn't use.

### Coupling principles — how packages should depend

- [ ] **ADP — Acyclic Dependencies** — Walk the import graph and identify any strongly-connected components (2+ nodes). Extract a shared dependency package or invert a dependency to break each cycle; flag each as high severity.
- [ ] **SDP — Stable Dependencies** — For each package, compute instability I = Fo / (Ca + Fo); flag any import edge where the depender has lower I than the dependee (a stable package depending on a volatile one). Move the volatile responsibility or introduce an abstracting interface.
- [ ] **SAP — Stable Abstractions** — For each package with I < 0.3, count protocols, ABCs, and type aliases against concrete classes; flag packages that are stable but predominantly concrete. Introduce protocols or ABCs so the stable package defines contracts, not implementations.
- [ ] Print all findings ordered by severity: high (ADP cycles) → medium (SDP, SAP, CCP, CRP violations) → low (REP mismatches). End with: *"Which of these would you like to dig into?"*
