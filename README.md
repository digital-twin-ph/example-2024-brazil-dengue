# Dengue in Rio de Janeiro state, Brazil, 2024

A worked example for [Fieldwork](https://github.com/digital-twin-ph/fieldwork), in progress: weekly dengue cases in
the 92 municipalities of Rio de Janeiro state in 2024, Brazil's largest dengue epidemic on record, and the satellite
measures of temperature and vegetation published beside them.

The question, from the practitioner's research note: do satellite-derived temperature and vegetation measures improve
prediction of municipal dengue incidence beyond seasonal patterns and past notifications? It has the outcome, the epidemic curve and where incidence was highest, and the
satellite measurement, land surface temperature per municipality. The fourth workflow relates the two, with lags. Fieldwork
experiment 180.

## The questions

| Workflow | Nodes | What it shows |
| --- | --- | --- |
| How did the 2024 epidemic unfold across the state? | 4 | Notified cases summed over the 92 municipalities, week by week: 51 weeks, the peak in the week starting 18 February |
| Where was incidence highest? | 10 | Notified cases per 100,000 residents in 2024, by municipality, on a map: from 54.6 in Nilópolis to 15,081 in Resende; 2,551 for the state as a whole |
| How hot was the ground in each municipality? | 5 | MODIS land surface temperature over each municipality, every 8-day period of 2024 from Terra and Aqua: the mean of the 1 km cells whose centre is in it that passed the quality rule, how many cells it rests on and how many days were clear. 8,188 rows from 180 files, read in about 4 minutes in a browser |
| Did warmer ground weeks earlier go with more cases? | 24 | Weekly cases beside Terra's land surface temperature 0, 2, 4, 6 and 8 weeks earlier, matched to the 8-day period containing each week's Wednesday, and five Poisson models with municipality and month: rate ratios per 1 °C from 0.963 (4 weeks) to 1.063 (8 weeks). Not an effect: see below |

Both keep to epidemiological year 2024, 31 December 2023 to 28 December 2024, which leaves out the doubled week. The
incidence is a ratio, not a proportion: a notification is an event, and one person can be notified twice. Cases are
summed over the weeks; population, the same every week for a municipality, is taken once.

**Land surface temperature is the ground's, where the sky was clear, not the air's.** Each 8-day value is the mean
of its clear days, often only one or two in the wet season, and each row says how many cells and days it rests on.
Terra passes mid-morning and Aqua early afternoon, so their values are kept apart. MODIS documents its quality bits
and recommends no rule; the one used is stated on every row.

**The lag models are a demonstration, not a finding.** The counts vary 18 to 28 times more than Poisson allows; the
intervals are scaled for that but not for week-to-week correlation, so they are too narrow. With one year and one
epidemic wave, lag and season are hard to tell apart; rainfall and humidity are not in the model; and the ground's
temperature is not the air's.

Each step is in `manual.json`, which Fieldwork shows as this example's Manual tab and plays back as a test.

## Files

| File | What it holds |
| --- | --- |
| `data/dengue-rj-2024.csv` | 4,876 rows: 92 municipalities × 53 weeks, 19 columns as published in DATASET_MULTIMODAL_V8 |
| `geo/rj-municipalities.geojson` | The 92 municipalities from geoBoundaries, with the dataset's IBGE codes |
| `provenance.json` | Where each came from, the digests, and what was found in them |
| `manual.json` | The steps that build both workflows from an empty canvas |
| `fieldwork-project.json.gz` | The Fieldwork project |

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
counts from InfoDengue. Boundaries from geoBoundaries (Runfola et al. 2020), from IBGE, CC BY 3.0 IGO. Land surface temperature from
MODIS MOD11A2 and MYD11A2 v061 (Wan, Hook and Hulley 2021, doi:10.5067/MODIS/MOD11A2.061 and
doi:10.5067/MODIS/MYD11A2.061), NASA LP DAAC, read from Microsoft Planetary Computer; open without restriction. See
`NOTICE.txt`.
