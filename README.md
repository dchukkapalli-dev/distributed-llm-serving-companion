# Companion archive — Distributed LLM Serving (CSUR submission)

This folder is the machine-readable companion to the survey
*"Distributed Serving Architectures for Large Language Model Inference:
A Taxonomy, Quantitative Models, and Practitioner's Decision Framework"*
(Chukkapalli, Mishra, Naik — ACM Computing Surveys submission, 2026).

**Deposit.** Permanently archived on Zenodo with DOI
[10.5281/zenodo.20423003](https://doi.org/10.5281/zenodo.20423003);
source mirror at
<https://github.com/dchukkapalli-dev/distributed-llm-serving-companion>.

It exists so that any reader can audit the taxonomy assignments and the
PRISMA flow counts without re-deriving the source set, and can amend a
pillar assignment by editing a single CSV row.

## License (CC-BY-4.0)

Copyright (c) 2026 Divya Chukkapalli, Sanjay Mishra, Ganesh R Naik.

This companion is released under the **Creative Commons Attribution 4.0
International License**. You may share and adapt the contents for any
purpose, including commercially, provided you cite the accompanying survey;
see [`LICENSE`](LICENSE) for the full text and [`CITATION.cff`](CITATION.cff)
for the required attribution format.

## Contents

| File | What it is |
|------|------------|
| `taxonomy.csv` | All 56 included techniques with fields: `name`, `pillar`, `representative_system`, `primary_objective`, `citation_key`, `cross_pillar`. One row per technique. The `pillar` field is the chapter the technique is anchored in; `cross_pillar` lists secondary chapters where the technique is *discussed but not anchored* (so each technique is counted exactly once in totals). The `citation_key` matches an entry in `references.bib`. |
| `references.bib` | BibTeX records for all 120 cited works in the manuscript. |
| `prisma_counts.csv` | Stage-by-stage record counts for the PRISMA-2020-style flow diagram (Fig. 1 in the manuscript): identification (412), de-duplication (287), snowballing (+64), title/abstract screening (351 → 198), full-text eligibility (198 → 56). |
| `CITATION.cff` | Citation File Format (1.2.0) record so the artefact can be cited independently of the manuscript. |
| `LICENSE` | Full text of the CC-BY-4.0 license. |

## How to use

- **Audit a pillar assignment.** Open `taxonomy.csv`, locate the row for
  the technique, and check `pillar` and `cross_pillar`. The manuscript
  text and Section / chapter headings use the same pillar names.
- **Recount included techniques.** `wc -l taxonomy.csv` returns 57
  (header + 56 rows); the 56 rows are exactly the techniques that
  passed full-text eligibility.
- **Reproduce the reference count.** `grep -c '^@' references.bib`
  returns 120.
- **Reclassify a technique.** Edit the relevant `pillar` /
  `cross_pillar` cell directly; downstream counts per pillar are a
  groupby on `pillar`.

## How to reproduce the PRISMA flow

The stage-by-stage record counts in `prisma_counts.csv` and Fig. 1 of the
manuscript were produced by the following procedure; a reader who repeats
it on the same dates and venues should land within a handful of records
of these numbers (small drift is expected as databases re-index).

1. **Time window.** 2019 through 2026; last refreshed May 2026. Pre-2019
   foundational works (e.g. Vaswani 2017) are cited where structurally
   necessary but excluded from the PRISMA counts.

2. **Venue list.** Systems: OSDI, SOSP, NSDI, ATC, ASPLOS, ISCA, HPCA, SC,
   SoCC, MLSys. Machine learning: NeurIPS, ICML, ICLR, ACL, EMNLP.
   Preprints on arXiv were included only when their content was referenced
   verbatim by a later peer-reviewed paper or by a deployed engine.

3. **Boolean query string.**
   ```
   ("LLM serving" OR "language model serving" OR "inference serving" OR
    "transformer inference")
   AND
   ("distributed" OR "parallelism" OR "KV cache" OR "speculative" OR
    "disaggregation" OR "scheduling" OR "batching" OR "fault tolerance" OR
    "multi-tenant")
   ```
   Per-database adaptations: DBLP uses unquoted phrase syntax with `&`
   instead of `AND`; ACM Digital Library uses the `Advanced Search`
   `AllField:` qualifier; IEEE Xplore uses `("All Metadata":...)`;
   USENIX uses simple keyword search and is filtered by venue post-hoc;
   Google Scholar uses the same query verbatim with `intitle:` removed
   so that abstract matches are admitted.

4. **Audit counts.** Initial query: 412 records. After de-duplication
   across databases: 287 unique items. Forward + backward snowballing
   from the included set: +64 records (32 backward from references of
   included items; 32 forward via Google Scholar's citation index, last
   refreshed May 2026). Title/abstract screening: 351 → 198 retained.
   Full-text eligibility: 198 → 56 included as distinct techniques (the
   rows in `taxonomy.csv`). Background and adjacent-scope citations
   bring the total reference count in the manuscript to 120.

5. **Snowballing stop criterion.** Iteration stopped when one forward and
   one backward pass over the previously included set produced fewer than
   five new candidates that passed full-text eligibility---i.e. when the
   marginal yield of an additional iteration fell below 10% of the typical
   single-iteration yield.

## Methodology note (single-coder)

Coding was performed by a single author. This release makes the coding
trivially auditable; it does not replace a multi-coder inter-rater
study, and the manuscript states this limitation in its Methodology
section. The companion CSV's `cross_pillar` field is populated for five
of the 56 techniques (~9%); these are the items most exposed to a
second coder's disagreement and can be re-anchored by editing the
corresponding `pillar` cell.

## Licence (full text)

All files in this companion archive (`taxonomy.csv`, `prisma_counts.csv`,
`references.bib`, `CITATION.cff`, this `README.md`) are released under the
**Creative Commons Attribution 4.0 International License (CC-BY-4.0)** to
maximise reuse for follow-up surveys. The full license text is in
[`LICENSE`](LICENSE); the canonical short summary is at
<https://creativecommons.org/licenses/by/4.0/>. Required attribution is a
citation to the accompanying survey as specified in [`CITATION.cff`](CITATION.cff).
