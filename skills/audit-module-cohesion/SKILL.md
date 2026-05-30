---
name: audit-module-cohesion
description: "Yourdon and Constantine's cohesion taxonomy holds that a module's elements can be bound together by anything from coincidence to shared purpose — and that lower cohesion predicts fragility, reuse failure, and tangled change. Apply it to Python modules to classify each by cohesion level and surface the refactoring move that would raise it. Use when the user wants to know why a module is hard to reuse or test, find the right split point for an oversized module, or evaluate whether a set of functions truly belongs together."
---

# Yourdon & Constantine Cohesion Analysis

Classify each module by its cohesion level — from coincidental (worst) to functional (best) — and name the one refactoring move that would raise it.

## Checklist

- [ ] **Coincidental cohesion** — Module contains unrelated functions or classes with no shared data, no shared purpose, and no meaningful name for their grouping — `utils.py`, `helpers.py`, and `misc.py` are the canonical signs. Split into purpose-named modules, one responsibility each.
- [ ] **Logical cohesion** — Module groups elements by *category of operation* (all validators, all formatters, all parsers) rather than by what they operate on or what task they serve; importers use only one branch at a time. Separate by subject or task — the module name should not answer "what kind of thing does this do?" but "what does this do?"
- [ ] **Temporal or procedural cohesion** — Module groups functions that run at the same phase (startup, teardown) or must be called in a fixed sequence, but don't share data or a common subject. Encapsulate the sequence in a single orchestrating function or class; split unrelated phase members by purpose.
- [ ] **Communicational or sequential cohesion** — Functions share a data structure, operate on the same object, or feed output to the next step — close to correct, but bound by shared data rather than shared purpose. Extract a class whose methods own the shared data, or collapse the pipeline into a named function that makes the sequence explicit.
- [ ] **Functional cohesion** — All elements serve a single, well-defined purpose; the module name captures that purpose without qualification. No action needed — flag as healthy.
- [ ] Print all findings ordered by severity: high (coincidental, logical) → medium (temporal, procedural) → low (communicational, sequential) → healthy (functional). End with: *"Which of these would you like to dig into?"*
