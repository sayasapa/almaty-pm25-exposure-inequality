# Almaty PM2.5 Exposure-Inequality — Reproducible Data & Code

Data and analysis code accompanying:

> Sapakova, S.; Sapakov, A.; Auyelbekov, O.; Tukenova, L.; Astaubayeva, G.; Tynymbayev, S.;
> Zhumasheva, Zh.; Suranchiyeva, Z. **Integrating Heterogeneous Sensor Data for Source-Aware
> Air-Quality Analysis in Data-Sparse Urban Networks: A Case Study of Almaty.** *Data* 2026 (under review).

## Contents

| File | Description |
|---|---|
| `almaty_pm25_full_pipeline.ipynb` | Complete, executed pipeline: quality control; seasonal cycle; inequality indices (winter = primary, full-period = secondary); WHO exceedance with a daily-completeness rule; source attribution (distance, NE quadrant, Figure 4); traffic/congestion correlations; Moran's I; school-level exposure; QC-parameter sensitivity; common-window check; regression (R², MAE) and leakage-free classification benchmark with simulated baseline, Bonferroni correction, McNemar tests and confusion matrices; k-NN geographic-proximity diagnostic. |
| `air_quality_data.zip` | Historical low-cost network: 146 stations, hourly PM2.5, 2020–2026 (544,943 raw records); zipped CSV (GitHub 25 MB limit), read directly by the notebook. |
| `city_twin_data.csv` | Real-time source-context collector: 25 co-identified stations, 12 Aug–17 Sep 2026. |
| `district_populations.csv` | Official district populations (1 August 2026). |
| `Supplementary_Materials.docx` | Supplementary Tables S1–S4. |
| `SCHEMA.md` | Column dictionary, `aq_id` ↔ `location_id` mapping, processing conventions. |
| `LICENSE.md` | CC BY 4.0. |

## How to run

1. Install: `pandas numpy scipy scikit-learn matplotlib statsmodels libpysal esda pyproj`.
2. Put all files in one folder and run the notebook top to bottom (`DATA_DIR = '.'`).
3. Runtime is a few minutes on a laptop; random seeds are fixed (`random_state=42`).

## What is and is not reproducible from this package

Every number in the manuscript is printed by the notebook, with one exception: district-level
PM2.5 values (manuscript Table 2) require a station-to-district boundary assignment that is not
included here. District populations are included; the station-to-district step is not.

## License

CC BY 4.0 — see `LICENSE.md`.
