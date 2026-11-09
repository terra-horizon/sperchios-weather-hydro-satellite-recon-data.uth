# Sperchios River Reconstruction Data

Four daily and five-day datasets for the Sperchios River basin in Greece. Each file combines satellite-derived water-quality indicators with weather and river-context variables for 32 tiles.

## Files

| File | Frequency | Rows | What it contains |
|---|---|---:|---|
| `data/sperchios_daily_matern.csv.gz` | Daily | 121,728 | Daily satellite targets with Matérn GP reconstruction. |
| `data/sperchios_daily_hydro_weather.csv.gz` | Daily | 121,728 | Daily satellite targets with weather-driven state-space reconstruction and river/hydraulic fields. |
| `data/sperchios_5day_matern.csv.gz` | Five-day | 24,320 | Five-day Matérn reconstruction dataset. |
| `data/sperchios_5day_hydro_weather.csv.gz` | Five-day | 24,320 | Five-day summary derived from the daily weather-driven dataset. |

Daily data cover 2016-01-01 through 2026-05-31. Five-day data cover 2016-01-05 through 2026-05-27. The five-day files group non-overlapping five-day periods and use the final day as the row date.

## Reading the data

The files are gzip-compressed CSVs. For example, with Python and pandas:

```python
import pandas as pd

df = pd.read_csv("data/sperchios_daily_matern.csv.gz")
print(df[["date", "tile_id", "WQI"]].head())
```

See [`metadata/data_dictionary.csv`](metadata/data_dictionary.csv) for column descriptions. [`metadata/dataset_manifest.json`](metadata/dataset_manifest.json) lists each file's size and date range.

## What the columns mean

Satellite-derived indicators include `CDOM`, `Chl_a`, `Color`, `Cya`, `DOC`, `Turb`, and `WQI`. `WQI` is a project-specific index. `s3_s8_brightness_temperature_c` is Sentinel-3 brightness temperature; it is not a validated river-temperature measurement.

Columns beginning with `ecmwf_` contain weather or land-surface variables. Columns beginning with `hydro_` contain hydraulic or river-network variables. Observation, availability, and interpolation flags indicate how values were obtained; their exact meaning can vary by file. Null values indicate unavailable or unfilled data.

## Notes for use

- The four files use different preprocessing and reconstruction rules, so they are not a controlled, like-for-like comparison.
- In the daily `hydro_weather` file, weather drives the state-space reconstruction. Hydraulic variables are included in the table but do not drive that reconstruction. The five-day file is an aggregation of the daily file, not an independently fitted model.
- Daily Matérn values are retrospective: the reconstruction can use observations from later dates. It fills only short gaps with observations on both sides; longer gaps remain empty. Do not treat it as a causal forecasting input.
- An internal source review identified the 2016–2018 hydraulic block as test/smoke-test data. Verify those values before interpreting them as production simulations or real-world measurements.
- These are derived time-series tables, not raw satellite images. The packaged files have not been verified as the exact data used for the paper's reported results.

## Attribution and license

This dataset is licensed under [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/). Please provide attribution when sharing or adapting the dataset, and distribute adaptations under the same license.

## Citation

Paper: **TBC**  
DOI: **TBC**
