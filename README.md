# Analysis materials

This repository contains the analysis code for:

> Evaluating Responsible-Gambling Message Framing Across Segments of High-Loss Online Sports Bettors: Evidence From Two Naturalistic Randomized Field Experiments
> Psychology of Addictive Behaviors (submitted)

## Data availability

Participant-level data used in this study were provided by Norsk Tipping (the Norwegian state-owned gambling operator) under a data-sharing agreement that does not permit public redistribution. Raw or cleaned participant data are not included in this repository.

The notebooks below take the analysis-ready files (`P10_final.csv` for
Experiment 1, `P11_final.csv` for Experiment 2) as their starting point and
reproduce every reported statistic from that point forward, so that the
analytic pipeline is transparent and reproducible on an equivalent dataset
with the same column structure.

## Expected data schema

Each notebook expects a CSV with (at minimum) the following columns, one row
per participant:

| Column | Description |
|---|---|
| `PARTICIPANT_ID` | Unique participant identifier |
| `CONTROL_GROUP` | Arm code (`AA`, `BB`, `CC`, `EE`, `FF`, `CONTROLGROUP`) |
| `BIRTH_YEAR`, `GENDER` | Demographics |
| `ACTION`, `VALUE_ACTION` | Whether the participant lowered their limit, and the amount selected (NOK) |
| `RATING_R`, `RATING_U`, `RATING_D` | Optional post-message ratings (1-5): relevance, usefulness, disruptiveness |
| `SUM_DEPOSITS_W_PRE_1..12`, `SUM_DEPOSITS_W_POST_1..12,40..51` | Weekly deposit sums (NOK), pre- and post-intervention |
| `NR_DEPOSITS_W_*` | Weekly deposit counts, same window structure |
| `SUM_WAGERS_DAILY_W_*` | Weekly wager sums (NOK) |
| `N_WAGERS_DAILY_W_*` | Weekly wager counts |
| `NR_DAYS_W_*` | Weekly gambling-day counts |
| `SUM_TL_W_*` | Weekly theoretical loss (NOK) |

Post-intervention weeks are numbered 1-12 and 40-51 relative to each
participant's own allocation date (not a shared calendar date); weeks 13-39
were not extracted by the operator. Arm codes map to manuscript labels as
`AA→A, BB→B, CC→C, EE→E, FF→F, CONTROLGROUP→Control`. In Experiment 1, arms B
and D (play-break conditions) are outside the scope of this analysis and are
not represented in the arm map used here.

## Contents

| File | Reproduces |
|---|---|
| `01_baseline_balance.ipynb` | Table 4, Tables S2-S3 |
| `02_limit_lowering_analysis.ipynb` | Table S4 |
| `03_gambling_outcomes_analysis.ipynb` | Table 5, Tables S6-S8 |
| `04_permutation_tests.ipynb` | Table S1 |
| `05_ratings_analysis.ipynb` | Table S9 |

Each notebook can be run independently given `P10_final.csv` and
`P11_final.csv`.

## Software

Developed and run with Python 3.12. Package versions:

- pandas
- numpy
- scipy (`>=1.11`, for `scipy.stats.false_discovery_control` and `scipy.stats.contingency.association`)
- scikit-posthocs

Install with:
```
pip install pandas numpy scipy scikit-posthocs
```

## Notes on statistical approach

Group differences in gambling outcomes were evaluated with Kruskal-Wallis
tests (rank-based, robust to the skewed and zero-inflated distributions
typical of gambling behavioural data), with Benjamini-Hochberg correction
applied within predefined families (18 outcome-by-period comparisons per
experiment; 6 baseline outcomes; 3 rating dimensions), each corrected
separately per experiment. Dunn's post-hoc tests (Holm correction) followed
significant omnibus results. Effect sizes are reported as rank-biserial
correlations. Permutation-based Kruskal-Wallis tests (10,000 resamples) were
used to confirm that the high frequency of tied values did not distort
significance for outcomes surviving correction (Notebook 04).
