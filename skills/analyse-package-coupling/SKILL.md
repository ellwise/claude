---
name: analyse-package-coupling
description: "Martin's package principles hold that packages must be cohesive (CCP, CRP, REP), acyclic (ADP), and arranged so dependencies flow toward stability (SDP, SAP) — violate any of these and packages become hard to test, deploy, or reuse without cascading effects. Apply them to a Python package to surface dependency cycles, stability inversions, cohesion mismatches, and abstraction failures. Use when the user wants to audit package boundaries, understand why changes ripple unexpectedly, or evaluate whether a package is safe to extract as a library. SAP applies to behavioral packages; skip data schema packages where structure is the contract."
---

# Martin's Package Principles

Six principles govern what belongs in a package and how packages depend on each other; where they are violated, changes hurt most.

## Scope Note

Martin's principles assume nominal typing and inheritance (OOP classes with methods). **SAP distinguishes stable packages that define behavioral contracts (interfaces, protocols) from those providing concrete implementations.** This distinction breaks down for data schemas — their concrete structure IS the contract. Skip SAP for packages whose primary purpose is defining data shapes (models, schemas, DTOs) rather than behaviors (services, operations, transformations).

## Checklist

### Cohesion principles — what belongs in a package

- [ ] **Release Equivalence Principle (REP)** — Flag packages that mix stable utility modules (high fan-in, infrequently changed) with volatile domain modules (low fan-in, frequently changed); they cannot be released as a coherent unit. Split into a stable library package and a volatile application package.
- [ ] **Common Closure Principle (CCP)** — Flag classes that change in the same commits but live in different packages, using `git log --follow`; consolidate them. If git history is unavailable, treat heavy mutual imports across package boundaries as a proxy for co-change.
- [ ] **Common Reuse Principle (CRP)** — Flag packages where importers consistently use only a narrow subset of the public surface (check `__all__` or top-level names vs. what importers actually bind). Split along usage boundaries so no importer is forced to depend on things it doesn't use.

### Coupling principles — how packages should depend

- [ ] **Acyclic Dependencies Principle (ADP)** — Walk the import graph and identify any strongly-connected components (2+ nodes). Extract a shared dependency package or invert a dependency to break each cycle; flag each as high severity.
- [ ] **Stable Dependencies Principle (SDP)** — For each package, compute instability I = Fo / (Ca + Fo); flag any import edge where the depender has lower I than the dependee (a stable package depending on a volatile one). Move the volatile responsibility or introduce an abstracting interface.
- [ ] **Stable Abstractions Principle (SAP)** — Examine each stable behavioral package (I < 0.3): count how many protocols and ABCs it defines versus concrete classes with methods. Skip data-schema packages—Pydantic models, dataclasses, and TypedDicts define their contract through structure, not abstraction. Flag stable packages that predominantly implement rather than abstract. Introduce protocols or ABCs so stable packages specify what must be done without dictating how to do it.
- [ ] Print all findings ordered by severity: high (Acyclic Dependencies violations) → medium (Stable Dependencies, Stable Abstractions, Common Closure, Common Reuse violations) → low (Release Equivalence mismatches). End with: *"Which of these would you like to dig into?"*
