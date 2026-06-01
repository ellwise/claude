---
name: create-new-skill
description: Generate a new skill that applies a named theoretical framework ("school of thought") to a problem domain. Use when the user wants to create a skill based on a specific theory, methodology, or analytical framework — e.g. Diataxis, DITA, coupling/cohesion, The Science of Scientific Writing.
---

# Create New Skill

Generate a Claude skill that applies a named analytical framework to a specific problem domain, following the structure in [TEMPLATE.md](TEMPLATE.md).

## Procedure

### 1. Gather inputs

- [ ] Confirm the **theory** (named framework) — ask if not stated.
- [ ] Confirm the **domain** (artefact type being analysed) — ask if unclear.
- [ ] Derive a kebab-case skill name; confirm with the user. Prefix: choose a verb that names what the skill does and matches the framework's orientation — a checking verb for goodness-focused frameworks, a hunting verb for smell-focused ones. Suffix: name the ideal or the smell, not the author, never abbreviated.
- [ ] Decide whether a reference file companion is needed. The test is not volume — it is whether the LLM will drift without the material. For well-known theories the LLM already knows (Swales, Diataxis, DITA, Gopen & Swan, cohesion/coupling, etc.), a reference file adds noise, not value. Only recommend one for: (a) obscure or proprietary theories the LLM cannot be expected to know, or (b) a small number of specific distinctions the LLM predictably blurs. If no reference file is needed, anchor checklist items in the theory's own native vocabulary.

Do not draft until theory, domain, and skill name are all confirmed.

### 2. Draft the checklist

Each checklist item encodes a signal → action pair from the theory. Because the LLM already knows the framework, the checklist distills that knowledge into executable form.

Draft the full checklist in one pass for well-known theories; for obscure or proprietary ones, confirm checklist items before writing.

- [ ] First item: read the artefact completely before starting — skim nothing.
- [ ] Each item: `**Label** — "verbatim signal." Fix instruction (imperative, one sentence).`
- [ ] Aim for a handful of items; consolidate or push content to the reference file if the count exceeds this.
- [ ] Add `###` subheadings only when the framework itself names the groupings — use its own vocabulary for the heading text, never an imposed editorial structure. Items without a natural grouping stay flat.
- [ ] No prose between items — restrict non-item content to the frontmatter, the one-liner, any finding-labels block, and subheadings from the framework's own vocabulary.
- [ ] Second-to-last item: a convergence check — review each finding for conflicting forces and significance; discard advice that churns rather than converges.
- [ ] Final item: summary in plain language — each finding with what's wrong and specific steps to fix it, ordered by severity, with an offer to dig deeper.

### 3. Draft reference file (if agreed)

- [ ] **SKILL.md** — checklist items and finding labels only.
- [ ] **Reference file** — only material the LLM cannot be expected to know, or distinctions it predictably blurs. Do not restate what the LLM already knows about the theory; that is not a reference file, it is noise.

### 4. Write the skill

- [ ] Write `~/.claude/skills/<skill-name>/SKILL.md` per [TEMPLATE.md](TEMPLATE.md).
- [ ] If a reference file was agreed, write it to `~/.claude/skills/<skill-name>/<chosen-name>.md` and add a markdown link at the top of SKILL.md instructing Claude to read it before starting.

### 5. Verify

- [ ] Skill name: prefix names the action and matches orientation (checking verb for goodness-focused; hunting verb for smell-focused); suffix names the ideal or the smell, not the author or framework.
- [ ] Description follows the three-part pattern: theory sentence ("[Theory] holds that [core claim]") → application sentence ("Apply it to [domain] to [output]") → "Use when" routing clause; stress position of the theory sentence carries the consequence, not the mechanics.
- [ ] No prose between items — only frontmatter, one-liner, finding-labels block (if any), `###` subheadings from the framework's own vocabulary (if any), and `- [ ]` items.
- [ ] Item count is acceptable; flag to user if greater than a handful.
- [ ] Checklist items encode observable signals (not vague instructions).
- [ ] Every finding label used in the checklist appears in the reference file glossary (if one exists); every entry in the reference file is either unknown to the LLM or a distinction it predictably blurs — remove any entry that restates what the LLM already knows.
- [ ] First checklist item instructs reading the artefact completely before starting.
- [ ] Second-to-last item is a convergence check: reviews findings for conflicting forces and significance.
- [ ] Final item: plain-language summary with a remediation plan per finding, ordered by severity, with an offer to dig deeper — no framework jargon in the output.

## Output format

```
## Skill created

**Skill:** `<skill-name>`
**Files written:**
- `~/.claude/skills/<skill-name>/SKILL.md`
- `~/.claude/skills/<skill-name>/<reference-file>.md` *(if applicable)*
```
