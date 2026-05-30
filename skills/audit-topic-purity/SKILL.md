---
name: audit-topic-purity
description: DITA holds that the three topic types of technical documentation — task, concept, and reference — must not be mixed within a single topic, because each type serves a different reader act: doing, understanding, or looking up. Apply it to documentation to surface type violations, mixed-type topics, and structural non-compliance. Use when the user wants to audit or restructure technical documentation against DITA's information-typing discipline.
---

# Type with DITA

DITA information typing catches documentation that directs when it should orient, or orients when it should list — content assigned the wrong topic type.

Use these terms exactly in findings: **type violation** (content of one type inside a topic typed as another — name the direction); **mixed-type topic** (a single topic that transitions between types); **type gap** (a subject area where one or more topic types are absent or underrepresented); **title mismatch** (topic title convention does not match declared or inferred type); **shortdesc failure** (absent or tautological short description); **scope overload** (topic covers more than one distinct subject).

## Checklist

### Classification

- [ ] **Type census** — "a topic's content does not clearly align with one of the three core types." Walk every topic and assign an inferred type (task, concept, or reference); flag any topic that resists clear typing.

### Task topics

- [ ] **Task type violations** — "a task topic contains no steps, or a concept or reference topic contains a numbered procedure." Ensure every task topic has a `<steps>` or `<steps-unordered>` body; move procedural content out of concept and reference topics.

### Concept topics

- [ ] **Concept type violations** — "a concept topic contains step-by-step instructions, or a task topic contains orientation, background, or rationale prose." Move explanatory content to a concept topic; strip it from task and reference bodies.

### Reference topics

- [ ] **Reference type violations** — "a task or concept topic contains a parameter table, syntax block, or exhaustive property enumeration." Move lookup-oriented data to a reference topic with a `<refbody>`.

### Cross-type concerns

- [ ] **Mixed-type topic** — "a single topic opens with background prose, shifts to numbered steps, and closes with a parameter table." Split into separate typed topics; connect them through the ditamap hierarchy or a relationship table.
- [ ] **Title mismatch** — "a task topic title is not a verb phrase, or a concept or reference topic title is a verb phrase." Rename: task titles to verb phrases ('Configure the server'), concept and reference titles to noun phrases ('Server configuration', 'Configuration parameters').
- [ ] **Shortdesc failure** — "a topic has no `<shortdesc>`, or the short description restates the title word for word." Write a 1–2 sentence `<shortdesc>` stating what the topic covers and who it is for.
- [ ] **Scope overload** — "a topic addresses more than one distinct subject, or is fewer than three meaningful sentences with no referenced sibling topics." Split overloaded topics into typed children; flag stubs for expansion or removal.
- [ ] Print a summary of all findings grouped by DITA topic type, ordered by severity.
