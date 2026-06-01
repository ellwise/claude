---
name: audit-dbt-style
description: dbt's "How we style our dbt projects" holds that consistent naming, formatting, and tooling across SQL, Python, Jinja, and YAML eliminate the friction that makes a codebase hard to read and harder to hand off. Apply it to a dbt project to surface field naming deviations, SQL formatting violations, CTE design problems, and inconsistent Jinja or YAML style. Use when the user wants to audit whether a dbt project conforms to dbt Labs' style conventions, enforce consistency before code review, or identify style drift across SQL, Jinja, and YAML.
---

# Audit dbt Project Style

dbt's style guide treats naming, formatting, and tooling not as aesthetics but as contract — every deviation makes the codebase harder to read and slower to hand off.

## Checklist

- [ ] Read all dbt model files, YAML config, and Python models completely before starting — skim nothing.

### Models

- [ ] **Model naming** — Models are not pluralized, or names use dots instead of underscores. Pluralize all model names (`customers`, not `customer`); use underscores instead of dots — dots map to `database.schema.object` on most platforms and force quoting.

- [ ] **Primary key** — A model has no primary key, the key is named `id` rather than `<object>_id`, keys are not string data types, or the same entity's key has different names across models. Add a primary key to every model, name it `<object>_id` (e.g., `account_id`), cast it to a string data type, and use that same name consistently across every model that references the entity — the object prefix and cross-model consistency make join relationships unambiguous.

- [ ] **Field naming** — Booleans lack `is_`/`has_` prefixes; timestamps aren't named `<event>_at` in UTC; dates aren't named `<event>_date`; event names are not past tense; price or revenue fields store integers without a `_in_cents` suffix; names use `camelCase` or mixed case instead of `snake_case`; fields use source system terminology, abbreviations, or reserved words. Prefix booleans with `is_` or `has_`; name timestamps `<event>_at` (always UTC) and dates `<event>_date` using past-tense event names (`created_at`, `updated_at`, `deleted_at`); store prices as decimal currency or suffix integer fields `_in_cents`; use `snake_case` for all schema, table, and column names; avoid reserved words; use business terminology over source terms — "an onboarding employee will understand `current_order_status` better than `current_os`."

- [ ] **Column ordering** — Columns are ordered arbitrarily without type grouping. Order columns as ids, strings, numerics, booleans, dates, timestamps; use separator comments (e.g., `---------- ids`) to label each group so downstream consumers can scan for the columns they need.

### SQL

- [ ] **SQL basics** — Keywords or function names are uppercase, indentation is not 4 spaces, lines exceed 80 characters, commas are leading rather than trailing, or `as` is omitted when aliasing. Use lowercase for all keywords, function names, and field names; use 4-space indentation; keep lines under 80 characters; use trailing commas; always write `as` explicitly when aliasing a field or table. Use Jinja comments (`{# #}`) for any comment that should not appear in compiled SQL. Write in-model config blocks using the `{{ config(...) }}` macro at the top of the model, with each argument on its own indented line.

- [ ] **Import CTEs** — `{{ ref('...') }}` or `{{ source('...') }}` calls appear inline in transformation logic, or import CTEs select `*` without filtering. Place every `{{ ref('...') }}` and `{{ source('...') }}` call in a named import CTE at the top of the file, name it after the table it references, and limit the data scanned — select only the columns the model needs and filter rows with `where` clauses to exclude data the model never uses.

- [ ] **Functional CTEs** — A CTE performs multiple unrelated operations, is named vaguely, or the same CTE logic appears in more than one model. Give each CTE a single, logical unit of work and a verbose name that describes what it does (e.g., `events_joined_to_users`, not `user_events`); extract CTEs duplicated across models into their own intermediate model.

- [ ] **Final select** — The model's last statement selects specific columns or references a non-final CTE. End every model with `select * from <final_cte_name>` — this lets you audit intermediate steps by simply changing which CTE the final select references.

- [ ] **Join style** — Bare `join` keywords, right joins, single-letter table aliases, `union` without `all`, or unqualified column names appear in multi-table queries. Write `inner join` explicitly; restructure queries that use `right join` so the primary table is in the `from` clause instead; prefer `union all` over `union` unless intentional deduplication is required; always prefix column names with the table name when two or more tables are joined; avoid short aliases — "it's harder to understand what the table called 'c' is as compared to 'customers'."

- [ ] **Aggregation placement** — A CTE joins large tables before aggregating, aggregate or window function expressions appear before plain fields, or `group by` lists column names instead of positional numbers. List plain fields before aggregates and window functions; aggregate on the smallest dataset possible before joining to other tables; use positional `group by` (e.g., `group by 1, 2`) rather than repeating column names — "if you are grouping by more than a few columns, it may be worth revisiting your model design."

### Python

- [ ] **Python tooling** — Python model files are unformatted or unlinted. Format with `black` and lint with `ruff` before committing; both are recommended by dbt Labs, and `black` is available built-in in the dbt Studio IDE.

### Jinja

- [ ] **Jinja style** — Jinja delimiters lack inner spaces (`{{this}}`), block contents aren't indented, or whitespace control is overengineered. Add spaces inside all Jinja delimiters (`{{ this }}`, `{% if %}`); indent 4 spaces inside Jinja blocks; use newlines to separate logical blocks — "don't worry (too much) about Jinja whitespace control, focus on your project code being readable."

### YAML

- [ ] **YAML formatting** — YAML uses indentation other than 2 spaces, lines exceed 80 characters, or files are not formatted with a validator. Use 2-space indentation; indent list items; keep lines under 80 characters; run Prettier (or the dbt Studio IDE YAML formatter) to enforce these rules automatically.

- [ ] Before reporting: review each finding for conflicting forces (would fixing this create a different problem?) and significance (is this too marginal to act on?). Aim to converge — discard advice that churns rather than converges.
- [ ] Report each finding in plain language: what's wrong, which section it's in (Models, SQL, Python, Jinja, YAML), and specific steps to fix it; order by severity. Offer to dig deeper on any finding.
