# Complete Data Model - Visual Diagram

## DyrWatt Analytics - Multi-Grain Constellation Schema

```
                                    ┌─────────────────────────────────────────┐
                                    │          dw dim_date                    │
                                    │         (Date Dimension)                │
                                    ├─────────────────────────────────────────┤
                                    │ 🔑 date_key (PK)                        │
                                    │    date                                 │
                                    │    year, month, quarter                 │
                                    │    week_number, day_of_week             │
                                    │    is_weekend, is_holiday               │
                                    └──────────────┬──────────────────────────┘
                                                   │
                    ┌──────────────────────────────┼──────────────────────────────────┐
                    │                              │                                  │
                    │                              │                                  │
                    │                              │                                  │
    ┌───────────────▼────────────────┐  ┌──────────▼──────────────┐  ┌──────────────▼─────────────────┐
    │    dw fact_magasin             │  │   dw fact_weather       │  │  report vw_weather_daily       │
    │    (Reservoir Daily Facts)     │  │  (Weather Daily Facts)  │  │  (Weather Denormalized)        │
    ├────────────────────────────────┤  ├─────────────────────────┤  ├────────────────────────────────┤
    │ 🔑 fact_magasin_id (PK)        │  │ 🔑 fact_weather_id (PK) │  │    Date                        │
    │ 🔗 date_key (FK)               │  │ 🔗 date_key (FK)        │  │    Station Name, Area Code     │
    │ 🔗 omraade_key (FK)            │  │ 🔗 station_key (FK)     │  │    Latitude, Longitude         │
    │                                │  │                         │  │    Year, Quarter, Month, Week  │
    │ 📊 magasininnhold_GWh          │  │ 📊 precipitation_1h_mm  │  │ 📊 Precipitation mm            │
    │ 📊 tilsig_GWh                  │  │ 📊 precipitation_24h_mm │  │ 📊 Precipitation 1h/24h mm     │
    │ 📊 tapping_GWh                 │  │ 📊 precipitation_daily  │  │ 📊 YoY Change                  │
    │ 📊 fyllingsgrad_prosent        │  │ 📊 measurement_count    │  │                                │
    │ 📊 magasinkapasitet_GWh        │  │    loaded_datetime      │  │ Grain: Daily per Station       │
    │ 📊 netto_endring_GWh           │  │                         │  │ Used by: 46 visuals            │
    │ 📊 utnyttelsesgrad             │  │ Grain: Daily per Station│  └────────────────────────────────┘
    │    loaded_datetime             │  │                         │
    │                                │  └─────────┬───────────────┘
    │ Grain: Daily per Area          │            │
    └──────────┬─────────────────────┘            │
               │                                  │
               │                                  │
    ┌──────────▼──────────────┐        ┌─────────▼──────────────────────┐
    │   dw dim_omraade        │        │  dw dim_weather_station        │
    │   (Area Dimension)      │        │  (Weather Station Dimension)   │
    ├─────────────────────────┤        ├────────────────────────────────┤
    │ 🔑 omraade_key (PK)     │        │ 🔑 station_key (PK)            │
    │    omraade_id           │        │    station_id                  │
    │    omraade_navn         │        │    station_name                │
    │    omraade_kode         │        │    latitude, longitude         │
    │    region               │        │    elevation_m                 │
    │    nedborfelt           │        │    municipality                │
    │                         │        │    region                      │
    │ Type: Geographic Areas  │        │                                │
    └─────────┬───────────────┘        │ Type: Weather Stations         │
              │                        └────────────────────────────────┘
              │
              │
              └────────────┐
                           │
              ┌────────────┼────────────────────────────────┐
              │            │                                │
              │            │                                │
    ┌─────────▼────────────▼──────┐  ┌─────────────────────▼────────────────┐
    │  report vw_extremes         │  │  report vw_largest_changes           │
    │  (Annual Extremes)          │  │  (Daily Change Analysis)             │
    ├─────────────────────────────┤  ├──────────────────────────────────────┤
    │    Year                     │  │    Date                              │
    │    Area Code, Area Name     │  │    Year, Month                       │
    │                             │  │    Area Code, Area Name              │
    │ 📊 Max Filling Rate %       │  │                                      │
    │    Max Filling Date         │  │ 📊 Filling Rate %                    │
    │ 📊 Min Filling Rate %       │  │ 📊 Previous Filling Rate %           │
    │    Min Filling Date         │  │ 📊 Daily Change %                    │
    │ 📊 Max Inflow GWh           │  │ 📊 Net Change GWh                    │
    │    Max Inflow Date          │  │    Change Magnitude                  │
    │                             │  │    Change Direction                  │
    │ Grain: Annual per Area      │  │                                      │
    │ Used by: 24 visuals         │  │ Grain: Daily per Area                │
    └─────────────────────────────┘  │ Used by: 31 visuals                  │
                                     └──────────────────────────────────────┘

              ┌────────────────────────────────────────┐
              │  report vw_period_comparison           │
              │  (Monthly YoY Comparison)              │
              ├────────────────────────────────────────┤
              │    Year, Month                         │
              │    Area Code, Area Name                │
              │                                        │
              │ 📊 Avg Filling Rate %                  │
              │ 📊 Avg Content GWh                     │
              │ 📊 Total Inflow/Outflow GWh            │
              │ 📊 Avg Filling Rate % PY               │
              │ 📊 Change Filling Rate %               │
              │ 📊 % Change Inflow                     │
              │                                        │
              │ Grain: Monthly per Area                │
              │ Used by: 13 visuals                    │
              └────────────────────────────────────────┘


                    ┌──────────────────────────────────────┐
                    │         _Measures                    │
                    │    (Centralized Calculations)        │
                    ├──────────────────────────────────────┤
                    │                                      │
                    │ 📐 73 DAX Measures:                  │
                    │                                      │
                    │ • Magasin - Basis (15)               │
                    │   Total Magasininnhold, Kapasitet    │
                    │   Fyllingsgrad, Tilsig, Tapping      │
                    │                                      │
                    │ • Magasin - Time Intelligence (12)   │
                    │   PY, YoY%, MoM%, Trends             │
                    │                                      │
                    │ • Weather Analytics (11)             │
                    │   Total Nedbør, Averages, Extremes   │
                    │                                      │
                    │ • KPIs & Alerts (25)                 │
                    │   Status, Thresholds, Variances      │
                    │                                      │
                    │ • Extreme Values (10)                │
                    │   Max, Min, Date calculations        │
                    │                                      │
                    │ References: Hidden fact columns      │
                    └──────────────────────────────────────┘
```

