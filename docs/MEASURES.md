# DAX Measures - Dokumentasjon

**Prosjekt**: DyrWatt AS - Magasin og Vær Analytics Dashboard
**Database**: DyrWatt_Analytics (SQL Server localhost)
**Totalt antall measures**: 50+
**Sist oppdatert**: 2025-11-19

---

## Innholdsfortegnelse

1. [Oversikt](#oversikt)
2. [Display Folder Struktur](#display-folder-struktur)
3. [Kategori 1: Magasin - Basis](#kategori-1-magasin---basis)
4. [Kategori 2: Magasin - Time Intelligence](#kategori-2-magasin---time-intelligence)
5. [Kategori 3: Magasin - KPIs & Alerts](#kategori-3-magasin---kpis--alerts)
6. [Kategori 4: Vær & Nedbør](#kategori-4-vær--nedbør)
7. [Kategori 5: Avansert Analyse](#kategori-5-avansert-analyse)
8. [Implementeringsveiledning](#implementeringsveiledning)
9. [Best Practices](#best-practices)
10. [Feilsøking](#feilsøking)

---

## Oversikt

Denne dokumentasjonen beskriver alle DAX measures for DyrWatt Analytics Power BI-rapport. Measures er organisert i 5 hovedkategorier med tilhørende Display Folders for enkel navigering i Power BI Desktop.

### Antall Measures per Kategori

| Kategori | Antall | Fil |
|----------|--------|-----|
| **1. Magasin - Basis** | 10 | `measures/magasin_basis.dax` |
| **2. Time Intelligence** | 12 | `measures/magasin_time_intelligence.dax` |
| **3. KPIs & Alerts** | 13 | `measures/magasin_kpi.dax` |
| **4. Vær & Nedbør** | 11 | `measures/vaer_measures.dax` |
| **5. Avansert Analyse** | 14 | `measures/advanced_calculations.dax` |
| **TOTALT** | **60** | |

---

## Display Folder Struktur

Measures skal organiseres i følgende Display Folders i Power BI:

```
Power BI Model
│
├── 1. Magasin - Basis
│   ├── Total Magasininnhold (GWh)
│   ├── Gjennomsnittlig Fyllingsgrad (%)
│   ├── Total Tilsig (GWh)
│   ├── Total Tapping (GWh)
│   ├── Netto Endring (GWh)
│   ├── Kapasitetsutnyttelse (%)
│   ├── Maks Fyllingsgrad (%)
│   ├── Min Fyllingsgrad (%)
│   ├── Total Kapasitet (GWh)
│   └── Antall Målinger
│
├── 2. Magasin - Time Intelligence
│   ├── YoY Sammenligninger/
│   ├── MoM Sammenligninger/
│   ├── Running Totals/
│   └── Glidende Gjennomsnitt/
│
├── 3. Magasin - KPIs & Alerts
│   ├── Status & Alerts/
│   ├── Rankings & Sammenligninger/
│   └── Avanserte KPIs/
│
├── 4. Vær & Nedbør
│   ├── Basis Nedbør/
│   ├── Tidsbasert Nedbør/
│   ├── Regional Analyse/
│   └── Korrelasjon/
│
└── 5. Avansert Analyse
    ├── Statistiske Mål/
    ├── Vektede Beregninger/
    ├── Performance Scores/
    └── Strategiske Beregninger/
```

---

## Kategori 1: Magasin - Basis

### 1.1 Total Magasininnhold (GWh)

**Formål**: Summerer alt magasininnhold for valgt periode/område

**DAX Kode**:
```dax
Total Magasininnhold (GWh) =
VAR _Sum = SUM('fact_magasin'[magasininnhold_GWh])
RETURN
    IF(ISBLANK(_Sum), BLANK(), _Sum)
```

**Bruksområder**:
- KPI cards på dashbord
- Trendlinjer over tid
- Områdesammenligning i matrix

**Format**: `#,##0.0 "GWh"`

---

### 1.2 Gjennomsnittlig Fyllingsgrad (%)

**Formål**: Beregner gjennomsnittlig fyllingsgrad over valgt periode

**DAX Kode**:
```dax
Gjennomsnittlig Fyllingsgrad (%) =
VAR _Avg = AVERAGE('fact_magasin'[fyllingsgrad_prosent])
RETURN
    IF(ISBLANK(_Avg), BLANK(), _Avg)
```

**Bruksområder**:
- Hovedindikator på alle dashbord-sider
- Gauge charts (mål: 70%)
- Områdesammenligning

**Format**: `0.0 "%"`

**Benchmark**: Målnivå 70%, kritisk < 30%

---

### 1.3 Total Tilsig (GWh)

**Formål**: Summerer alt tilsig (vanninngang) for valgt periode

**DAX Kode**:
```dax
Total Tilsig (GWh) =
VAR _Tilsig = SUM('fact_magasin'[tilsig_GWh])
RETURN
    IF(ISBLANK(_Tilsig), BLANK(), _Tilsig)
```

**Bruksområder**:
- Vannbalanse-analyse
- Korrelasjon med nedbør
- Sesonganalyse

**Format**: `#,##0 "GWh"`

---

### 1.4 Total Tapping (GWh)

**Formål**: Summerer all tapping (vannuttaking/produksjon) for valgt periode

**DAX Kode**:
```dax
Total Tapping (GWh) =
VAR _Tapping = SUM('fact_magasin'[tapping_GWh])
RETURN
    IF(ISBLANK(_Tapping), BLANK(), _Tapping)
```

**Bruksområder**:
- Produksjonsanalyse
- Vannbalanse beregning
- Kapasitetsutnyttelse

**Format**: `#,##0 "GWh"`

---

### 1.5 Netto Endring (GWh)

**Formål**: Beregner netto endring (Tilsig - Tapping)

**DAX Kode**:
```dax
Netto Endring (GWh) =
VAR _Tilsig = [Total Tilsig (GWh)]
VAR _Tapping = [Total Tapping (GWh)]
VAR _NettoEndring = _Tilsig - _Tapping
RETURN
    IF(ISBLANK(_Tilsig) || ISBLANK(_Tapping), BLANK(), _NettoEndring)
```

**Bruksområder**:
- Vannbalanse-dashboard
- Trend-analyse (fylles/tømmes)
- Forecasting

**Format**: `+#,##0;-#,##0;0 "GWh"` (viser +/- tegn)

**Tolkning**:
- Positiv verdi: Magasinet fylles opp
- Negativ verdi: Magasinet tømmes
- Null: Stabil

---

### 1.6 Kapasitetsutnyttelse (%)

**Formål**: Beregner hvor mye av totalkapasiteten som er i bruk

**DAX Kode**:
```dax
Kapasitetsutnyttelse (%) =
VAR _Innhold = SUM('fact_magasin'[magasininnhold_GWh])
VAR _Kapasitet = SUM('fact_magasin'[magasinkapasitet_GWh])
VAR _Utnyttelse = DIVIDE(_Innhold, _Kapasitet, 0) * 100
RETURN
    IF(ISBLANK(_Innhold) || ISBLANK(_Kapasitet), BLANK(), _Utnyttelse)
```

**Bruksområder**:
- Kapasitetsplanlegging
- Effektivitetsmåling
- Regional sammenligning

**Format**: `0.0 "%"`

---

### 1.7-1.10 Andre Basis Measures

- **Maks Fyllingsgrad (%)**: Høyeste verdi i perioden
- **Min Fyllingsgrad (%)**: Laveste verdi i perioden
- **Total Kapasitet (GWh)**: Sum av all magasinkapasitet
- **Antall Målinger**: Teller antall datapunkter (datakvalitet)

---

## Kategori 2: Magasin - Time Intelligence

### 2.1 Year-over-Year (YoY) Measures

#### Magasininnhold Forrige År (GWh)

**Formål**: Henter data for samme periode i fjor

**DAX Kode**:
```dax
Magasininnhold Forrige År (GWh) =
CALCULATE(
    [Total Magasininnhold (GWh)],
    SAMEPERIODLASTYEAR('dim_date'[date])
)
```

**Krav**: Må ha en korrekt Date-dimensjonstabell

---

#### YoY Endring - Magasin (GWh)

**Formål**: Absoluttverdier endring fra i fjor

**DAX Kode**:
```dax
YoY Endring - Magasin (GWh) =
VAR _CurrentYear = [Total Magasininnhold (GWh)]
VAR _LastYear = [Magasininnhold Forrige År (GWh)]
VAR _Change = _CurrentYear - _LastYear
RETURN
    IF(ISBLANK(_CurrentYear) || ISBLANK(_LastYear), BLANK(), _Change)
```

**Bruksområder**:
- Historisk sammenligning
- Waterfall charts
- Year-over-Year trend-analyse

**Format**: `+#,##0;-#,##0;0 "GWh"`

---

#### YoY Prosentvis Endring (%)

**Formål**: Prosentvis vekst fra i fjor til i år

**DAX Kode**:
```dax
YoY Prosentvis Endring (%) =
VAR _CurrentYear = [Total Magasininnhold (GWh)]
VAR _LastYear = [Magasininnhold Forrige År (GWh)]
VAR _PercentChange = DIVIDE(_CurrentYear - _LastYear, _LastYear, 0) * 100
RETURN
    IF(ISBLANK(_CurrentYear) || ISBLANK(_LastYear) || _LastYear = 0, BLANK(), _PercentChange)
```

**Format**: `+0.0%;-0.0%;0%`

---

### 2.2 Month-over-Month (MoM) Measures

#### MoM Endring - Magasin (GWh)

**Formål**: Måned-over-måned endring

**DAX Kode**:
```dax
MoM Endring - Magasin (GWh) =
VAR _CurrentMonth = [Total Magasininnhold (GWh)]
VAR _LastMonth = [Magasininnhold Forrige Måned (GWh)]
VAR _Change = _CurrentMonth - _LastMonth
RETURN
    IF(ISBLANK(_CurrentMonth) || ISBLANK(_LastMonth), BLANK(), _Change)
```

**Bruksområder**:
- Kortsiktig trendanalyse
- Månedlige rapporter
- Executive summary

---

### 2.3 Running Totals (YTD)

#### Running Total - Tilsig YTD (GWh)

**Formål**: Akkumulert tilsig hittil i år

**DAX Kode**:
```dax
Running Total - Tilsig YTD (GWh) =
CALCULATE(
    [Total Tilsig (GWh)],
    DATESYTD('dim_date'[date])
)
```

**Bruksområder**:
- Kumulativ trendanalyse
- YTD sammenligning mot budsjett
- Area charts for akkumulerte verdier

---

### 2.4 Glidende Gjennomsnitt

#### MA7 Fyllingsgrad (%) - 7-dagers glidende gjennomsnitt

**Formål**: Smooth ut daglige variasjoner

**DAX Kode**:
```dax
MA7 Fyllingsgrad (%) =
VAR _CurrentDate = MAX('dim_date'[date])
VAR _7DaysAgo = _CurrentDate - 7
VAR _Avg7Days =
    CALCULATE(
        [Gjennomsnittlig Fyllingsgrad (%)],
        DATESBETWEEN('dim_date'[date], _7DaysAgo, _CurrentDate)
    )
RETURN
    IF(ISBLANK(_Avg7Days), BLANK(), _Avg7Days)
```

**Bruksområder**:
- Trend-identifikasjon
- Smooth line i linjegrafer
- Signal/noise separasjon

---

#### MA30 Fyllingsgrad (%) - 30-dagers glidende gjennomsnitt

**Formål**: Langsiktig trendanalyse

**Bruksområder**:
- Sesongvariasjoner
- Langsiktig forecast
- Strategic planning

---

## Kategori 3: Magasin - KPIs & Alerts

### 3.1 Fyllingsgrad Status

**Formål**: Kategoriserer fyllingsgrad i 4 nivåer

**DAX Kode**:
```dax
Fyllingsgrad Status =
VAR _Filling = [Gjennomsnittlig Fyllingsgrad (%)]
VAR _Status =
    SWITCH(
        TRUE(),
        _Filling < 30, "Kritisk",
        _Filling < 50, "Lav",
        _Filling < 80, "Normal",
        "Høy"
    )
RETURN
    IF(ISBLANK(_Filling), BLANK(), _Status)
```

**Nivåer**:
- **Kritisk**: < 30% (Rød alarm)
- **Lav**: 30-50% (Oransje advarsel)
- **Normal**: 50-80% (Grønn OK)
- **Høy**: > 80% (Blå info)

**Bruksområder**:
- Conditional formatting
- Alert-systemer
- Executive dashboards

---

### 3.2 Fyllingsgrad Status Farge

**Formål**: Returnerer hex-fargekode for conditional formatting

**DAX Kode**:
```dax
Fyllingsgrad Status Farge =
VAR _Status = [Fyllingsgrad Status]
VAR _Farge =
    SWITCH(
        _Status,
        "Kritisk", "#E74C3C",  // Rød
        "Lav", "#F39C12",       // Oransje
        "Normal", "#27AE60",    // Grønn
        "Høy", "#3498DB",       // Blå
        "#95A5A6"               // Grå (default)
    )
RETURN
    _Farge
```

**Fargekoder**:
- Kritisk: `#E74C3C` (Rød)
- Lav: `#F39C12` (Oransje)
- Normal: `#27AE60` (Grønn)
- Høy: `#3498DB` (Blå)

**Bruk**: Conditional formatting → Field value → [Fyllingsgrad Status Farge]

---

### 3.3 Kritisk Alert Count

**Formål**: Teller antall områder med kritisk fyllingsgrad

**DAX Kode**:
```dax
Kritisk Alert Count =
VAR _CriticalAreas =
    CALCULATE(
        DISTINCTCOUNT('dim_omraade'[omraade_key]),
        FILTER(
            ALL('dim_omraade'),
            CALCULATE([Gjennomsnittlig Fyllingsgrad (%)]) < 30
        )
    )
RETURN
    IF(ISBLANK(_CriticalAreas) || _CriticalAreas = 0, 0, _CriticalAreas)
```

**Bruksområder**:
- Alert dashboard
- Executive summary card
- Email alerts (via Power BI alerting)

---

### 3.4 Område med Høyest/Lavest Fyllingsgrad

**Formål**: Identifiserer best/worst performers

**DAX Kode (Høyest)**:
```dax
Område med Høyest Fyllingsgrad =
VAR _TopArea =
    TOPN(
        1,
        SUMMARIZE('dim_omraade', 'dim_omraade'[omraade_kode], "AvgFilling", [Gjennomsnittlig Fyllingsgrad (%)]),
        [AvgFilling],
        DESC
    )
VAR _AreaName = MAXX(_TopArea, 'dim_omraade'[omraade_kode])
RETURN
    IF(ISBLANK(_AreaName), "Ingen data", _AreaName)
```

**Bruksområder**:
- Top performers card
- Executive summary
- Competition analysis

---

### 3.5 Avvik fra Gjennomsnitt (%)

**Formål**: Beregner avvik fra nasjonal gjennomsnittlig fyllingsgrad

**DAX Kode**:
```dax
Avvik fra Gjennomsnitt (%) =
VAR _CurrentFilling = [Gjennomsnittlig Fyllingsgrad (%)]
VAR _NationalAvg =
    CALCULATE(
        [Gjennomsnittlig Fyllingsgrad (%)],
        ALL('dim_omraade')
    )
VAR _Deviation = _CurrentFilling - _NationalAvg
RETURN
    IF(ISBLANK(_CurrentFilling) || ISBLANK(_NationalAvg), BLANK(), _Deviation)
```

**Tolkning**:
- Positiv: Over landssnittet
- Negativ: Under landssnittet

**Bruksområder**:
- Benchmarking
- Regional sammenligning
- Performance gaps

---

### 3.6 Andre KPI Measures

- **Dager til Kritisk Nivå**: Estimerer tid til 30% basert på trend
- **Kapasitet Ubrukt (GWh)**: Ledig kapasitet
- **Vannbalanse Ratio**: Tilsig/Tapping ratio
- **Trend Indikator**: ↑ Økende | ↓ Synkende | → Stabil
- **Område Ranking**: Rangerer områder
- **Måloppnåelse (%)**: Oppnåelse mot 70% mål
- **Risiko Score**: Kombinert risikovurdering (0-100)

---

## Kategori 4: Vær & Nedbør

### 4.1 Total Nedbør (mm)

**Formål**: Summerer all nedbør for valgt periode

**DAX Kode**:
```dax
Total Nedbør (mm) =
VAR _Precip = SUM('fact_weather'[precipitation_mm])
RETURN
    IF(ISBLANK(_Precip), BLANK(), _Precip)
```

**Bruksområder**:
- Væranalyse
- Korrelasjon med tilsig
- Regional sammenligning

**Format**: `#,##0.0 "mm"`

---

### 4.2 Gjennomsnittlig Daglig Nedbør (mm)

**Formål**: Normalisert nedbørsmåling per dag

**DAX Kode**:
```dax
Gjennomsnittlig Daglig Nedbør (mm) =
VAR _AvgPrecip = AVERAGE('fact_weather'[precipitation_mm])
RETURN
    IF(ISBLANK(_AvgPrecip), BLANK(), _AvgPrecip)
```

---

### 4.3 Værmønster Measures

- **Maks Døgnnedbør (mm)**: Høyeste registrerte døgnnedbør
- **Antall Regnværsdager**: Dager med > 1mm nedbør
- **Nedbør Siste 7 Dager (mm)**: Rullerende 7-dagers sum
- **Nedbør YoY Endring (mm)**: Sammenligning med i fjor

---

### 4.4 Regional Væ ranalyse

- **Tørreste Stasjon**: Stasjon med minst nedbør
- **Våteste Stasjon**: Stasjon med mest nedbør
- **Nedbør Trend Indikator**: ↑ Øker | ↓ Synker | → Stabil

---

### 4.5 Korrelasjon Nedbør-Tilsig

**Formål**: Enkel indikator for sammenheng

**DAX Kode**:
```dax
Korrelasjon Nedbør-Tilsig =
VAR _Precip = [Total Nedbør (mm)]
VAR _Tilsig = [Total Tilsig (GWh)]
VAR _AvgPrecip = CALCULATE([Total Nedbør (mm)], ALL('dim_date'))
VAR _AvgTilsig = CALCULATE([Total Tilsig (GWh)], ALL('dim_date'))
VAR _Correlation =
    SWITCH(
        TRUE(),
        _Precip > _AvgPrecip && _Tilsig > _AvgTilsig, "Positiv",
        _Precip < _AvgPrecip && _Tilsig < _AvgTilsig, "Positiv",
        "Negativ"
    )
RETURN
    IF(ISBLANK(_Precip) || ISBLANK(_Tilsig), BLANK(), _Correlation)
```

**Note**: Dette er en forenklet indikator. For statistisk korrelasjon, bruk Python/R visuals.

---

## Kategori 5: Avansert Analyse

### 5.1 Statistiske Mål

#### Standardavvik Fyllingsgrad

**Formål**: Måler variasjon/volatilitet

**DAX Kode**:
```dax
Standardavvik Fyllingsgrad =
VAR _StdDev = STDEV.P('fact_magasin'[fyllingsgrad_prosent])
RETURN
    IF(ISBLANK(_StdDev), BLANK(), _StdDev)
```

**Tolkning**:
- Høy verdi: Stor variasjon, ustabil
- Lav verdi: Liten variasjon, stabil

---

#### Percentiler

**DAX Kode (Eksempel Q1)**:
```dax
Percentil 25 Fyllingsgrad (%) =
PERCENTILEX.INC('fact_magasin', 'fact_magasin'[fyllingsgrad_prosent], 0.25)
```

**Percentiler**:
- **Q1 (25%)**: 25% av observasjonene er lavere
- **Median (50%)**: Midtpunkt i distribusjonen
- **Q3 (75%)**: 75% av observasjonene er lavere

**Bruksområder**:
- Distribusjonskortikk
- Box plots
- Outlier detection

---

#### Interkvartil Range (IQR)

**Formål**: Måler spredning i midtre 50% av data

**DAX Kode**:
```dax
Interkvartil Range (%) =
VAR _Q3 = [Percentil 75 Fyllingsgrad (%)]
VAR _Q1 = [Percentil 25 Fyllingsgrad (%)]
VAR _IQR = _Q3 - _Q1
RETURN
    IF(ISBLANK(_Q3) || ISBLANK(_Q1), BLANK(), _IQR)
```

**Outlier detection**: Verdier utenfor [Q1 - 1.5×IQR, Q3 + 1.5×IQR]

---

### 5.2 Vektede Beregninger

#### Vektet Gjennomsnitt Fyllingsgrad (%)

**Formål**: Fyllingsgrad vektet etter kapasitet

**DAX Kode**:
```dax
Vektet Gjennomsnitt Fyllingsgrad (%) =
SUMX(
    'fact_magasin',
    'fact_magasin'[fyllingsgrad_prosent] * 'fact_magasin'[magasinkapasitet_GWh]
) / SUM('fact_magasin'[magasinkapasitet_GWh])
```

**Fordel**: Store magasiner får større innflytelse, mer representativt nasjonalt tall

---

#### Område Markedsandel (%)

**Formål**: Områdets andel av total nasjonal kapasitet

**DAX Kode**:
```dax
Område Markedsandel (%) =
VAR _AreaCapacity = [Total Kapasitet (GWh)]
VAR _NationalCapacity = CALCULATE([Total Kapasitet (GWh)], ALL('dim_omraade'))
VAR _Share = DIVIDE(_AreaCapacity, _NationalCapacity, 0) * 100
RETURN
    IF(ISBLANK(_AreaCapacity) || ISBLANK(_NationalCapacity), BLANK(), _Share)
```

---

### 5.3 Performance Scores

#### Effektivitet Index

**Formål**: Kombinert metric for optimal magasinforvaltning

**Formel**: (Fylling × 0.5) + (100-CV × 0.3) + (Utnyttelse × 0.2)

**DAX Kode**:
```dax
Effektivitet Index =
VAR _Filling = [Gjennomsnittlig Fyllingsgrad (%)]
VAR _CV = [Varianskoeffisient (%)]
VAR _Utilization = [Kapasitetsutnyttelse (%)]
VAR _FillingScore = _Filling * 0.5
VAR _StabilityScore = (100 - MIN(100, _CV)) * 0.3
VAR _UtilizationScore = _Utilization * 0.2
VAR _TotalScore = _FillingScore + _StabilityScore + _UtilizationScore
RETURN
    IF(ISBLANK(_Filling) || ISBLANK(_CV) || ISBLANK(_Utilization), BLANK(), _TotalScore)
```

**Score**: 0-100, høyere = bedre

**Komponenter**:
- 50% Fyllingsgrad
- 30% Stabilitet (lav volatilitet)
- 20% Utnyttelse

---

### 5.4 Strategiske Beregninger

#### Kraftpotensiell Tapt (GWh)

**Formål**: Estimerer tapt produksjonspotensial

**DAX Kode**:
```dax
Kraftpotensiell Tapt (GWh) =
VAR _TargetFilling = 70
VAR _CurrentFilling = [Gjennomsnittlig Fyllingsgrad (%)]
VAR _Capacity = [Total Kapasitet (GWh)]
VAR _CurrentContent = (_CurrentFilling / 100) * _Capacity
VAR _TargetContent = (_TargetFilling / 100) * _Capacity
VAR _PotentialLost = IF(_TargetContent > _CurrentContent, _TargetContent - _CurrentContent, 0)
RETURN
    IF(ISBLANK(_CurrentFilling) || ISBLANK(_Capacity), BLANK(), _PotentialLost)
```

**Bruksområder**:
- Strategic planning
- Opportunity cost analysis
- Budgeting

---

#### Total Fleksibilitet (GWh)

**Formål**: Maksimal mengde vann som kan tappes før kritisk nivå

**DAX Kode**:
```dax
Total Fleksibilitet (GWh) =
VAR _Content = [Total Magasininnhold (GWh)]
VAR _Capacity = [Total Kapasitet (GWh)]
VAR _CriticalLevel = (_Capacity * 0.30)
VAR _Flexibility = _Content - _CriticalLevel
RETURN
    IF(ISBLANK(_Content) || ISBLANK(_Capacity), BLANK(), MAX(0, _Flexibility))
```

**Bruksområder**:
- Operasjonell fleksibilitet
- Kraftmarked-strategier
- Risk management

---

## Implementeringsveiledning

### Steg 1: Åpne Power BI Desktop

1. Åpne `C:\Users\chris\Desktop\fraktalpowerbi\theme.pbip`
2. Gå til **Model View** (venstre sidebar)

### Steg 2: Velg Tabell for Measures

Velg `fact_magasin` tabellen i Model View (measures skal ligge her)

### Steg 3: Opprett New Measure

1. Klikk på **Modeling** ribbon
2. Velg **New Measure**
3. Kopier DAX-kode fra `.dax` filene
4. Klikk Enter for å lagre

### Steg 4: Sett Display Folder

1. Høyreklikk på measure i Fields pane
2. Velg **Properties**
3. Sett **Display Folder** (f.eks. "1. Magasin - Basis")
4. Klikk OK

### Steg 5: Formater Measure

1. Velg measure i Fields pane
2. Gå til **Measure tools** ribbon
3. Sett **Format** (f.eks. `#,##0.0 "GWh"`)
4. Sett **Data category** hvis relevant (Geography, Web URL, etc.)

### Steg 6: Test Measure

1. Gå til **Report View**
2. Lag en **Card** visual
3. Dra measure til **Fields**
4. Legg til slicer (f.eks. område, dato) for å teste filtering

### Steg 7: Gjenta for Alle Measures

Gjenta steg 2-6 for alle 60 measures.

---

## Best Practices

### 1. Naming Conventions

- Bruk beskrivende norske navn
- Inkluder enhet i parenteser: `(GWh)`, `(%)`, `(mm)`
- Unngå forkortelser (bruk "Gjennomsnittlig" ikke "Avg")
- Konsistent bruk av stor forbokstav

### 2. DAX Koding

- Bruk `VAR` for alle mellomberegninger (bedre lesbarhet)
- Alltid håndter `BLANK()` verdier
- Bruk `DIVIDE()` i stedet for `/` (unngår divisjon med null)
- Kommenter komplekse measures

### 3. Display Folders

- Maks 3 nivåer dypt
- Grupper relaterte measures
- Bruk nummerering for sortering (1., 2., 3.)
- Lag subfoldere for store kategorier

### 4. Formatering

- Konsistent desimal-separering (bruk `.` ikke `,`)
- Bruk tusenskilletegn: `#,##0`
- Spesifiser enheter: `"GWh"`, `"mm"`, `"%"`
- Bruk +/- indikatorer for endringer: `+#,##0;-#,##0;0`

### 5. Ytelse

- Bruk `SUM()`, `AVERAGE()` etc. i stedet for `SUMX()` når mulig
- Unngå nested `CALCULATE()` (flatten hvis mulig)
- Test measures med store datasett
- Bruk DAX Studio for performance analysis

### 6. Testing

- Test med forskjellige filter-kontekster
- Verifiser mot SQL-queries
- Sjekk edge cases (null, zero, negative)
- Test med slicers på alle dimensjoner

---

## Feilsøking

### Problem: "A circular dependency was detected"

**Årsak**: Measure refererer til seg selv (direkte eller indirekte)

**Løsning**: Bryt cirkulær referanse ved å bruke base columns

---

### Problem: "The measure uses a function that is not supported"

**Årsak**: Bruker Python/R funksjoner i DAX

**Løsning**: Bruk DAX-ekvivalenter (AVERAGE i stedet for MEAN etc.)

---

### Problem: "Cannot find table 'fact_magasin'"

**Årsak**: Tabellen er ikke importert eller har annet navn

**Løsning**:
1. Gå til **Transform Data**
2. Sjekk at tabellen er importert
3. Verifiser skrivemåte (case-sensitive i noen tilfeller)

---

### Problem: Measure returnerer feil verdi

**Debugging steg**:
1. Bryt ned measure med `VAR` statements
2. Test hver variable separat i en ny measure
3. Bruk DAX Studio for å se evaluation context
4. Sjekk filter context med `FILTERS()` funksjon

---

### Problem: Measure er treg

**Optimalisering**:
1. Bruk `SUM()` i stedet for `SUMX()` når mulig
2. Flytt `CALCULATE()` ut av iterator-funksjoner
3. Bruk aggregerte tabeller (`fact_magasin_monthly`)
4. Indexer kolonner i SQL Server
5. Reduser antall rader i model (filter i Power Query)

---

## Kategori 6: Ekstremverdier & Største Endringer (NYE MEASURES)

### 6.1 Fyllingsgrad - Ekstremverdier

#### Største Daglig Endring - Fyllingsgrad (pp)
**Formål**: Identifiserer den største endringen i fyllingsgrad fra én dag til neste

**Bruksområder**:
- Ekstremverdi-dashboard
- Identifisere perioder med kraftige endringer
- Alert-systemer

**Format**: `0.0 "pp"`

---

#### Dato for Største Endring - Fyllingsgrad
**Formål**: Returnerer datoen med største endring i fyllingsgrad

**Bruksområder**:
- Historisk analyse
- Event-identifikasjon
- Root cause analysis

**Format**: `Short Date`

---

#### Område med Største Volatilitet
**Formål**: Identifiserer området med størst variasjon i fyllingsgrad (basert på standardavvik)

**Bruksområder**:
- Risikovurdering
- Stabilitetsanalyse
- Operasjonell planlegging

---

#### Volatilitets-Score
**Formål**: Måler variasjonen i fyllingsgrad - høyere score = mer ustabil

**Tolkning**:
- 0-5: Svært stabil
- 5-10: Moderat variasjon
- 10-15: Høy variasjon
- >15: Ekstrem volatilitet

**Format**: `0.0`

---

#### Største Ukeendring - Magasin (GWh)
**Formål**: Største endring i magasininnhold over en 7-dagers periode

**Bruksområder**:
- Ukeanalyse
- Trend-identifikasjon
- Kapasitetsplanlegging

**Format**: `#,##0 "GWh"`

---

#### Antall Kritiske Perioder
**Formål**: Teller antall dager med fyllingsgrad under 30%

**Bruksområder**:
- Risikovurdering
- Historisk analyse
- KPI for driftssikkerhet

**Format**: `#,##0 "dager"`

---

#### Gjennomsnittlig Daglig Endring (pp)
**Formål**: Gjennomsnittlig daglig endring i fyllingsgrad

**Tolkning**:
- Positiv: Generell oppfyllingstrend
- Negativ: Generell tømmingstrend
- Nær null: Stabil

**Format**: `+0.00;-0.00;0.00 "pp"`

---

### 6.2 Nedbør - Ekstremverdier

#### Største Daglig Nedbør - Alle Stasjoner (mm)
**Formål**: Høyeste registrerte døgnnedbør på tvers av alle stasjoner

**Bruksområder**:
- Ekstremværanalyse
- Flomrisiko-vurdering
- Historiske rekorder

**Format**: `0.0 "mm"`

---

#### Dato for Største Nedbør
**Formål**: Returnerer datoen med høyest registrert nedbør

**Bruksområder**:
- Event-analyse
- Korrelasjon med tilsig
- Historisk dokumentasjon

**Format**: `Short Date`

---

#### Stasjon med Største Nedbør
**Formål**: Identifiserer værstasjon med høyest registrert døgnnedbør

**Bruksområder**:
- Regional analyse
- Værmønster-identifikasjon
- Stasjonsammenligning

---

#### Største Ukenedbør (mm)
**Formål**: Høyeste nedbørsum over en 7-dagers periode

**Bruksområder**:
- Ukeanalyse
- Flomrisiko
- Sesongvurdering

**Format**: `#,##0.0 "mm"`

---

#### Antall Kraftige Nedbørsdager (>20mm)
**Formål**: Teller dager med kraftig nedbør (>20mm regnes som kraftig nedbør)

**Definisjon**:
- >20mm: Kraftig nedbør
- >50mm: Svært kraftig nedbør

**Bruksområder**:
- Klimastatistikk
- Ekstremværfrekvens
- Trendanalyse

**Format**: `#,##0 "dager"`

---

#### Antall Tørre Dager (<1mm)
**Formål**: Teller dager med minimal eller ingen nedbør

**Bruksområder**:
- Tørkeanalyse
- Sesongvariasjoner
- Klimatrendspotting

**Format**: `#,##0 "dager"`

---

#### Nedbør Variasjon (Standardavvik)
**Formål**: Måler variasjon i nedbørsmønstre - høyere = mer uforutsigbart

**Tolkning**:
- Lav: Stabile nedbørsmønstre
- Høy: Uforutsigbare værmønstre

**Format**: `0.0 "mm"`

---

#### Gjennomsnittlig Ukeendring - Nedbør (mm)
**Formål**: Gjennomsnittlig endring i ukentlig nedbørsum

**Bruksområder**:
- Variabilitetsanalyse
- Sesongmønster
- Forecasting

**Format**: `0.0 "mm"`

---

## OPPDATERT: Antall Measures per Kategori

| Kategori | Antall | Beskrivelse |
|----------|--------|-------------|
| **1. Magasin - Basis** | 10 | Grunnleggende magasinmålinger |
| **2. Time Intelligence** | 12 | YoY, MoM, YTD, glidende gjennomsnitt |
| **3. KPIs & Alerts** | 13 | Status, alerts, rankings |
| **4. Vær & Nedbør** | 11 | Nedbørsmålinger og trender |
| **5. Avansert Analyse** | 14 | Statistikk, performance scores |
| **6. Ekstremverdier & Endringer** | 15 | **NYE** - Største endringer, volatilitet |
| **TOTALT** | **75** | |

---

## Kundens Krav - Dekningsgrad

### ✅ Trender over tid for fyllingsgrad
- MA7 Fyllingsgrad (%)
- MA30 Fyllingsgrad (%)
- Gjennomsnittlig Daglig Endring (pp)
- Trend Indikator

### ✅ Trender over tid for nedbør
- Nedbør Siste 7 Dager (mm)
- Nedbør Trend Indikator
- Gjennomsnittlig Ukeendring - Nedbør (mm)

### ✅ Maksimums- og minimumsverdier
- Maks Fyllingsgrad (%)
- Min Fyllingsgrad (%)
- Maks Døgnnedbør (mm)
- Største Daglig Nedbør - Alle Stasjoner (mm)
- Største Ukenedbør (mm)

### ✅ Sammenligning med året før
- YoY Endring - Magasin (GWh)
- YoY Endring - Fyllingsgrad (%)
- YoY Prosentvis Endring (%)
- Nedbør YoY Endring (mm)

### ✅ Perioder med størst endring
**Fyllingsgrad:**
- Største Daglig Endring - Fyllingsgrad (pp)
- Dato for Største Endring - Fyllingsgrad
- Største Ukeendring - Magasin (GWh)
- Område med Største Volatilitet

**Nedbør:**
- Største Daglig Nedbør - Alle Stasjoner (mm)
- Dato for Største Nedbør
- Stasjon med Største Nedbør
- Antall Kraftige Nedbørsdager (>20mm)

---

## Neste Steg

Etter implementering av measures, gå videre til:

1. **VISUALIZATIONS.md** - Opprett rapportsider
2. **USER_GUIDE.md** - Brukerveiledning
3. **README.md** - Prosjektoversikt

---

**Dokumentasjon versjon**: 2.0
**Sist oppdatert**: 2025-11-20
**Nye measures**: 15 ekstremverdi- og endringsmeasures
**Totalt antall measures**: 75
**Kontakt**: Data Engineering Team
