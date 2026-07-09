# Evaluating France's ICHN Agricultural Subsidy Program: RDD & Difference-in-Differences

**Causal impact evaluation of a 40-year place-based subsidy program using quasi-experimental methods on ~28,000 French communes (1955–2012).**

Master's thesis — Toulouse School of Economics (2017), supervised by Prof. Sylvain Chabé-Ferret.
Full thesis: [`Agricultural_subsidy_program_evaluation_RDD.pdf`](Agricultural_subsidy_program_evaluation_RDD.pdf)

---

## Overview

The ICHN program (*Indemnités Compensatoires de Handicaps Naturels*) is a French agricultural subsidy, running since 1975 under the EU's Less Favored Areas framework, that compensates farmers in mountain communes for natural handicaps (high altitude and slope). Despite decades of operation and ~€410M in annual payments to 110,000 beneficiaries, it had never been quantitatively evaluated.

This project estimates the program's causal effects on:

- **Number of farms** — structural change in the agricultural sector
- **Usable farm land** — the program's core objective of maintaining farming activity
- **Livestock density** — environmental impact, before and after the 2000 payment-decoupling reform

## Why it's interesting as a case study

Eligibility is determined by a Natural Handicap Index (NHI) with a formal cutoff at 2.0 — a textbook setup for Regression Discontinuity. But real-world implementation deviated from the rulebook: the observed participation cutoff sat closer to 1.0–1.2, turning an intended sharp design into a fuzzy (and ultimately unusable) one. The project documents this failure honestly, validates it with placebo tests, and pivots to a Difference-in-Differences design that exploits the staggered rollout of the program.

## Data

| | |
|---|---|
| Unit of analysis | French commune (mainland) |
| Observations | 27,928 communes |
| Period | 1955–2012 (sparse panel: 1955, 1970, 1979, 1988, 2000, 2010/2012) |
| Treatment | ICHN participation status (from 1962 onward) |
| Outcomes | Number of farms, usable farm land (ha), livestock density (LSU/ha) |

Data merged from two commune-level datasets provided by INRA / TSE (not redistributable in this repo).

## Methods

**1. Regression Discontinuity (Section 4.1)**

- Parametric: polynomial regressions of order 1–4 with interaction terms, in log and level form, with trimming
- Non-parametric: local linear regression with
  - plug-in optimal bandwidth (Imbens & Kalyanaraman 2011), via the `rdd` package
  - leave-one-out / Generalized Cross-Validation bandwidth selection, via `locfit`
- Validity checks: McCrary-style density inspection of the running variable, placebo tests on pre-treatment (1955) outcomes, bandwidth sensitivity analysis, alternative cutoff (1.8) in the appendix

**2. Difference-in-Differences (Section 4.2)**

- Commune fixed-effects model on three purpose-built samples exploiting different entry cohorts (1962 joiners, 1970–79 joiners, late joiners as controls)
- Explicit parallel-trend testing using two pre-treatment periods where available

**3. Reform analysis (Section 5)** — fixed-effects DiD on livestock density around the 2000 switch from coupled (per-head) to decoupled (per-hectare) payments.

## Key findings

- **The program worked economically.** Without ICHN, mountain communes would have lost 14.6% more farms and 31.2% more usable farm land than comparable communes over 1955–1970. After the 1970s expansion, the gap narrowed to ~1.7% for farm counts, and treated communes actually declined *less* in farm land.
- **The 2000 "green" reform backfired environmentally.** Post-reform, livestock density in treated communes plateaued while the control group continued declining — the decoupled payment scheme was associated with *more* intensive farming, likely driven by the 50-hectare support cap.
- **Methodological lesson:** a clean RD design on paper is worthless if implementation doesn't follow the rules. Verifying the assignment mechanism against the data before trusting the design is essential.

## Tools

**R** — `rdd` (RD estimation), `locfit` (local regression, GCV bandwidth selection), `stargazer` (publication-quality regression tables), plus standard data manipulation and plotting.

## Skills demonstrated

Causal inference (RDD, DiD, fixed effects), assumption testing and falsification (placebo tests, parallel trends, density checks), panel data construction from heterogeneous historical sources, bandwidth/model selection and sensitivity analysis, and communicating a null/failed design transparently.

## Author

**Thomas (Thuy) Nguyen** — [thomas@tnguyen.ca](mailto:thomas@tnguyen.ca)

## References

Key methodological references: Imbens & Lemieux (2008), Imbens & Kalyanaraman (2011), Gelman & Imbens (2014), McCrary (2008), Hahn, Todd & Van der Klaauw (2001). Full bibliography in the thesis PDF.
