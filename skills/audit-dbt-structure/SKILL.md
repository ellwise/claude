---
name: audit-dbt-structure
description: dbt's "How we structure our dbt projects" holds that staging, intermediate, and marts layers must each be source- or business-conformed, correctly named, and appropriately materialized, because any violation at a layer makes the DAG's logic unreadable and fractures the single source of truth. Apply it to a dbt project to surface layer violations, naming deviations, wrong materializations, premature aggregation, and DAG shapes that signal logic in the wrong layer. Use when the user wants to audit whether a dbt project follows dbt Labs' structural conventions, review a new project's layer design, or identify where complexity is accumulating across the staging, intermediate, or marts layers.
---

# Audit dbt Project Structure

dbt holds that folder structure, naming, and materialization must trace the journey from "source-conformed atoms" through intermediate molecules to entity-grained marts — deviations at any layer fracture the single source of truth and multiply competing definitions.

## Checklist

- [ ] Read all dbt model files, source YAML, and dbt_project.yml completely before starting — skim nothing.

### Staging

- [ ] **Staging organization** — Staging models are not grouped by source system, or file names don't follow `stg_[source]__[entity]s.sql`. Group under `models/staging/[source_system]/` (never by loader or business grouping); name each file `stg_[source]__[entity]s.sql` — the double underscore distinguishes source from entity, and entity names are always plural.

- [ ] **1-to-1 source relationship** — A staging model references multiple source tables or refs a non-source model. Restore the 1-to-1 correspondence with source tables; when a join is unavoidable to represent a clean concept — joining a delete table, unioning symmetrical sources — build `base` models in a `base/` subdirectory first.

- [ ] **Staging transformations** — A staging model contains joins (outside a base model) or aggregates that change the grain. Remove them; staging permits only renaming, type casting, basic computation, and categorization — "joins are almost always a bad idea here."

- [ ] **DRY upstream** — A transformation always applied to a concept (e.g., cents-to-dollars) repeats in downstream models rather than being applied once in staging. Move it as far upstream as staging so downstream models reference rather than repeat it — "the DRY principle is ultimately the litmus test for whether transformations should happen in the staging layer."

- [ ] **Staging materialization** — Staging models are not materialized as views. Set `+materialized: view` in `dbt_project.yml` for the staging directory; staging models are building blocks for later models, not final artifacts intended for direct querying.

### Intermediate

- [ ] **Intermediate organization** — Intermediate subdirectories group by source system rather than business area, or files don't follow `int_[entity]s_[verb]s.sql`. Regroup under `models/intermediate/[business_area]/`; name files using an entity and a transformation verb (e.g., `int_payments_pivoted_to_orders`) — unlike staging, the intermediate layer is business-conformed.

- [ ] **Intermediate materialization** — Intermediate models are materialized in the main production schema. Materialize as ephemeral or as views in a custom schema with restricted permissions; "intermediate models should generally not be exposed in the main production schema."

- [ ] **Single clear purpose** — An intermediate model serves multiple unrelated transformation goals. Refactor to a single purpose — structural simplification (joining 4–6 staging models), re-graining, or isolating a complex operation.

- [ ] **Multiple inputs, not multiple outputs** — A post-staging model fans out to multiple downstream dependents. Redesign so each model accepts multiple inputs but produces a single output; "several arrows coming out is a red flag."

### Marts

- [ ] **Entity grain** — A mart aggregates rows along a time spine (e.g., `user_orders_per_day`) instead of maintaining entity grain. Remove the time rollup; each row must represent a discrete instance of one entity — time-based rollups belong in metrics.

- [ ] **Mart organization** — Mart files include a team prefix (`finance_orders`) or time dimension, or marts group by source rather than business department. Name by entity in plain English (`orders`, `customers`); group under `models/marts/[department]/`; reserve separate mart concepts for genuinely distinct logic (`tax_revenue` vs `revenue`, not `finance_revenue` vs `marketing_revenue`).

- [ ] **Wide and denormalized** — Mart models are normalized and require consumers to join (when not using the Semantic Layer). Denormalize marts to include all relevant data about the entity at its grain; "storage is cheap and compute is expensive." If the project uses the Semantic Layer, normalize instead — MetricFlow requires the flexibility that normalization preserves.

- [ ] **Mart materialization** — Marts are not materialized as tables in production. Set `+materialized: table` in `dbt_project.yml` for the marts directory; introduce incremental materialization only when table build time becomes a bottleneck.

- [ ] **Mart complexity** — A mart joins more than 4–5 concepts directly. Extract intermediate models to pre-join groups of 3–4 concepts; "two intermediate models that bring together three concepts each, and a mart that brings together those two intermediate models, will typically result in a much more readable chain of logic."

### Project files

- [ ] **Config per folder** — YAML config files are not named `_[directory]__models.yml` / `_[directory]__sources.yml`, or all config is merged into one project-wide file. Create one `_[directory]__models.yml` per directory (and `_[directory]__sources.yml` for every staging subdirectory); the leading underscore sorts YAML files to the top of every folder. If the project uses doc blocks, also create `_[directory]__docs.md` per directory to co-locate all doc block content alongside the models it documents.

- [ ] **Cascade defaults** — Materialization, schema, or other configs are set model-by-model rather than at the directory level. Set directory-level defaults in `dbt_project.yml`; configure only exceptions at the model level — "we only need to configure exceptions to our general rules."

- [ ] **Folder-based selection** — Tags serve as the primary grouping and selection mechanism. Replace primary tag selectors with folder-based selectors (e.g., `dbt build --select marts.marketing`); reserve tags for exception groups that don't map cleanly to a folder.

- [ ] **Seeds for lookup tables only** — Seeds load data that exists in a source system. Remove those seeds and load source data with a proper EL tool; seeds are for lookup tables that don't exist in any source system.

- [ ] **Utilities folder** — General-purpose models (e.g., date spines generated from macros or seeds) are placed in the models root or inside a staging subdirectory rather than their own folder. Move them to `models/utilities/`; this folder holds tooling models that support the modeling work rather than data to model itself.

- [ ] **Macros documentation** — Macros exist without a `_macros.yml` file documenting their purpose and arguments. Create `_macros.yml` in the `macros/` folder and document every macro's purpose and arguments before it goes into regular use.

- [ ] Before reporting: review each finding for conflicting forces (would fixing this create a different problem?) and significance (is this too marginal to act on?). Aim to converge — discard advice that churns rather than converges.
- [ ] Report each finding in plain language: what's wrong, which layer it's in (Staging, Intermediate, Marts, Project files), and specific steps to fix it; order by severity. Offer to dig deeper on any finding.
