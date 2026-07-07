# AI Safety Coverage Maps — public field maps

Interactive maps of how research covers different areas of AI safety — and, more
importantly, what it has **not** covered yet.

**Explore the live site: https://mrswallbreakermaps.com**

This repository is the open companion to the site: the human-readable source and
analysis for the maps that are published publicly. The web app itself is developed
separately; here you get the maps' written form and their statistics.

## What a field map is

A field map is a **question ledger first, a paper catalog second**. Each map is a
sparse N-dimensional tensor of categorical axes (research question, lens, locus,
behavior, ...). A **cell** is a full coordinate tuple that holds the papers sitting
there plus a derived, graded coverage status. The whole point is to make every
*unanswered* question read as red as the answered ones read green: what is answered
(and how well), what is still open, what a paper's future-work already **asks**, and
what only the map reveals as still **askable**.

## The published maps

- **refusal_geometry** — *Refusal directions, harm schemas, evasion and defense of
  probe/latent monitors.* Where an LLM's refusal behavior lives, breaks, and is
  defended: refusal directions, jailbreaks, sparse-autoencoder features, probes and
  safety neurons.
- **vcrc** — *VCRC.* Where and how the validity, conceptual robustness and
  confounding of empirical ML research break, mapped across the field's research
  questions.

| Map | Live on the site | Report | Pipeline | Written source |
|-----|------------------|--------|----------|----------------|
| **refusal_geometry** | [open the map](https://mrswallbreakermaps.com/maps/refusal_geometry) | [report.md](field_stat_reports/refusal_geometry/report.md) | [pipeline.md](field_stat_reports/refusal_geometry/pipeline.md) | [FIELD_MAP.md](field_maps/refusal_geometry/FIELD_MAP.md) |
| **vcrc** | [open the map](https://mrswallbreakermaps.com/maps/vcrc) | [report.md](field_stat_reports/vcrc/report.md) | [pipeline.md](field_stat_reports/vcrc/pipeline.md) | [FIELD_MAP.md](field_maps/vcrc/FIELD_MAP.md) |

Cross-map meta-report (all maps at once): [field_stat_reports/map_space/report.md](field_stat_reports/map_space/report.md).

## What's in this repo

- `field_maps/<map>/`
  - `FIELD_MAP.md` — the map's written form: the tensor axes, every cell, its
    coverage aspects and the papers that sit there.
  - `FIELD_MAP_edges.md` — the field's own future-work questions overlaid on the
    same tensor.
- `field_stat_reports/<map>/`
  - `report.md` — a state-of-the-field analytical report with embedded figures.
  - `pipeline.md` — how that report's figures are derived.
- `field_stat_reports/map_space/`
  - `report.md` — a cross-map meta-report: "what can a field map look like at all,
    and where does each of ours sit?" (theory-of-fields lenses + the economics of
    supply vs. demand across maps).

Start with the live site for the interactive experience, then read
`field_stat_reports/map_space/report.md` for the big picture and each
`field_maps/<map>/FIELD_MAP.md` for the detail.
