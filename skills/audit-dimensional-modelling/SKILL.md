---
name: audit-dimensional-modelling
description: Kimball's dimensional modelling holds that declaring a fact table's grain before choosing dimensions or measures prevents every downstream defect, and that conformed dimensions across the bus make enterprise-wide analysis possible. Apply it to a data warehouse schema to surface grain violations, conformance failures, missing SCD assignments, and dimension design smells. Use when the user wants to audit a dimensional schema, review a data mart design for correctness, or find design violations before a build.
---

# Audit Dimensional Modelling

Kimball's bus architecture requires one grain per fact table and conformed dimensions across the enterprise — violate either and every downstream analysis fails.

## Checklist

- [ ] Read all schema definitions, fact tables, and dimension tables completely before starting — skim nothing.

### Fact Tables

- [ ] **Fact table type** — "a fact table's structure is chosen without identifying whether the business process produces discrete events, regular period-end states, or tracked lifecycle instances." Classify as transaction (one row per event), periodic snapshot (one row per entity per period), or accumulating snapshot (one row per process instance with multiple milestone date stamps), then redesign accordingly.
- [ ] **Grain** — "a fact table lacks a one-sentence grain declaration." Declare the grain (who, what, when, where) before defining any dimension or measure.
- [ ] **Off-grain facts** — "a fact measure does not exist at the declared grain of the fact table." Move the offending measure to a separate fact table at the correct grain.
- [ ] **NULL foreign keys** — "a fact row contains a NULL foreign key to a dimension." Add a 'Not Applicable' or 'Unknown' member row to each dimension table (surrogate key 0); replace every NULL foreign key in the fact table with that row's surrogate key.
- [ ] **Non-numeric fact column** — "a fact table column contains descriptive text, a status label, or a non-numeric flag." Move text to a dimension attribute; consolidate low-cardinality flags and indicators into a junk dimension; retain source transaction identifiers on the fact row as degenerate dimensions.
- [ ] **Semi-additive and non-additive facts** — "a numeric fact measure is aggregated with SUM across all dimensions without checking whether it is semi-additive or non-additive." Classify each measure as fully additive, semi-additive (meaningful to sum across most dimensions but not time, e.g., account balances, inventory levels), or non-additive (ratios, percentages, unit prices); document the correct aggregation function for each.
- [ ] **Multi-valued dimension** — "a dimension has a many-to-many relationship to the grain of the fact table (e.g., multiple diagnoses per patient visit, multiple account holders per account)." Introduce a bridge table between the fact table and the dimension; assign and document a weighting factor for each bridge row to prevent double-counting.
- [ ] **Natural keys** — "a fact or dimension table uses a source system natural key as a primary or foreign key." Replace all natural keys with integer surrogate keys; retain the original natural key as a non-key attribute on the dimension.

### Dimension Tables

- [ ] **Snowflaking** — "a dimension table is normalised into joined sub-tables forming a snowflake." Denormalise all sub-tables back into a single, flat dimension table.
- [ ] **SCD strategy absent** — "no slowly changing dimension type is assigned to dimension attributes that are updated at source." Assign Type 1, 2, or 3 to each attribute based on whether its history must be preserved.
- [ ] **Type 2 tracking columns absent** — "a dimension attribute is assigned SCD Type 2 but the table has no `row_effective_date`, `row_expiration_date`, or `current_row_indicator` columns." Add all three; set `row_expiration_date` to a far-future sentinel value (e.g., `9999-12-31`) for currently active rows.
- [ ] **Mini-dimension needed** — "a large dimension has a small set of rapidly changing demographic or behavioural attributes causing disproportionate Type 2 row proliferation." Split those volatile attributes into a separate mini-dimension with its own surrogate key; add the mini-dimension FK to the fact table.
- [ ] **Operational codes as attributes** — "a dimension attribute contains source system operational codes, numeric status flags, or cryptic abbreviations without verbose, self-explanatory counterparts." Decode each operational code to a full descriptive label; retain the original code as a separate attribute if source system traceability requires it.
- [ ] **Role-playing dimensions** — "a single physical dimension table appears in the same fact table under two or more roles (e.g., order date, ship date, delivery date) without an alias view for each." Create a named alias view per role over the single physical dimension table; query each role exclusively through its alias.
- [ ] **Date dimension absent** — "date foreign keys in a fact table do not join to a purpose-built date dimension." Add a date dimension spanning the full analysis horizon, with calendar, fiscal, and relative-period attributes.

### Bus Architecture

- [ ] **Non-conformed dimensions** — "a business entity (e.g., Customer, Product) is defined with different keys or attribute sets across two or more subject-area schemas." Redefine it once as an enterprise conformed dimension; give each subject area the canonical table or an agreed-upon conformed subset.
- [ ] **Non-conformed facts** — "a shared business metric (e.g., revenue, headcount) is calculated differently in two or more subject-area schemas." Define a single enterprise-wide calculation for each shared metric, publish it in the bus matrix, and give any local variants a distinct name.
- [ ] **Bus matrix absent** — "no enterprise data warehouse bus matrix documents which conformed dimensions apply to which business process fact tables." Build the bus matrix as a grid of business processes (rows) against conformed dimensions (columns) before schema sign-off.

- [ ] Before reporting: review each finding for conflicting forces (would fixing this create a different problem?) and significance (is this too marginal to act on?). Aim to converge — discard advice that churns rather than converges.
- [ ] Report each finding in plain language: label, affected table or schema, and specific steps to fix it; order by severity. Offer to dig deeper on any finding.
