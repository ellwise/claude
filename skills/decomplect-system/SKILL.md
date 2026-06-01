---
name: decomplect-system
description: Hickey's "Simple Made Easy" holds that complexity enters systems through complecting — braiding together things that could be independent — and that the remedy is replacing each complexity-generating construct with a simpler alternative, not reorganising the existing ones. Apply it to a system to surface each instance of complecting and name the specific construct that replaces it. Use when the user wants to radically simplify a system, identify what is entangled and why, or choose which constructs to replace rather than where to reorganise.
---

# Decomplect System

Surface each instance of complecting — the unnecessary braiding of concerns into a single construct — and name the simpler alternative, following Rich Hickey's "Simple Made Easy" (Strange Loop, 2011).

Use these terms exactly in findings: **complect**, **simple**, **easy**, **construct**, **values**, **managed reference**, **polymorphism à la carte**, **queue**, **declarative**.

## Checklist

- [ ] **Easy mistaken for simple** — "construct justified on grounds of familiarity, convention, or framework support." To test whether it is also simple, apply Hickey's independence criterion: *could these concerns ever change for different reasons — would any caller ever need one without the other?* If the bundled concerns co-vary by definition (all facets of a single boundary or invariant), that is cohesion, not complecting. Flag only when genuinely independent concerns have been forced to travel together.
- [ ] **State** — "mutable variable or field whose value changes over time." Replace with values.
- [ ] **Objects** — "class or object bundles state, identity, and value together." Separate into values and functions.
- [ ] **Information hidden in objects** — "information passed or returned as a custom class rather than as a map, array, or set." Represent as a plain data structure.
- [ ] **Methods** — "function defined on an object, coupling behaviour to state." Extract as a function that takes a value argument.
- [ ] **Inheritance** — "type hierarchy couples concrete types together through shared base." Replace with polymorphism à la carte — protocols, type classes, or interfaces with no shared implementation.
- [ ] **Switch / pattern matching on type** — "dispatch on a tag or type **repeated across call sites**." Replace with polymorphism à la carte.
- [ ] **Mutable variables** — "variable reassigned over time rather than holding a value." Separate value from the change operation using a managed reference.
- [ ] **Imperative loops** — "loop that entangles what to compute with how to iterate." Replace with set functions or declarative operations.
- [ ] **Actors** — "actor couples the message (what) with its recipient (who)." Replace with a queue.
- [ ] **ORM** — "ORM layer braids the object model with the relational model." Replace with declarative data manipulation — SQL or Datalog.
- [ ] **Scattered conditionals** — "policy or business rules encoded as conditionals spread across the system." Extract to a rule system or declarative configuration.
- [ ] **Modularity mistaken for simplicity** — "system described as well-structured or modular while constructs within its units remain complected." Identify the complecting construct inside each unit; partitioning does not remove complecting.
- [ ] Print a summary of findings ordered by severity, naming the construct, what it complects, and the simpler alternative.
