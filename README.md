# Almaty PM2.5 Exposure-Inequality — Reproducible Data & Code

This repository contains the complete dataset and analysis code accompanying:

> Sapakova, S.; Sapakov, A.; Auyelbekov, O.; Tukenova, L.; Astaubayeva, G.; Tynymbayev, S.;
> Zhumasheva, Zh.; Suranchiyeva, Z. **Integrating Heterogeneous Sensor Data for Source-Aware
> Air-Quality Analysis in Data-Sparse Urban Networks: A Case Study of Almaty.** *Data* 2026,
> *11*(x), xxx. https://doi.org/10.3390/dataXXXXXXXX

## Contents

| File | Description |
|---|---|
| `almaty_pm25_full_pipeline.ipynb` | Complete, executed analysis pipeline — quality control, inequality indices (Gini, Theil, Atkinson), source attribution, spatial autocorrelation (Moran's I), school-level exposure, QC-parameter sensitivity analysis, and the leakage-free predictive-modelling benchmark (regression + classification). Reproduces every table and figure in the manuscript. |
| `air_quality_data.csv` | Historical low-cost sensor network: 146 stations, hourly PM2.5, 2020–2026 (544,943 raw records). |
| `city_twin_data.csv` | Real-time source-context collector: 25 co-identified stations, traffic and CHP-geometry features, 12 Aug–17 Sep 2026. |
| `SCHEMA.md` | Full column-by-column data dictionary for both CSV files, including the `aq_id` ↔ `location_id` station mapping. |
| `LICENSE.md` | CC BY 4.0 licence covering the dataset and code. |
| `figure4_source_attribution.png` | Regenerated Figure 4 (source-attribution panel) as produced by the notebook. |

## How to run

1. Clone this repository (or download the ZIP).
2. Install dependencies: `pandas`, `numpy`, `scipy`, `scikit-learn`, `matplotlib`, `libpysal`, `esda`, `statsmodels`.
3. Open `almaty_pm25_full_pipeline.ipynb` in Jupyter and run all cells top to bottom — both CSV files are expected in the same directory (`DATA_DIR = '.'` in the first code cell).
4. All numbers, tables, and figures reported in the manuscript are reproduced from these two input files with no additional data required.

## Known reproducibility note

The quality-control pipeline in this notebook retains 39 stations and 442,322 records from the
146-station raw file — this exact figure is what the manuscript reports and is fully reproducible
from this code. See `SCHEMA.md` for the full per-column data dictionary.

## Citation

If you use this dataset or code, please cite both the article above and this repository:

> Sapakova, S. et al. Almaty PM2.5 Exposure-Inequality Analysis: Data and Code [Data set].
> Zenodo, 2026. https://doi.org/[ZENODO-DOI-PENDING]

## License

CC BY 4.0 — see `LICENSE.md`.
