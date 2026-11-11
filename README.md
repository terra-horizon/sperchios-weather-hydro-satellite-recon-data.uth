# Sperchios River Satellite Water-Quality Reconstructions with Weather and Hydrology

Four time-series datasets for 32 river tiles in the Sperchios River basin, Greece. They bring together satellite-derived water-quality indicators, ECMWF weather data, and hydraulic or river-network variables at daily and five-day frequencies.

## Files

| File | Frequency | Reconstruction | Data included |
|---|---|---|---|
| `data/sperchios_daily_matern.csv.gz` | Daily | Matérn Gaussian process | Sentinel-2-derived water-quality indicators and Sentinel-3 S8 brightness temperature, with ECMWF weather and river/hydraulic variables. |
| `data/sperchios_daily_hydro_weather.csv.gz` | Daily | Weather-driven state-space model | The same satellite-derived targets, reconstructed using weather inputs; includes ECMWF weather and river/hydraulic variables. Hydraulic variables are present as covariates, but do not drive this reconstruction. |
| `data/sperchios_5day_matern.csv.gz` | Five-day | Matérn Gaussian process | Five-day satellite-target panel with weather and hydraulic variables, prepared with fold-specific reconstruction for model training. |
| `data/sperchios_5day_hydro_weather.csv.gz` | Five-day | Aggregated daily state-space panel | Satellite-derived targets, ECMWF weather, and river/hydraulic variables aggregated from the daily weather-driven dataset. No separate five-day reconstruction is fitted. |

Daily data cover 2016-01-01 through 2026-05-31. Five-day data cover 2016-01-05 through 2026-05-27. The five-day files group non-overlapping five-day periods and use the final day as the row date.

## Data included

- **Satellite:** Sentinel-2-derived indicators for CDOM, chlorophyll-a (`Chl_a`), color, cyanobacteria (`Cya`), dissolved organic carbon (`DOC`), turbidity (`Turb`), and the project-specific water-quality index (`WQI`). Sentinel-3 SLSTR S8 brightness temperature is also included. These are derived time-series values, not raw satellite images.
- **Weather:** ECMWF ERA5-Land variables, including precipitation, runoff, air temperature, solar radiation, soil moisture, and wind.
- **Hydrology and river context:** hydraulic and river-network variables such as discharge, flow change, velocity, depth, channel geometry, and network context.

The dataset files share these broad data sources, but their reconstruction and preprocessing differ. The `hydro_weather` name indicates the combined weather and river variables in the panel; in the daily state-space file, only weather variables drive label reconstruction.

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
