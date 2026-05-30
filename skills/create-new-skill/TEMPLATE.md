# Create New Skill Template

## SKILL.md format

A SKILL.md contains: frontmatter, one-liner, `- [ ]` checklist items, and `###` subheadings when the framework names the groupings. No prose between items; no headers that impose structure the framework does not itself provide.

```markdown
---
name: <kebab-case>
description: <"[Theory] holds that [core claim — stress position carries the consequence]." "Apply it to [domain] to [skill output]." "Use when the user wants to [trigger conditions].">
---

# <Skill Title>

<One sentence: what the theory optimises for and what it catches.>

<!-- If a reference file was agreed, add: Read [<filename>.md](<filename>.md) before starting. -->
<!-- If the skill uses finding labels, add a "Use these terms exactly in findings: ..." block here. -->

## Checklist

<!-- Use ### subheadings only when the framework itself names these groupings. -->

- [ ] **Label** — "verbatim signal from the theory." Fix instruction (one sentence, imperative).
- [ ] **Label** — ...
- [ ] Print a summary ordered by severity.
```

**What belongs in SKILL.md vs a reference file:**

- **SKILL.md** — checklist items and finding labels only. If Claude acts on it, it goes here.
- **Reference file** — glossary, annotated examples, supporting theory. If Claude reads it to understand a term, it goes there.
