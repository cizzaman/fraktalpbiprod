# Constellation Schema Diagram

## Overview
This document visualizes the current data model structure - a constellation schema with 2 facts sharing dimensions.

## Visual Diagram

```
                              CONSTELLATION SCHEMA
                        (Multi-Fact Star with Shared Dimensions)


                                dw dim_date
                             ┌────────────────┐
                             │ date_key (PK)  │
                             │ date           │
                             │ year           │
                             │ month          │
                             │ quarter        │
                      ┌──────│ day_of_week    │──────┐
                      │      │ week_number    │      │
                      │      └────────────────┘      │
                      │                              │
                      │                              │
           date_key (FK)                          date_key (FK)
                      │                              │
      ┌───────────────▼──────────────┐   ┌──────────▼────────────────┐
      │   dw fact_magasin (FACT 1)   │   │  dw fact_weather (FACT 2) │
      ├──────────────────────────────┤   ├───────────────────────────┤
      │ fact_magasin_id (PK)         │   │ fact_weather_id (PK)      │
      │ date_key (FK) ────────────────┼───│ date_key (FK)             │
      │ omraade_key (FK)             │   │ station_key (FK)          │
      │                              │   │                           │
      │ MEASURES:                    │   │ MEASURES:                 │
      │ - magasininnhold_GWh         │   │ - precipitation_1h_sum_mm │
      │ - tilsig_GWh                 │   │ - precipitation_24h_sum_mm│
      │ - tapping_GWh                │   │ - precipitation_daily_mm  │
      │ - fyllingsgrad_prosent       │   │ - measurement_count       │
      │ - magasinkapasitet_GWh       │   │                           │
      │ - netto_endring_GWh          │   │                           │
      │ - utnyttelsesgrad            │   │                           │
      └──────────┬───────────────────┘   └──────────┬────────────────┘
                 │                                  │
        omraade_key (FK)                   station_key (FK)
                 │                                  │
                 │                                  │
      ┌──────────▼──────────────┐       ┌──────────▼───────────────┐
      │   dw dim_omraade        │       │ dw dim_weather_station   │
      ├─────────────────────────┤       ├──────────────────────────┤
      │ omraade_key (PK)        │       │ station_key (PK)         │
      │ omraade_id              │       │ station_id               │
      │ omraade_navn            │       │ station_name             │
      │ omraade_kode            │       │ latitude                 │
      │                         │       │ longitude                │
      └─────────────────────────┘       └──────────────────────────┘


                           ┌───────────────────────┐
                           │     _Measures         │
                           ├───────────────────────┤
                           │ 73 DAX Measures:      │
                           │ - Magasin metrics     │
                           │ - Weather analytics   │
                           │ - Time intelligence   │
                           │ - KPIs & Alerts       │
                           │ (References hidden    │
                           │  fact table columns)  │
                           └───────────────────────┘
```

## Legend

- **(PK)** = Primary Key
- **(FK)** = Foreign Key
- **────** = One-to-Many Relationship

## Current Relationships

1. `dim_date.date_key` ←→ `fact_magasin.date_key`
2. `dim_omraade.omraade_key` ←→ `fact_magasin.omraade_key`
3. `dim_date.date_key` ←→ `fact_weather.date_key`
4. `dim_weather_station.station_key` ←→ `fact_weather.station_key`

## What Makes This a Constellation Schema?

Both fact tables share the `dw dim_date` dimension table, creating a constellation pattern (multiple stars connected by shared dimensions). This is also known as a **galaxy schema** or **fact constellation**.

## Tables Visible in Model View

After optimization, only these 6 tables should be visible in the Model diagram:

1. **_Measures** - Centralized DAX measures library
2. **dw fact_magasin** - Reservoir/magasin fact table
3. **dw fact_weather** - Weather fact table
4. **dw dim_date** - Date dimension (shared)
5. **dw dim_omraade** - Area dimension
6. **dw dim_weather_station** - Weather station dimension

## Hidden Tables

These tables are marked `isHidden` and won't appear in the Model diagram:

- **dw dim_month** - Redundant (covered by dim_date)
- **report vw_extremes** - Pre-calculated analytical view
- **report vw_largest_changes** - Pre-calculated analytical view
- **report vw_period_comparison** - Pre-calculated analytical view
- **report vw_weather_daily** - Pre-calculated analytical view

## Architecture Benefits

1. **Clean Presentation**: Model View shows clear star/constellation pattern
2. **Centralized Measures**: All 73 DAX measures in one organized table
3. **Shared Dimensions**: Efficient reuse of dim_date across both facts
4. **Separation of Concerns**: Technical model vs. user-friendly interface
5. **Maintainability**: Single source of truth for calculations
