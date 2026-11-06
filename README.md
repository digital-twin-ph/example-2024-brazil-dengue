# Dengue in Rio de Janeiro state, Brazil, 2024

A worked example for [Fieldwork](https://github.com/digital-twin-ph/fieldwork), in progress: weekly dengue cases in
the 92 municipalities of Rio de Janeiro state in 2024, Brazil's largest dengue epidemic on record, and the satellite
measures of temperature and vegetation published beside them.

The question, from the practitioner's research note: do satellite-derived temperature and vegetation measures improve
prediction of municipal dengue incidence beyond seasonal patterns and past notifications? This repository starts with
the outcome: the epidemic curve and where incidence was highest. Fieldwork experiment 180.

## Files

| File | What it holds |
| --- | --- |
| `data/dengue-rj-2024.csv` | 4,876 rows: 92 municipalities × 53 weeks, 18 columns as published in DATASET_MULTIMODAL_V8 |
| `geo/rj-municipalities.geojson` | The 92 municipalities from geoBoundaries, with the dataset's IBGE codes |
| `provenance.json` | Where each came from, the digests, and what was found in them |

## Reading the data

- **`casos` is notified cases** as InfoDengue reports them, by epidemiological week (`SE`) and municipality
  (`codigo_ibge`, the IBGE code). `data_iniSE` is the Sunday the week starts.
- **Week 202401 is missing, and week 202501 is there twice.** The week starting 29 December 2024 appears twice for
  every municipality, labelled `semana` 1 and `anio` 2024. The two rows differ only in rainfall, one of them 0. They
  are kept as published.
- **The satellite columns are monthly.** `LST_Day_mean`, `LST_Night_mean`, `NDVI_mean` and `EVI_mean` take 11 or 12
  values per municipality over the year, repeated across the weeks of each month. How they were computed is not stated
  in the dataset.

## Sources

Data from Sánchez-Marqués R, Sánchez JS, DATASET_MULTIMODAL_V8, Zenodo 2026, doi:10.5281/zenodo.22029053, CC BY 4.0;
counts from InfoDengue. Boundaries from geoBoundaries (Runfola et al. 2020), from IBGE, CC BY 3.0 IGO. See
`NOTICE.txt`.