## Legend

| Symbol | Meaning |
|--------|---------|
| 🔑 | Primary Key |
| 🔗 | Foreign Key (relationship to dimension) |
| 📊 | Measure/Metric column |
| 📐 | Calculated measure (DAX) |
| ──── | Active relationship (one-to-many) |

## Model Summary

### Facts (6 tables at different grains)

1. **dw fact_magasin** - Daily reservoir data per area
2. **dw fact_weather** - Daily weather measurements per station
3. **report vw_extremes** - Annual extreme values per area
4. **report vw_largest_changes** - Daily change analysis per area
5. **report vw_weather_daily** - Daily weather denormalized
6. **report vw_period_comparison** - Monthly comparisons per area

### Dimensions (4 tables)

1. **dw dim_date** - Date attributes (shared by all facts)
2. **dw dim_omraade** - Geographic areas
3. **dw dim_weather_station** - Weather station metadata
4. **dw dim_month** - Monthly helper (hidden from diagram)

### Calculations (1 table)

1. **_Measures** - 73 centralized DAX measures

## Relationships

### Core Constellation Pattern

```
dim_date (1) ──→ (*) fact_magasin
dim_date (1) ──→ (*) fact_weather
dim_omraade (1) ──→ (*) fact_magasin
dim_weather_station (1) ──→ (*) fact_weather
```

### Report View Relationships

```
dim_date (1) ──→ (*) vw_largest_changes (via Date)
dim_date (1) ──→ (*) vw_weather_daily (via Date)
dim_omraade (1) ──→ (*) vw_extremes (via Area Code)
dim_omraade (1) ──→ (*) vw_largest_changes (via Area Code)
dim_omraade (1) ──→ (*) vw_period_comparison (via Area Code)
dim_weather_station (1) ──→ (*) vw_weather_daily (via Station Name)
```

**Total Active Relationships**: 10

## Data Flow Architecture

```
┌────────────────────────────────────────────────────────────┐
│  SQL Server Database: DyrWatt_Analytics                   │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  dw schema (Physical Tables)                               │
│  ├── fact_magasin                                          │
│  ├── fact_weather                                          │
│  ├── dim_date                                              │
│  ├── dim_omraade                                           │
│  └── dim_weather_station                                   │
│                                                            │
│  report schema (SQL Views - Complex Aggregations)          │
│  ├── vw_extremes (Window functions, MAX/MIN)              │
│  ├── vw_largest_changes (LAG, daily deltas)               │
│  ├── vw_weather_daily (Joins + denormalization)           │
│  └── vw_period_comparison (Monthly rollups + YoY)         │
│                                                            │
└──────────────────────┬─────────────────────────────────────┘
                       │
                       │ Power Query M
                       │ (Import Mode)
                       ▼
┌────────────────────────────────────────────────────────────┐
│  Power BI Semantic Model (This Model)                     │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  Imported Tables + Relationships + DAX Measures            │
│  ↓                                                         │
│  114 Visuals across 16 Report Pages                        │
│  ↓                                                         │
│  Published Report                                          │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

## Model Characteristics

### Schema Pattern
**Multi-Grain Constellation Schema** (also called Galaxy Schema)
- Multiple facts at different grain levels
- Shared conformed dimensions (especially dim_date)
- Mix of atomic and aggregated facts

### Grain Levels
1. **Daily**: fact_magasin, fact_weather, vw_largest_changes, vw_weather_daily
2. **Monthly**: vw_period_comparison
3. **Annual**: vw_extremes

### Design Principles Applied
✅ Kimball dimensional modeling methodology
✅ Conformed dimensions (dim_date shared across all facts)
✅ Proper surrogate keys (integer PKs)
✅ Separate measure table (no scattered calculations)
✅ SQL views for complex aggregations (performance)
✅ Denormalization where appropriate (vw_weather_daily)
✅ Multiple grains for different analytical needs

## Usage Statistics

- **Total Visuals**: 114
- **Report Pages**: 16
- **Most Used View**: vw_weather_daily (46 visuals)
- **Measures**: 73 DAX calculations
- **Data Refresh**: Import mode from SQL Server

## Presentation Talking Points

When presenting this model:

1. **"This is a multi-grain constellation schema"**
   - Shows sophistication beyond simple star schema
   - Multiple facts at different detail levels

2. **"We leverage SQL views for performance"**
   - Heavy calculations done in database layer
   - Power BI imports optimized results

3. **"Centralized measure library ensures consistency"**
   - Single source of truth for all calculations
   - Easy maintenance and updates

4. **"Conformed dimensions enable cross-fact analysis"**
   - Can compare reservoir and weather data over time
   - Unified date dimension across all metrics

5. **"114 visuals rely on this architecture"**
   - Production-proven design
   - Supports diverse analytical needs
