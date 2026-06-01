---
name: audit-documentation-modes
description: "Diataxis holds that tutorial, how-to guide, explanation, and reference each serve a distinct combination of reader need and learning orientation, and that conflating them is 'at the heart of a vast number of problems in documentation.' Apply the framework to a docs site or individual document to surface conflation, absorbed content, and missing modes. Use when the user wants to classify documentation, find where boundaries are being crossed, or restructure a docs set around the four Diataxis modes."
---

# Classify with Diataxis

Procida's Compass maps documentation along two axes — action vs. cognition, and acquisition vs. application — and places the root of most documentation problems in the crossing of those boundaries.

Use these terms exactly in findings: **conflation** (a page crosses or blurs the boundary between two modes — name both); **absorbing** (an explanation page that has absorbed instructional or reference content); **missing mode** (a mode with no or inadequate coverage in the set).

## Checklist

### Classification

- [ ] **Mode census** — "a page resists clear assignment to one of the four modes." Walk every page and assign a mode using the Compass: tutorials (action + acquisition), how-to guides (action + application), explanation (cognition + acquisition), reference (cognition + application). Flag any page that resists clear classification as a candidate for conflation or splitting.

### Action-oriented modes — tutorials and how-to guides

- [ ] **Tutorial shape** — "a tutorial is learning-oriented and action-oriented"; it should direct the reader through a concrete sequence toward a single, reliably achievable outcome, minimising explanation and avoiding abstraction. Flag any tutorial that explains concepts instead of directing action, leaves the outcome open-ended, or cannot be followed to a consistent result.
- [ ] **How-to guide shape** — a how-to guide addresses "a real-world problem or task" for a reader who already knows what they want to achieve; it uses conditional imperatives ("if you want x, do y") and avoids teaching or orientation. Flag any how-to that answers *why* rather than directing *how*, or that assumes the reader needs grounding before they can act.
- [ ] **Tutorial/how-to conflation** — "tutorials are often conflated with how-to guides," making this the most common boundary crossing in practice. Flag pages that mix learning-oriented narration with task-oriented directives, or that teach a concept while claiming to guide a task.

### Cognition-oriented modes — explanation and reference

- [ ] **Explanation shape** — explanation makes connections, discusses alternatives, admits opinion, and provides context on design decisions — the understanding-oriented mode. Flag explanation that contains numbered steps (conflation with how-to) or exhaustive lookup tables (conflation with reference).
- [ ] **Explanation absorbing** — "one risk of explanation is that it tends to absorb other things." Flag explanation pages that have absorbed instructional sequences or reference data; both belong in separate pages.
- [ ] **Reference shape** — reference should be "austere and uncompromising" — purely descriptive, factual, consistent in format, and consulted rather than read. Flag reference pages that contain narrative prose, inline tutorial sequences, or opinionated recommendations.

### Coverage and navigation

- [ ] **Missing modes** — flag any of the four modes that has no coverage or only thin coverage; Procida notes tutorials in particular are often absent entirely. Propose page titles that would fill the gap.
- [ ] **Navigation alignment** — navigation should make the four modes visually distinct so readers can orient themselves before opening any page. Flag navigation that mixes modes in a single group or uses labels that do not signal the mode.
- [ ] Print a summary of findings grouped by mode, ordered by severity: missing modes → conflation and absorbing → shape failures → navigation.
