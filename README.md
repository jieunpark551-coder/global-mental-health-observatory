# Global Mental Health Observatory

**An epidemiologic analysis of global mental health workforce distribution,
inequality, and the exploratory ecological association between workforce
capacity and suicide mortality**

---

## Overview

This project characterizes global patterns in mental health workforce capacity
using country-level data from the WHO Global Health Observatory (Mental Health
Atlas 2020), WHO Global Health Estimates, World Bank, and UNDP. The analysis
addresses three interconnected research questions:

**RQ1.** How is mental health workforce capacity distributed globally, across
WHO regions, and across World Bank income groups?

**RQ2.** How unequally is mental health workforce distributed across countries?
(Lorenz curve, Gini coefficient)

**RQ3.** Is national mental health workforce density ecologically associated
with age-standardized suicide mortality rates, after accounting for
socioeconomic development? (exploratory, cross-sectional)

A fourth analytic focus documents data gaps in workforce reporting as a
substantive public health finding.

> **Study design:** Cross-national ecological study. Country is the unit of
> analysis. Findings are descriptive and hypothesis-generating. Causal
> inference is not claimed.

---

## Data Sources

| Variable | Source | Indicator | Year |
|---|---|---|---|
| Psychiatrists per 100,000 | WHO Mental Health Atlas 2020 (2019 data) | MH_6 | 2019 |
| Psychologists per 100,000 | WHO Mental Health Atlas 2020 (2019 data) | MH_7 | 2019 |
| MH Nurses per 100,000 | WHO Mental Health Atlas 2020 (2019 data) | MH_8 | 2019 |
| Age-std. suicide rate | WHO GHE | MHSUICIDEASDR | 2019 |
| GDP per capita PPP | World Bank WDI | NY.GDP.PCAP.PP.KD | 2019 |
| Income Group | World Bank (WDI classification) | NY.GDP.PCAP.PP.KD (extra=TRUE) | Current at run time\* |
| Human Development Index | UNDP HDR | — | 2019 |

\* World Bank income group classification is updated annually (fiscal-year basis). The `WDI()` call with `extra = TRUE` returns the classification current at download time. For reproducibility, the exact classification applied is saved in `data_processed/wb_wdi_clean_2019.csv` and documented in `docs/country_harmonization_table.csv`.

All data are from official sources. No Kaggle, secondary mirrors, or
unverified repositories are used.

---

## Repository Structure

```
global-mental-health-observatory/
├── scripts/
│   ├── 00_setup.R               # Package installation and renv
│   ├── 01_data_acquisition.R    # Data download from all sources
│   ├── 02_harmonization.R       # Merge, clean, exclusion workflow
│   ├── 03_descriptive.R         # Workforce distribution analyses
│   ├── 04_inequality.R          # Lorenz curves and Gini coefficients
│   ├── 05a_modeling_diagnostics.R  # Pre-modeling diagnostics and Model Selection Report
│   └── 05b_workforce_suicide.R     # Final ecological regression models (run after reviewing 05a output)
├── data_raw/                    # Downloaded raw files (not version-controlled)
├── data_processed/              # Cleaned datasets
├── figures/
│   ├── workforce/               # Distribution maps and plots
│   ├── inequality/              # Lorenz curves, Gini comparisons
│   ├── suicide/                 # Workforce-suicide scatterplots
│   └── maps/                    # Choropleth maps
├── outputs/                     # Quarto report
├── docs/
│   ├── data_dictionary.md
│   ├── exclusion_log.csv
│   ├── country_harmonization_table.csv
│   ├── mnar_assessment_workforce.csv
│   ├── data_gap_summary.csv
│   └── indicator_code_verification.csv
└── dashboard/                   # Supplementary interactive dashboard
```

---

## Reproducibility

This project uses `renv` for package management.

```r
# Restore package environment
renv::restore()

# Run full pipeline
source("scripts/00_setup.R")
source("scripts/01_data_acquisition.R")
source("scripts/02_harmonization.R")
source("scripts/03_descriptive.R")
source("scripts/04_inequality.R")
source("scripts/05a_modeling_diagnostics.R")
# Review docs/model_selection_report.md before continuing
# Edit docs/model_decisions.csv if overriding any recommendation
source("scripts/05b_workforce_suicide.R")
```

All analyses are reproducible from raw data downloads.
Session information is saved to `docs/session_info.txt`.

---

## Methodological Notes

- **Ecological fallacy:** Country-level associations cannot be extrapolated
  to individuals.
- **Workforce missingness:** Expected to be MNAR (Missing Not At Random).
  Countries with weakest systems are most likely to have missing data.
  Complete-case analyses are accompanied by systematic missingness
  characterization.
- **Reference years:** Age-standardized suicide rates (ASR), HDI, and GDP PPP correspond to 2019. Mental health workforce indicators were obtained from the most recent country-level observations available through the WHO Mental Health Atlas database. Inspection of the extracted data indicates that workforce observations are predominantly from 2016–2017 (approximately 86% of countries), with a small number of countries reporting 2013–2015 values. Consequently, the analysis should be interpreted as a cross-sectional ecological assessment using the most recent available workforce observations rather than a perfectly year-aligned 2019 dataset. Potential temporal mismatch between workforce indicators and 2019 outcome/covariate data is acknowledged as a study limitation.
- **Total workforce density:** Calculated only when all three cadres
  (psychiatrists + psychologists + MH nurses) are reported. No partial
  summation.

---

## Author

Incoming MHS Student  
Department of Mental Health  
Johns Hopkins Bloomberg School of Public Health  
2026

---

## Citation

*To be completed upon project finalization.*

## License

Data re-use subject to original source terms:
- WHO GHO: CC BY-NC-SA 3.0 IGO
- World Bank WDI: CC BY 4.0
- UNDP HDR: CC BY 3.0 IGO
