# Health Shocks, Catastrophic Health Expenditure, and the Private-Insurance Buffer: Toward Design-Based (Quasi-Experimental) Algorithmic Recourse

**Authors:** Hyunjeong Kang (Kookmin University, Seoul, Republic of Korea;
[ORCID 0009-0007-9181-4649](https://orcid.org/0009-0007-9181-4649)) and
Munil Yang (corresponding author; Institute for Industrial Policy Studies, Seoul,
Republic of Korea; [ORCID 0009-0004-1493-6691](https://orcid.org/0009-0004-1493-6691);
miyang@ips.or.kr).

> Replication code for a manuscript under review at *Borsa İstanbul Review*
> (single-anonymized peer review). The Korea Health Panel microdata are not
> redistributed; see **Data access** below.

## Overview

This repository contains the full, reproducible analysis pipeline for a study of how
acute **health shocks** drive **catastrophic health expenditure (CHE)** in households,
how **private health insurance (PHI)** buffers that path, and why **naive algorithmic
recourse** built on an observational predictive model gives advice that *contradicts*
the design-based (quasi-experimental) evidence.

All quantities identified under standard difference-in-differences assumptions
(parallel trends, no anticipation) are reported as design-based / quasi-experimental
estimates rather than as unconditional causal effects.

The empirical strategy combines:

1. **Two-way (individual + year) fixed-effects** difference-in-differences,
2. a **heterogeneity-robust estimator** (Callaway & Sant'Anna, 2021) with an acute
   shock (first new hospitalization) as an absorbing treatment, and
3. a **CHE risk model + counterfactual recourse (DiCE)** layer that is contrasted,
   lever by lever, against the design-based (quasi-experimental) estimates.

Headline: holding PHI *raises* predicted CHE risk in the observational model
(adverse-selection signature) but *lowers* it under the design-based (DiD) estimate;
naive recourse therefore almost never recommends acquiring insurance and instead
proposes perverse changes (lowering income, switching to medical-aid eligibility).
This motivates **design-based (DiD-validated) recourse**.

## Repository structure

```
.
├── notebooks/        # executed Jupyter notebooks (one code cell each)
│   ├── 01_build_panel.ipynb
│   ├── 02_descriptive_analysis.ipynb
│   ├── 03_fixed_effects_event_study.ipynb
│   ├── 04_callaway_santanna.ipynb
│   ├── 05_recourse_dice.ipynb
│   ├── 06_recourse_simulation.ipynb
│   ├── 07_net_oop_robustness.ipynb
│   ├── 08_selection_covariate_adjusted.ipynb
│   ├── 09_sun_abraham_comparison.ipynb
│   ├── 10_longitudinal_weights.ipynb
│   └── 11_design_based_recourse.ipynb
├── data/             # NOT included (see Data access); place source files here
│   ├── raw/          # Korea Health Panel SAS files (sas_2019 ... sas_2024)
│   └── processed/    # generated person-year panel (git-ignored)
├── results/
│   ├── figures/      # grayscale figures, 600-dpi PNG and PDF
│   └── tables/       # CSV tables
├── requirements.txt
└── README.md
```

## Data access

The analysis uses the **Korea Health Panel Study (2nd phase, 2019–2024)**. The
microdata are governed by the data provider's terms of use and **cannot be
redistributed**; they are therefore not included in this repository. Researchers may
request the data through the official Korea Health Panel channel and place the wave
folders (`sas_2019` … `sas_2024`) and the codebook under `data/raw/`. Notebook `01`
regenerates the analytic panel from those files.

## Reproduce

```bash
pip install -r requirements.txt
# run the notebooks in order (01 -> 11); 01 regenerates data/processed/
for nb in notebooks/[01]*.ipynb; do
  jupyter nbconvert --to notebook --execute --inplace "$nb"
done
```

Run order and outputs:

| Notebook | Produces |
|---|---|
| 01 build_panel | person-year panel (`data/processed/`) |
| 02 descriptive_analysis | fig01–04, descriptive tables |
| 03 fixed_effects_event_study | TWFE + event study (fig05) |
| 04 callaway_santanna | Callaway–Sant'Anna (fig06) |
| 05 recourse_dice | CHE-risk model + DiCE (fig07–08) |
| 06 recourse_simulation | counterfactual simulation (fig09) |
| 07 net_oop_robustness | net-of-reimbursement CHE (fig10) |
| 08 selection_covariate_adjusted | propensity + covariate-adjusted CS (fig11) |
| 09 sun_abraham_comparison | TWFE / CS / Sun–Abraham (fig12) |
| 10 longitudinal_weights | longitudinally-weighted estimates (fig13) |
| 11 design_based_recourse | design-based constrained recourse (fig14) |

Each notebook is self-contained (a single code cell), writes its figures to
`results/figures/` (PNG + PDF, 600 dpi, grayscale) and its tables to `results/tables/`.

## Methods / key packages

`pyreadstat`, `pandas`, `numpy`, `linearmodels` (PanelOLS), `differences` (ATTgt;
Callaway–Sant'Anna), `pyfixest` (Sun–Abraham / saturated event study),
`scikit-learn` (HistGradientBoosting), `dice-ml` (DiCE recourse),
`seaborn` / `matplotlib`.

## License

Code is released for review purposes. A license will be added upon publication.
