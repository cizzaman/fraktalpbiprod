# Visualiseringer - Step-by-Step Guide

**Prosjekt**: DyrWatt AS - Magasin og Vær Analytics Dashboard
**Antall Rapportsider**: 7
**Estimert tid**: 3-4 timer
**Sist oppdatert**: 2025-11-19

---

## Innholdsfortegnelse

1. [Oversikt](#oversikt)
2. [Før du starter](#før-du-starter)
3. [Side 1: Executive Dashboard](#side-1-executive-dashboard)
4. [Side 2: Magasin Detaljanalyse](#side-2-magasin-detaljanalyse)
5. [Side 3: Tilsig & Tapping](#side-3-tilsig--tapping)
6. [Side 4: Vær & Nedbør](#side-4-vær--nedbør)
7. [Side 5: Year-over-Year](#side-5-year-over-year)
8. [Side 6: Ekstremverdier](#side-6-ekstremverdier)
9. [Side 7: Månedlig Oppsummering](#side-7-månedlig-oppsummering)
10. [Designprinsipper](#designprinsipper)
11. [Interaktivitet & Filters](#interaktivitet--filters)
12. [Ytelsesoptimalisering](#ytelsesoptimalisering)

---

## Oversikt

Denne guiden tar deg gjennom opprettelsen av alle 7 rapportsider i DyrWatt Analytics dashboardet. Hver side har spesifikke formål og målgrupper.

### Rapportstruktur

| # | Side | Formål | Målgruppe | Kompleksitet |
|---|------|--------|-----------|--------------|
| 1 | Executive Dashboard | High-level KPIer og oversikt | Ledelse | Lav |
| 2 | Magasin Detaljanalyse | Detaljert magasinanalyse | Driftsoperatører | Medium |
| 3 | Tilsig & Tapping | Vannbalanse og produksjon | Analyseansvarlige | Medium |
| 4 | Vær & Nedbør | Værmønster og korrelasjon | Væranalytikere | Medium |
| 5 | Year-over-Year | Historiske sammenligninger | Strategisk planlegging | Medium |
| 6 | Ekstremverdier | Alarmer og kritiske verdier | Risk management | Lav |
| 7 | Månedlig Oppsummering | Aggregerte månedlige data | Rapportering | Lav |

---

## Før du starter

### Sjekkliste

- [ ] Power BI Desktop installert (versjon 2023.11 eller nyere)
- [ ] `.pbip` fil åpnet (`C:\Users\chris\Desktop\fraktalpowerbi\theme.pbip`)
- [ ] Alle 9 tabeller importert fra SQL Server
- [ ] Alle 5 relasjoner opprettet
- [ ] Alle 60 DAX measures implementert
- [ ] Datamodellen validert (ingen feil i Model View)

### Generelle Innstillinger

1. Gå til **File → Options and settings → Options**
2. Under **Current File → Data Load**:
   - Deaktiver "Auto date/time" (vi har egen date-dimensjon)
3. Under **Current File → Report settings**:
   - Sett "Default visual interaction" til "Cross-highlight"

---

## Side 1: Executive Dashboard

### Formål
High-level oversikt med de viktigste KPIene for daglig ledelsesrapportering.

### Layout Overview
```
┌─────────────────────────────────────────────────────────────┐
│  DYRWATT AS - EXECUTIVE DASHBOARD                           │
├─────────────────────────────────────────────────────────────┤
│  [KPI 1]    [KPI 2]    [KPI 3]    [KPI 4]    [KPI 5]       │
│                                                              │
│  ┌──────────────────────┐  ┌──────────────────────────┐    │
│  │ Fyllingsgrad Trend   │  │ Magasin per Område       │    │
│  │ (Line Chart)         │  │ (Clustered Column)       │    │
│  └──────────────────────┘  └──────────────────────────┘    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Fyllingsgrad per Område - Trend (Line Chart)         │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Visual 1.1: KPI Card - Gjennomsnittlig Fyllingsgrad

**Steg**:
1. Gå til **Report View**
2. Velg **Card** visual fra Visualizations pane
3. Dra felter:
   - **Fields**: `[Gjennomsnittlig Fyllingsgrad (%)]`
4. Formater:
   - **Callout value** → Font size: `40pt`
   - **Callout value** → Color: Conditional formatting basert på verdi
     - < 30%: Rød `#E74C3C`
     - 30-50%: Oransje `#F39C12`
     - 50-80%: Grønn `#27AE60`
     - \> 80%: Blå `#3498DB`
   - **Category label**: "Gjennomsnittlig Fyllingsgrad"
   - **Category label** → Font size: `12pt`
5. Posisjon: Øverst til venstre

---

### Visual 1.2: KPI Card - Total Magasininnhold

**Steg**:
1. Velg **Card** visual
2. Dra felter:
   - **Fields**: `[Total Magasininnhold (GWh)]`
3. Formater:
   - **Callout value** → Font size: `40pt`
   - **Callout value** → Color: `#2C3E50` (Mørkeblå)
   - **Category label**: "Total Magasininnhold"
4. Posisjon: Nest øverst

---

### Visual 1.3: KPI Card - Kritisk Alert Count

**Steg**:
1. Velg **Card** visual
2. Dra felter:
   - **Fields**: `[Kritisk Alert Count]`
3. Formater:
   - **Callout value** → Font size: `40pt`
   - **Callout value** → Color: Conditional formatting
     - 0: Grønn
     - \> 0: Rød
   - **Category label**: "Kritiske Områder"
4. Posisjon: Midten

---

### Visual 1.4: KPI Card - YoY Endring

**Steg**:
1. Velg **Card** visual
2. Dra felter:
   - **Fields**: `[YoY Endring - Fyllingsgrad (%)]`
3. Formater:
   - **Callout value** → Font size: `40pt`
   - **Callout value** → Display units: None
   - **Callout value** → Color: Conditional formatting
     - Positiv: Grønn
     - Negativ: Rød
   - **Category label**: "YoY Endring"
4. Posisjon: Nest øverst til høyre

---

### Visual 1.5: KPI Card - Netto Endring

**Steg**:
1. Velg **Card** visual
2. Dra felter:
   - **Fields**: `[Netto Endring (GWh)]`
3. Formater:
   - **Callout value** → Font size: `40pt`
   - **Callout value** → Color: Conditional formatting
     - Positiv: Blå (fylles)
     - Negativ: Oransje (tømmes)
   - **Category label**: "Netto Endring (Tilsig-Tapping)"
4. Posisjon: Øverst til høyre

---

### Visual 1.6: Line Chart - Fyllingsgrad Trend

**Steg**:
1. Velg **Line chart** visual
2. Dra felter:
   - **X-axis**: `dim_date[date]`
   - **Y-axis**: `[Gjennomsnittlig Fyllingsgrad (%)]`
   - **Secondary Y-axis**: `[MA7 Fyllingsgrad (%)]`
3. Formater:
   - **Title**: "Fyllingsgrad Trend - Siste 90 Dager"
   - **X-axis** → Type: Continuous
   - **Y-axis** → Start: 0, End: 100
   - **Line** → Stroke width: 2px
   - **Legend**: Top center
4. Legg til:
   - **Constant line** på Y-axis ved 70% (målnivå) - Grønn stiplet
   - **Constant line** på Y-axis ved 30% (kritisk) - Rød stiplet
5. Posisjon: Venstre side, under KPI cards

---

### Visual 1.7: Clustered Column Chart - Magasin per Område

**Steg**:
1. Velg **Clustered column chart**
2. Dra felter:
   - **X-axis**: `dim_omraade[omraade_kode]`
   - **Y-axis**: `[Total Magasininnhold (GWh)]`
3. Formater:
   - **Title**: "Magasininnhold per Område"
   - **Data labels**: On
   - **Data colors**: Bruk distinkte farger per område
     - NO1: `#E74C3C` (Rød)
     - NO2: `#3498DB` (Blå)
     - NO3: `#2ECC71` (Grønn)
     - NO4: `#F39C12` (Oransje)
     - NO5: `#9B59B6` (Lilla)
4. Posisjon: Høyre side, under KPI cards

---

### Visual 1.8: Multi-Line Chart - Fyllingsgrad per Område

**Steg**:
1. Velg **Line chart**
2. Dra felter:
   - **X-axis**: `dim_date[date]`
   - **Y-axis**: `[Gjennomsnittlig Fyllingsgrad (%)]`
   - **Legend**: `dim_omraade[omraade_kode]`
3. Formater:
   - **Title**: "Fyllingsgrad Trend per Område"
   - **Line colors**: Samme som Visual 1.7
   - **Y-axis**: 0-100%
4. Posisjon: Bunn, full bredde

---

### Visual 1.9: Slicer - Datofilter

**Steg**:
1. Velg **Slicer** visual
2. Dra felter:
   - **Field**: `dim_date[date]`
3. Formater:
   - **Slicer settings** → Style: Between
   - **Slicer settings** → Default: Last 90 days
4. Posisjon: Øverst til høyre (over KPI 5)

---

## Side 2: Magasin Detaljanalyse

### Formål
Detaljert analyse av magasindata med fokus på granulære målinger og heatmaps.

### Layout Overview
```
┌─────────────────────────────────────────────────────────────┐
│  MAGASIN DETALJANALYSE                                       │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────────────┐  ┌──────────────────────────┐    │
│  │ Fyllingsgrad Heatmap │  │ Gauges per Område        │    │
│  │ (Matrix)             │  │ (Gauge Charts x5)        │    │
│  └──────────────────────┘  └──────────────────────────┘    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Fyllingsgrad Distribusjon (Histogram)                │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────┐  ┌──────────┐  ┌────────────────────────┐   │
│  │ Top Area │  │ Low Area │  │ Statistics Table       │   │
│  └──────────┘  └──────────┘  └────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### Visual 2.1: Matrix - Fyllingsgrad Heatmap

**Steg**:
1. Velg **Matrix** visual
2. Dra felter:
   - **Rows**: `dim_date[date]` (gruppér etter uke eller måned)
   - **Columns**: `dim_omraade[omraade_kode]`
   - **Values**: `[Gjennomsnittlig Fyllingsgrad (%)]`
3. Formater:
   - **Cell elements** → Background color: Conditional formatting
     - Min (0%): Rød `#E74C3C`
     - Mid (50%): Gul `#F1C40F`
     - Max (100%): Grønn `#27AE60`
   - **Values** → Font color: Hvit (for lesbarhet)
4. Posisjon: Øverst til venstre

---

### Visual 2.2-2.6: Gauge Charts - Fyllingsgrad per Område

**Steg** (gjenta for alle 5 områder):
1. Velg **Gauge** chart
2. Dra felter:
   - **Value**: `[Gjennomsnittlig Fyllingsgrad (%)]`
3. Filter:
   - Legg til **Visual level filter** på `dim_omraade[omraade_kode]`
   - Velg ett område (NO1, NO2, NO3, NO4, eller NO5)
4. Formater:
   - **Gauge axis** → Min: 0, Max: 100
   - **Target**: 70 (målnivå)
   - **Data colors**:
     - < 30%: Rød
     - 30-50%: Oransje
     - 50-80%: Grønn
     - \> 80%: Blå
5. Posisjon: Øverst til høyre (5 gauges i rad)

---

### Visual 2.7: Histogram - Fyllingsgrad Distribusjon

**Steg**:
1. Velg **Clustered column chart**
2. Dra felter:
   - **X-axis**: `fact_magasin[fyllingsgrad_prosent]` (bin i 10% intervaller)
   - **Y-axis**: Antall målinger (COUNT av rader)
3. Formater:
   - **Title**: "Distribusjon av Fyllingsgrad"
   - **X-axis** → Title: "Fyllingsgrad (%)"
   - **Y-axis** → Title: "Antall Observasjoner"
4. Posisjon: Midten, full bredde

---

### Visual 2.8: Card - Område med Høyest Fyllingsgrad

**Steg**:
1. Velg **Card** visual
2. Dra felter:
   - **Fields**: `[Område med Høyest Fyllingsgrad]`
3. Formater:
   - **Category label**: "Best Presterende Område"
   - **Callout value** → Font size: `28pt`
   - **Callout value** → Color: Grønn

---

### Visual 2.9: Card - Område med Lavest Fyllingsgrad

**Steg**:
1. Velg **Card** visual
2. Dra felter:
   - **Fields**: `[Område med Lavest Fyllingsgrad]`
3. Formater:
   - **Category label**: "Laveste Fyllingsgrad"
   - **Callout value** → Color: Oransje

---

### Visual 2.10: Table - Statistikk Tabell

**Steg**:
1. Velg **Table** visual
2. Dra felter:
   - **Columns**: `dim_omraade[omraade_kode]`
   - **Values**:
     - `[Gjennomsnittlig Fyllingsgrad (%)]`
     - `[Standardavvik Fyllingsgrad]`
     - `[Min Fyllingsgrad (%)]`
     - `[Maks Fyllingsgrad (%)]`
     - `[Median Fyllingsgrad (%)]`
3. Formater:
   - **Grid** → Row padding: 8px
   - **Specific column** → Conditional formatting per measure
4. Posisjon: Bunn til høyre

---

## Side 3: Tilsig & Tapping

### Formål
Analyse av vannbalanse med fokus på tilsig, tapping, og netto endring.

### Layout Overview
```
┌─────────────────────────────────────────────────────────────┐
│  TILSIG & TAPPING ANALYSE                                    │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Vannbalanse Waterfall Chart                          │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────┐  ┌──────────────────────────┐    │
│  │ Tilsig Trend         │  │ Tapping Trend            │    │
│  │ (Area Chart)         │  │ (Area Chart)             │    │
│  └──────────────────────┘  └──────────────────────────┘    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Tilsig vs Tapping Scatter Plot                       │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Visual 3.1: Waterfall Chart - Vannbalanse

**Steg**:
1. Velg **Waterfall chart**
2. Dra felter:
   - **Category**: `dim_omraade[omraade_kode]`
   - **Y-axis**:
     - Start: `[Total Magasininnhold (GWh)]` (forrige periode)
     - **+**: `[Total Tilsig (GWh)]`
     - **-**: `[Total Tapping (GWh)]`
     - End: `[Total Magasininnhold (GWh)]` (gjeldende)
3. Formater:
   - **Title**: "Vannbalanse per Område"
   - **Colors**:
     - Increase: Blå
     - Decrease: Rød
     - Total: Grønn
4. Posisjon: Øverst, full bredde

---

### Visual 3.2: Area Chart - Tilsig Trend

**Steg**:
1. Velg **Area chart**
2. Dra felter:
   - **X-axis**: `dim_date[date]`
   - **Y-axis**: `[Total Tilsig (GWh)]`
   - **Legend**: `dim_omraade[omraade_kode]`
3. Formater:
   - **Title**: "Tilsig Trend per Område"
   - **Data colors**: Samme som Executive Dashboard
   - **Y-axis** → Start: 0
4. Posisjon: Venstre side, midten

---

### Visual 3.3: Area Chart - Tapping Trend

**Steg**:
1. Velg **Area chart**
2. Dra felter:
   - **X-axis**: `dim_date[date]`
   - **Y-axis**: `[Total Tapping (GWh)]`
   - **Legend**: `dim_omraade[omraade_kode]`
3. Formater:
   - **Title**: "Tapping Trend per Område"
   - **Data colors**: Samme som tilsig
4. Posisjon: Høyre side, midten

---

### Visual 3.4: Scatter Plot - Tilsig vs Tapping

**Steg**:
1. Velg **Scatter chart**
2. Dra felter:
   - **X-axis**: `[Total Tilsig (GWh)]`
   - **Y-axis**: `[Total Tapping (GWh)]`
   - **Details**: `dim_omraade[omraade_kode]`
   - **Size**: `[Total Magasininnhold (GWh)]`
3. Formater:
   - **Title**: "Tilsig vs Tapping Korrelasjon"
   - **Shapes** → Transparency: 50%
   - Legg til **Constant line** (diagonal) for balanse-linjen (X=Y)
4. Posisjon: Bunn, full bredde

---

### Visual 3.5: Card - Vannbalanse Ratio

**Steg**:
1. Velg **Card** visual
2. Dra felter:
   - **Fields**: `[Vannbalanse Ratio]`
3. Formater:
   - **Category label**: "Vannbalanse Ratio (Tilsig/Tapping)"
   - **Callout value** → Color: Conditional formatting
     - < 1.0: Rød (tømmes)
     - = 1.0: Grønn (balansert)
     - \> 1.0: Blå (fylles)

---

## Side 4: Vær & Nedbør

### Formål
Analyse av værdata og korrelasjon med tilsig.

### Layout Overview
```
┌─────────────────────────────────────────────────────────────┐
│  VÆR & NEDBØR ANALYSE                                        │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Norge Kart med Nedbør per Stasjon (Map Visual)      │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────┐  ┌──────────────────────────┐    │
│  │ Nedbør Trend         │  │ Nedbør vs Tilsig         │    │
│  │ (Line Chart)         │  │ (Combo Chart)            │    │
│  └──────────────────────┘  └──────────────────────────┘    │
│                                                              │
│  ┌─────────┐  ┌─────────┐  ┌───────────────────────────┐  │
│  │ Våteste │  │ Tørreste│  │ Korrelasjon Tabell        │  │
│  └─────────┘  └─────────┘  └───────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Visual 4.1: Map - Nedbør per Stasjon

**Steg**:
1. Velg **Map** visual (Azure Maps)
2. Dra felter:
   - **Location**: `dim_weather_station[station_name]`
   - **Latitude**: `dim_weather_station[latitude]`
   - **Longitude**: `dim_weather_station[longitude]`
   - **Bubble size**: `[Total Nedbør (mm)]`
   - **Bubble color**: `[Gjennomsnittlig Daglig Nedbør (mm)]`
3. Formater:
   - **Title**: "Nedbør per Værstasjon"
   - **Bubble** → Min/Max size: 10-50px
   - **Data colors**: Gradient fra lys blå til mørk blå
4. Posisjon: Øverst, full bredde

---

### Visual 4.2: Line Chart - Nedbør Trend

**Steg**:
1. Velg **Line chart**
2. Dra felter:
   - **X-axis**: `dim_date[date]`
   - **Y-axis**: `[Total Nedbør (mm)]`
   - **Legend**: `dim_weather_station[station_name]`
3. Formater:
   - **Title**: "Nedbør Trend - Alle Stasjoner"
   - **Line** → Stroke width: 1.5px
   - **Legend**: Bottom (10 stasjoner)
4. Posisjon: Venstre side, midten

---

### Visual 4.3: Combo Chart - Nedbør vs Tilsig

**Steg**:
1. Velg **Line and clustered column chart**
2. Dra felter:
   - **X-axis**: `dim_date[date]`
   - **Column Y-axis**: `[Total Nedbør (mm)]`
   - **Line Y-axis**: `[Total Tilsig (GWh)]`
3. Formater:
   - **Title**: "Korrelasjon: Nedbør vs Tilsig"
   - **Column colors**: Blå
   - **Line colors**: Grønn
   - **Secondary Y-axis**: On
4. Posisjon: Høyre side, midten

---

### Visual 4.4: Card - Våteste Stasjon

**Steg**:
1. Velg **Card** visual
2. Dra felter:
   - **Fields**: `[Våteste Stasjon]`
3. Formater:
   - **Category label**: "Mest Nedbør"
   - **Callout value** → Color: Blå

---

### Visual 4.5: Card - Tørreste Stasjon

**Steg**:
1. Velg **Card** visual
2. Dra felter:
   - **Fields**: `[Tørreste Stasjon]`
3. Formater:
   - **Category label**: "Minst Nedbør"
   - **Callout value** → Color: Oransje

---

### Visual 4.6: Table - Korrelasjon Tabell

**Steg**:
1. Velg **Table** visual
2. Dra felter:
   - **Columns**: `dim_omraade[omraade_kode]`
   - **Values**:
     - `[Total Nedbør (mm)]`
     - `[Total Tilsig (GWh)]`
     - `[Korrelasjon Nedbør-Tilsig]`
3. Formater:
   - **Specific column** → Conditional icons for korrelasjon
4. Posisjon: Bunn til høyre

---

## Side 5: Year-over-Year

### Formål
Historiske sammenligninger og trendanalyse.

### Layout Overview
```
┌─────────────────────────────────────────────────────────────┐
│  YEAR-OVER-YEAR ANALYSE                                      │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────────────────┐  │
│  │ YoY Sammenligning - Line Chart                       │  │
│  │ (Nåværende år vs Forrige år)                         │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────┐  ┌──────────────────────────┐    │
│  │ YoY Endring per Måned│  │ YoY % Endring Matrix     │    │
│  │ (Clustered Bar)      │  │ (Matrix)                 │    │
│  └──────────────────────┘  └──────────────────────────┘    │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Sesongindeks per Måned (Clustered Column)           │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Visual 5.1: Line Chart - YoY Sammenligning

**Steg**:
1. Velg **Line chart**
2. Dra felter:
   - **X-axis**: `dim_date[month_name]` eller `dim_date[day_of_year]`
   - **Y-axis**:
     - `[Gjennomsnittlig Fyllingsgrad (%)]` (gjeldende år)
     - `[Fyllingsgrad Forrige År (%)]`
   - **Legend**: År
3. Formater:
   - **Title**: "Fyllingsgrad: 2025 vs 2024"
   - **Line colors**:
     - Gjeldende år: Solid blå
     - Forrige år: Stiplet grå
   - **Line** → Style: Solid vs Dashed
4. Posisjon: Øverst, full bredde

---

### Visual 5.2: Clustered Bar Chart - YoY Endring per Måned

**Steg**:
1. Velg **Clustered bar chart**
2. Dra felter:
   - **Y-axis**: `dim_date[month_name]`
   - **X-axis**: `[YoY Endring - Fyllingsgrad (%)]`
3. Formater:
   - **Title**: "Måned-over-Måned YoY Endring"
   - **Data colors**: Conditional formatting
     - Positiv: Grønn
     - Negativ: Rød
   - **Data labels**: On
4. Posisjon: Venstre side, midten

---

### Visual 5.3: Matrix - YoY % Endring

**Steg**:
1. Velg **Matrix** visual
2. Dra felter:
   - **Rows**: `dim_date[month_name]`
   - **Columns**: `dim_omraade[omraade_kode]`
   - **Values**: `[YoY Prosentvis Endring (%)]`
3. Formater:
   - **Cell elements** → Background color: Conditional formatting
     - Negativ: Rød gradient
     - Positiv: Grønn gradient
   - **Values** → Display as: +0.0%
4. Posisjon: Høyre side, midten

---

### Visual 5.4: Clustered Column Chart - Sesongindeks

**Steg**:
1. Velg **Clustered column chart**
2. Dra felter:
   - **X-axis**: `dim_date[month_name]`
   - **Y-axis**: `[Sesongindeks]`
   - **Legend**: `dim_omraade[omraade_kode]`
3. Formater:
   - **Title**: "Sesongindeks per Måned"
   - **Y-axis** → Add constant line ved 100 (gjennomsnitt)
   - **Data colors**: Distinkte per område
4. Posisjon: Bunn, full bredde

---

## Side 6: Ekstremverdier

### Formål
Alarmer, kritiske verdier, og ekstremverdier-overvåkning.

### Layout Overview
```
┌─────────────────────────────────────────────────────────────┐
│  EKSTREMVERDIER & ALARMER                                    │
├─────────────────────────────────────────────────────────────┤
│  [Alert Count] [Risiko Score] [Kritisk Nivå] [Dager til]   │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Kritiske Områder Tabell (Table)                      │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────┐  ┌──────────────────────────┐    │
│  │ Risiko Score Gauge   │  │ Største Endringer        │    │
│  │ (Gauge)              │  │ (Table)                  │    │
│  └──────────────────────┘  └──────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Visual 6.1: KPI Cards

Opprett 4 KPI cards øverst:
- `[Kritisk Alert Count]`
- `[Risiko Score]` (gjennomsnitt)
- Antall områder under 30%
- `[Dager til Kritisk Nivå]` (minimum)

---

### Visual 6.2: Table - Kritiske Områder

**Steg**:
1. Velg **Table** visual
2. Dra felter:
   - **Columns**:
     - `dim_omraade[omraade_kode]`
     - `[Gjennomsnittlig Fyllingsgrad (%)]`
     - `[Fyllingsgrad Status]`
     - `[Risiko Score]`
     - `[Dager til Kritisk Nivå]`
     - `[Trend Indikator]`
3. Filter:
   - **Visual level filter**: `[Fyllingsgrad Status]` IN ("Kritisk", "Lav")
4. Formater:
   - **Conditional formatting** på alle kolonner
   - **Icon sets** for trend og status
5. Posisjon: Øverst, full bredde

---

### Visual 6.3: Gauge - Risiko Score

**Steg**:
1. Velg **Gauge** chart
2. Dra felter:
   - **Value**: `[Risiko Score]`
3. Formater:
   - **Gauge axis** → Min: 0, Max: 100
   - **Target**: 50 (moderat risiko)
   - **Data colors**:
     - 0-25: Grønn (lav risiko)
     - 25-50: Gul (moderat)
     - 50-75: Oransje (høy)
     - 75-100: Rød (kritisk)
4. Posisjon: Venstre side, midten

---

### Visual 6.4: Table - Største Endringer

**Steg**:
1. Velg **Table** visual
2. Dra felter:
   - **Columns**:
     - `dim_omraade[omraade_kode]`
     - `dim_date[date]`
     - `[Gjennomsnittlig Fyllingsgrad (%)]`
     - Daglig endring (custom measure)
3. Sorter:
   - Etter absoluttverdi av daglig endring (DESC)
   - Top 10
4. Posisjon: Høyre side, midten

---

## Side 7: Månedlig Oppsummering

### Formål
Aggregerte månedlige data for rapportering.

### Layout Overview
```
┌─────────────────────────────────────────────────────────────┐
│  MÅNEDLIG OPPSUMMERING                                       │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────────────────┐  │
│  │ Månedlig Matrix Tabell                               │  │
│  │ (Matrix med alle KPIer)                              │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────┐  ┌──────────────────────────┐    │
│  │ Månedlig Trend       │  │ Månedlig Vannbalanse     │    │
│  │ (Line Chart)         │  │ (Clustered Column)       │    │
│  └──────────────────────┘  └──────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Visual 7.1: Matrix - Månedlig Oppsummering

**Steg**:
1. Velg **Matrix** visual
2. Dra felter:
   - **Rows**: `dim_date[year_month]` (YYYY-MM format)
   - **Columns**: Measures
     - `[Gjennomsnittlig Fyllingsgrad (%)]`
     - `[Total Tilsig (GWh)]`
     - `[Total Tapping (GWh)]`
     - `[Netto Endring (GWh)]`
     - `[Total Nedbør (mm)]`
   - **Sub-rows**: `dim_omraade[omraade_kode]` (expandable)
3. Formater:
   - **Conditional formatting** per measure
   - **Row subtotals**: On
   - **Column subtotals**: On
4. Posisjon: Øverst, full bredde

---

### Visual 7.2: Line Chart - Månedlig Trend

**Steg**:
1. Velg **Line chart**
2. Dra felter:
   - **X-axis**: `dim_date[year_month]`
   - **Y-axis**: `[Gjennomsnittlig Fyllingsgrad (%)]`
   - **Legend**: `dim_omraade[omraade_kode]`
3. Formater:
   - **Title**: "Månedlig Fyllingsgrad Trend"
   - **Markers**: On
   - **Data labels**: On
4. Posisjon: Venstre side, bunn

---

### Visual 7.3: Clustered Column Chart - Månedlig Vannbalanse

**Steg**:
1. Velg **Clustered column chart**
2. Dra felter:
   - **X-axis**: `dim_date[year_month]`
   - **Y-axis**:
     - `[Total Tilsig (GWh)]`
     - `[Total Tapping (GWh)]`
   - **Legend**: Measure
3. Formater:
   - **Title**: "Månedlig Tilsig vs Tapping"
   - **Data labels**: On
   - **Colors**:
     - Tilsig: Blå
     - Tapping: Oransje
4. Posisjon: Høyre side, bunn

---

## Designprinsipper

### Fargepalett

**Strømområder**:
- NO1: `#E74C3C` (Rød)
- NO2: `#3498DB` (Blå)
- NO3: `#2ECC71` (Grønn)
- NO4: `#F39C12` (Oransje)
- NO5: `#9B59B6` (Lilla)

**Status/Alerts**:
- Kritisk: `#E74C3C` (Rød)
- Lav: `#F39C12` (Oransje)
- Normal: `#27AE60` (Grønn)
- Høy: `#3498DB` (Blå)

**Backgrounds**:
- Canvas: `#FFFFFF` (Hvit)
- Visual background: `#F8F9FA` (Lysgrå)
- Header: `#2C3E50` (Mørkeblå)

### Typography

- **Title font**: Segoe UI, 14pt, Bold
- **Label font**: Segoe UI, 10pt, Regular
- **Data labels**: Segoe UI, 11pt, Regular
- **Card values**: Segoe UI, 36-40pt, Bold

### Layout

- **Margins**: 10px mellom visuals
- **Padding**: 15px canvas padding
- **Alignment**: Grid-basert, snap to grid
- **Visual sizes**: Konsistente størrelser per visual type

---

## Interaktivitet & Filters

### Global Filters (på alle sider)

1. **Dato Slicer**:
   - Type: Between
   - Default: Last 90 days
   - Sync across all pages

2. **Område Slicer**:
   - Type: List (checkboxes)
   - Default: All selected
   - Sync across all pages

### Drillthrough

Opprett drillthrough fra Executive Dashboard til Magasin Detaljanalyse:

1. Gå til **Magasin Detaljanalyse** side
2. I **Drillthrough** well, dra `dim_omraade[omraade_kode]`
3. Test ved å høyreklikke på et område i Executive Dashboard → Drillthrough

### Tooltips

Opprett custom tooltip page:

1. Lag ny side kalt "Tooltip - Område"
2. Sett **Page size** til Tooltip
3. Legg til:
   - Område navn
   - Gjeldende fyllingsgrad
   - 7-dagers trend
   - YoY endring
4. Aktiver som tooltip på relevante visuals

### Cross-filtering

Standardinnstilling: Cross-highlight
Unntak:
- KPI cards: No interaction
- Slicers: Filter all visuals

---

## Ytelsesoptimalisering

### Best Practices

1. **Begrens antall visuals per side**: Maks 10 visuals
2. **Bruk aggregerte tabeller**: `fact_magasin_monthly` for månedlige views
3. **Deaktiver interactions** der ikke nødvendig
4. **Reduser data labels** på detaljerte visuals
5. **Test med full datasett** før deployment

### Performance Analyzer

1. Gå til **View** ribbon → **Performance Analyzer**
2. Klikk **Start recording**
3. Interager med rapporten
4. Klikk **Stop**
5. Analyser hvilke visuals som er tregest
6. Optimaliser DAX measures eller visual settings

### Refresh Strategy

- Bruk **Scheduled refresh** i Power BI Service (1x daglig kl 06:00)
- Alternativt: Manuell refresh etter ETL-kjøring

---

## Sjekkliste - Ferdigstillelse

- [ ] Alle 7 sider opprettet
- [ ] Alle visuals formatert konsistent
- [ ] Fargepalett anvendt korrekt
- [ ] Interaktivitet testet (cross-filtering, drillthrough)
- [ ] Global filters konfigurert og synkronisert
- [ ] Custom tooltips opprettet
- [ ] Performance testet (< 2 sek refresh)
- [ ] Tested på forskjellige skjermstørrelser
- [ ] UAT utført med sluttbrukere
- [ ] Dokumentasjon komplett

---

**Neste steg**: Se `USER_GUIDE.md` for brukerveiledning

**Dokumentasjon versjon**: 1.0
**Sist oppdatert**: 2025-11-19
