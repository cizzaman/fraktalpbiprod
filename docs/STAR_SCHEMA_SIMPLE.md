# ⭐ STJERNEMODELL - ENKEL FORKLARING

**DyrWatt Analytics - Magasin og Vær Dashboard**

---

## 🎯 Hva er en Stjernemodell?

En **stjernemodell** (star schema) er den mest brukte datamodelleringsmetoden i Business Intelligence. Den heter "stjerne" fordi den ser ut som en stjerne når du tegner den:

```
        dim_date
            |
            |
    dim_omraade ←--→ FACT TABLE ←--→ dim_weather_station
                   (i midten)
```

### Hovedkomponenter

#### 🔵 DIMENSJONER (Hvem, Hva, Når, Hvor)
Beskrivende informasjon - svarer på "hvem, hva, når, hvor?"

- **dim_date** - Kalenderdimensjon (datoer, år, måned, kvartal)
- **dim_omraade** - Strømområder (NO1, NO2, NO3, NO4, NO5)
- **dim_weather_station** - Værstasjoner (10 stasjoner i Norge)

#### 🟢 FAKTA-TABELLER (Tall og Målinger)
Numeriske målinger - svarer på "hvor mye?"

- **fact_magasin** - Daglige magasinmålinger (450 rader)
- **fact_weather** - Daglige værmålinger (900 rader)
- **fact_magasin_monthly** - Månedlige aggregeringer (20 rader)

#### 🟡 ANALYTISKE VIEWS (Pre-beregnede Rapporter)
Kombinerer fakta + dimensjoner for rask rapportering:

- **vw_area_combined** - Magasin + vær kombinert
- **vw_monthly_summary** - Månedlige KPIer
- **vw_extremes** - Maks/min verdier
- **vw_largest_changes** - Største endringer
- **vw_period_comparison** - Year-over-Year sammenligninger
- **vw_weather_daily** - Daglig vær med YoY

---

## 📊 Hvordan Fungerer Det?

### Eksempel: "Vis fyllingsgrad for NO1 i august 2025"

1. **Velg område** fra `dim_omraade` → NO1
2. **Velg måned** fra `dim_date` → August 2025
3. **Hent data** fra `fact_magasin` → Alle målinger for NO1 i august
4. **Vis resultat** → Gjennomsnittlig fyllingsgrad for NO1 i august

### Fordeler med Stjernemodell

✅ **Enkel å forstå** - Intuitivt design
✅ **Rask ytelse** - Få joins, optimalisert for queries
✅ **Fleksibel** - Lett å legge til nye dimensjoner eller fakta
✅ **Skalerbar** - Fungerer for små og store datamengder
✅ **BI-standard** - Alle BI-verktøy støtter dette

---

## 🔗 Relasjoner i Vår Modell

### Fakta-tabeller → Dimensjoner

```
fact_magasin:
├── date_key → dim_date.date_key
└── omraade_key → dim_omraade.omraade_key

fact_weather:
├── date_key → dim_date.date_key
└── station_key → dim_weather_station.station_key

fact_magasin_monthly:
├── month_key → dim_month.month_key
└── omraade_key → dim_omraade.omraade_key
```

### Analytiske Views → Dimensjoner

```
vw_area_combined:
├── Date → dim_date.date
└── Area Code → dim_omraade.omraade_kode

vw_extremes:
└── Area Code → dim_omraade.omraade_kode

vw_largest_changes:
├── Date → dim_date.date
└── Area Code → dim_omraade.omraade_kode

vw_period_comparison:
└── Area Code → dim_omraade.omraade_kode

vw_monthly_summary:
└── Area Code → dim_omraade.omraade_kode

vw_weather_daily:
├── Date → dim_date.date
└── Station Name → dim_weather_station.station_name
```

**Total: 15 relasjoner**

---

## 📈 Datavolum

| Type | Tabeller | Rader | Formål |
|------|----------|-------|--------|
| **Dimensjoner** | 3 | ~4,000 | Beskrivende data |
| **Fakta** | 3 | ~1,370 | Målinger |
| **Views** | 6 | ~1,900 | Pre-beregnede rapporter |
| **TOTALT** | 12 | ~7,270 | Komplett modell |

---

## 🎨 Visualiseringer som Bruker Modellen

### Side 1-3: Bruker Fakta-tabeller
- KPI cards
- Trend charts
- Heatmaps

### Side 4-7: Bruker Analytiske Views
- YoY comparisons (`vw_period_comparison`)
- Extreme value analysis (`vw_extremes`, `vw_largest_changes`)
- Weather maps (`vw_weather_daily`)
- Combined analysis (`vw_area_combined`)

---

## ✨ Hvorfor Denne Strukturen?

1. **Ytelse** - Analytiske views reduserer datamengde med 95% for månedlige rapporter
2. **Enkelhet** - Visuals kan bruke views direkte uten komplekse DAX
3. **Fleksibilitet** - Både detaljerte (fakta) og aggregerte (views) analyser tilgjengelig
4. **Best Practice** - Standard BI-arkitektur som alle forstår

---

**Se `STAR_SCHEMA_DETAILED.md` for teknisk dybdeforklaring**
**Åpne `star_schema_visualization.html` i nettleser for visuell modell**
