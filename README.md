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
| Did warmer ground weeks earlier go with more cases? | 33 | Weekly cases beside Terra's land surface temperature 0, 2, 4, 6 and 8 weeks earlier, matched to the 8-day period containing each week's Wednesday, and Poisson models with municipality and month, drawn as a lag-response plot. Two leads, the temperature 2 and 4 weeks *after* each week, are a falsification test, and it fails: 1.061 per 1 °C 2 weeks after against 1.063 at 8 weeks before. Not an effect: see below |
| Across sixteen years, did warmer ground weeks earlier go with more cases? | 43 | Weekly cases 2010–2025 beside Terra's land surface temperature at the same lags and leads, each week compared only with weeks of the same municipality, month and year, adjusted for that week's rainfall: 1.020 per 1 °C at 8 weeks before and 1.021 at 6, 0.989 at 4, 1.005 for both leads. The leads are a quarter the size of 2024's. Not an effect: see below |

The first four keep to epidemiological year 2024, 31 December 2023 to 28 December 2024, which leaves out the doubled week. The
incidence is a ratio, not a proportion: a notification is an event, and one person can be notified twice. Cases are
summed over the weeks; population, the same every week for a municipality, is taken once.

**Land surface temperature is the ground's, where the sky was clear, not the air's.** Each 8-day value is the mean
of its clear days, often only one or two in the wet season, and each row says how many cells and days it rests on.
Terra passes mid-morning and Aqua early afternoon, so their values are kept apart. MODIS documents its quality bits
and recommends no rule; the one used is stated on every row.

**The lag models are a demonstration, not a finding, and the leads show why.** Temperature after the cases cannot have caused them, yet it goes with them as strongly as temperature 8 weeks before: with one year and one epidemic wave, these models trace the season. The counts vary 18 to 28 times more than Poisson allows; the
intervals are scaled for that but not for week-to-week correlation, so they are too narrow. With one year and one
epidemic wave, lag and season are hard to tell apart; rainfall and humidity are not in the model; and the ground's
temperature is not the air's. In the plot each point is its own model, not adjusted for the others.

**The sixteen-year models are a better test, and still not a finding.** Comparing weeks only within a municipality,
month and year removes each year's epidemic timing, and the leads fall from 1.061 in 2024 alone to 1.005. They still
do not reach 1, and the intervals allow nothing for correlation between neighbouring weeks, so they are too narrow.
The dip at 4 weeks has no mechanism behind it. Rainfall is the dataset's: CHIRPS at one grid cell per municipality,
summed Monday to Sunday, with a missing day counted as 0. Twelve weeks the file holds twice, at years' edges, are left
out, 2,208 rows.

The sixteen-year table is `data/dengue-rj-2010-2025.csv`, the same 19 columns for every year in the file.

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
