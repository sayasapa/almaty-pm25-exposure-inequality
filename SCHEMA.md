# Data Schema and Dictionary

## `air_quality_data.csv` — Historical low-cost sensor network (146 stations, 2020–2026)

| Column | Type | Description |
|---|---|---|
| `datetime` | ISO 8601 string (UTC) | Hourly observation timestamp |
| `location_id` | integer | Unique station identifier. Joins to `city_twin_data.csv` via `aq_id` for the 25 co-identified stations (see `aq_id mapping` below) |
| `pm25` | float, µg/m³ | PM2.5 mass concentration (raw, pre-QC) |
| `pm1` | float, µg/m³ | PM1 mass concentration (sparse; not used in analysis) |
| `pm10` | float, µg/m³ | PM10 mass concentration (sparse; not used in analysis) |
| `relativehumidity` | float, % | Relative humidity (sparse; not used in analysis) |
| `temperature` | float, °C | Ambient temperature (sparse; not used in analysis) |
| `um003` | float | Particle-count channel (sparse; not used in analysis) |
| `name` | string | Station site name/label as assigned by the provider |
| `lat`, `lon` | float, decimal degrees (WGS84) | Station coordinates |
| `provider_name` | string | Data provider: `AirNow` (1 station, reference-grade), `Clarity` (low-cost network), `AirGradient` (low-cost network) |

## `city_twin_data.csv` — Real-time source-context collector (25 co-identified stations, 12 Aug–17 Sep 2026)

| Column | Type | Description |
|---|---|---|
| `timestamp_utc` | ISO 8601 string (UTC) | Poll-cycle timestamp (irregular interval; median 40 min) |
| `cycle_id` | string | Identifier for one polling cycle across all 25 stations |
| `station_id` | string | Human-readable station code |
| `station_name` | string | Station site name |
| `aq_id` | integer | **Joins to `location_id` in `air_quality_data.csv`** — identical physical station, verified by independent coordinate match (max discrepancy 0.6 m) |
| `lat`, `lon` | float, decimal degrees (WGS84) | Station coordinates |
| `speed_kmh`, `free_flow_speed_kmh` | integer, km/h | Current and free-flow traffic speed (TomTom API) |
| `current_travel_time_s`, `free_flow_travel_time_s` | integer, seconds | Current and free-flow travel time on the reference road segment |
| `density_cars_per_km` | float | Estimated vehicle density |
| `cars_per_hour` | float | Estimated traffic volume |
| `congestion_percent` | float, % | Congestion index (0 = free-flow, 100 = maximum) |
| `co2_kg_per_hour` | float | Estimated traffic-related CO2 emission rate |
| `pm25_ugm3`, `pm10_ugm3`, `no2_ugm3`, `o3_ugm3`, `co_ugm3`, `so2_ugm3`, `nh3_ugm3` | float, µg/m³ | Real-time air-quality readings (OpenWeatherMap + WAQI); **not used for the historical exposure analysis**, which relies on `air_quality_data.csv` instead |
| `air_index`, `waqi_aqi` | float / integer | Composite/aggregate air-quality indices reported by the source APIs (not equivalent to the manuscript's Gini or exposure-inequality indices) |
| `population_exposure_index` | float | Per-record composite index from the source API; time-varying, tracks `pm25_ugm3` closely (not a district-population weighting; see Limitations) |
| `temperature_c`, `feels_like_c`, `humidity_percent`, `pressure_hpa`, `wind_speed_ms`, `wind_deg`, `clouds_percent`, `visibility_m`, `weather_desc` | various | Real-time meteorology (OpenWeatherMap) |
| `dist_to_chp_km` | float, km | Precomputed geodesic distance from the station to the CHP plant |
| `heating_season` | integer (0/1) | Flag for the heating season at time of the poll |
| `downwind_of_chp` | integer (0/1) | **Time-varying** flag derived from live `wind_deg`; changes across poll cycles for the same station. This is the live wind-based indicator excluded from the main analysis (Section 4.3) due to temporal mismatch with the historical, winter-dominated PM2.5 record — distinct from the static geometric NE-quadrant indicator used in Section 2.5/3.3 |
| `traffic_source`, `air_source`, `weather_source` | string | Data-provenance flags per record. All 16,650 records in the released file show `traffic_source=tomtom`, `air_source=openweathermap+waqi`, `weather_source=openweathermap` (0% fallback) |

## `aq_id` ↔ `location_id` mapping

All 25 `aq_id` values in `city_twin_data.csv` are identical to `location_id` values in `air_quality_data.csv` (same integer). Correspondence independently verified by comparing each station's own `lat`/`lon` in both files (see notebook Section 5).

## Additional input file

| File | Contents |
|---|---|
| `district_populations.csv` | Population of Almaty's eight districts as of 1 August 2026 (Bureau of National Statistics of the Republic of Kazakhstan); used for population weighting (manuscript Table 2). |

## Derived files (written by the notebook when run)

| File | Contents |
|---|---|
| `stations_with_geometry.csv` | Per-station bearing, distance to CHP, quadrant, full-period and winter PM2.5 means (39 QC-retained stations) |
| `table5_final.csv` | Classification benchmark (manuscript Table 5): accuracy, 95% CI, confusion-matrix counts, p-value vs. simulated baseline |
| `table_s3_sensitivity.csv` | QC-parameter sensitivity grid (Supplementary Table S3) |
| `table_s4_schools.csv` | School-level winter PM2.5 with administrative district (Supplementary Table S4) |
| `figure4_source_attribution.png` | Figure 4 as produced by the notebook |

## Processing conventions

- Daily means are computed in Almaty local time (UTC+5); a station-day is retained only if it has at least 18 of 24 hourly records (≥75% completeness).
- "City-wide mean" = mean of station-level means.
- Spatial weights for Moran's I use UTM zone 43N projected coordinates (EPSG:32643).
