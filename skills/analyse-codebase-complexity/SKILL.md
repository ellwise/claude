---
name: analyse-codebase-complexity
description: "Ousterhout's Philosophy of Software Design holds that complexity is the root cause of software failure, accumulating wherever modules stay shallow, design decisions leak across boundaries, or nothing can be understood in isolation. Apply it to a codebase to surface Ousterhout's named red flags across interface depth, decomposition, naming, and comments. Use when the user wants to find where complexity is accumulating, evaluate module depth, or identify design smells before a refactor."
---

# Analyse Codebase Complexity

Ousterhout's framework identifies twelve named red flags that let complexity accumulate; each maps to a fix that restores depth, localises knowledge, or clarifies intent.

## Checklist

### Interface depth

- [ ] **Shallow module** — "the benefit it provides is not much greater than the cost to learn or use it" (large interface, small functionality). Push more work inside; simplify the interface until the ratio inverts.
- [ ] **Information leakage** — "a design decision is reflected in multiple modules"; any change to it ripples everywhere. Consolidate the decision into one owner; the others derive from it.
- [ ] **Temporal decomposition** — "the structure of the system corresponds to the time order in which operations occur" rather than to what each module knows. Reorganise around information ownership, not execution order.
- [ ] **Overexposure** — "the API for a commonly used feature forces users to learn about other features that are rarely used." Hide the rarely-used machinery behind defaults or a separate interface.

### Decomposition

- [ ] **Pass-through method** — "a method that does little except invoke another method, whose signature is similar or identical." Eliminate the indirection: have callers invoke the lower-level method directly, or consolidate the responsibility in one class.
- [ ] **Repetition** — "if the same piece of code appears over and over again, you haven't found the right abstraction." Extract the common logic into one place; let call sites derive from it.
- [ ] **Special-general mixture** — "special-purpose code is not cleanly separated from general-purpose code." Extract the general-purpose mechanism into its own module; special-purpose logic sits above or alongside it.
- [ ] **Conjoined methods** — "the only way to understand the implementation of one method is to also understand the implementation of another." Merge them, or introduce a shared abstraction so each can be understood independently.

### Naming

- [ ] **Vague name** — "a name is so imprecise that it allows multiple interpretations." Rename until no misreading survives without ignoring the word itself.
- [ ] **Hard to pick name** — you cannot find a name that is both simple and precise. Treat this as a design signal: the concept itself may be poorly defined. Reconsider the interface or split the responsibility until a name comes easily.

### Comments

- [ ] **Comment repeats code** — "the comment says exactly what the code does." Delete it; if explanation is still needed, write a why-comment or improve the code itself.
- [ ] **Implementation documentation contaminates interface** — "an interface comment describes implementation details that users needn't know." Move those details to an implementation comment inside the body; the interface comment should describe only what callers need.

### Summary

- [ ] Print a severity-ordered summary of findings, labelling each with the red-flag name and the file/symbol where it appears.
