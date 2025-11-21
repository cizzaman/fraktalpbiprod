# Brukerveiledning - DyrWatt Analytics Dashboard

**Målgruppe**: Sluttbrukere (ledelse, driftsoperatører, analyseansvarlige)
**Formål**: Guide for å navigere og bruke DyrWatt Analytics Power BI-rapport
**Sist oppdatert**: 2025-11-19

---

## Innholdsfortegnelse

1. [Introduksjon](#introduksjon)
2. [Åpne Rapporten](#åpne-rapporten)
3. [Navigasjon](#navigasjon)
4. [Rapportsider Oversikt](#rapportsider-oversikt)
5. [Hvordan Bruke Filters](#hvordan-bruke-filters)
6. [Forstå KPIer og Metrics](#forstå-kpier-og-metrics)
7. [Vanlige Oppgaver](#vanlige-oppgaver)
8. [Tolke Visualiseringer](#tolke-visualiseringer)
9. [Alarmer og Varsler](#alarmer-og-varsler)
10. [Tips og Triks](#tips-og-triks)
11. [Ofte Stilte Spørsmål](#ofte-stilte-spørsmål)
12. [Kontakt og Support](#kontakt-og-support)

---

## Introduksjon

### Hva er DyrWatt Analytics?

DyrWatt Analytics er et Power BI dashboard som gir deg sanntidsinnblikk i:
- **Magasinfylling** i Norges 5 strømområder (NO1-NO5)
- **Tilsig og tapping** (vannbalanse)
- **Værdata** (nedbør) fra 10 værstasjoner
- **Historiske trender** og sammenligninger

### Hvem er rapporten for?

- **Kraftproduksjonsledere**: Strategiske beslutninger basert på magasinnivåer
- **Driftsoperatører**: Daglig overvåkning av tilsig, tapping, og fyllingsgrad
- **Analyseansvarlige**: Detaljerte analyser av trender og korrelasjon

### Datakilde og Oppdateringsfrekvens

- **Data**: Daglige målinger fra NVE (magasin) og Frost API (vær)
- **Oppdatering**: Automatisk hver dag kl 06:00
- **Historikk**: 90 dager rulrende vindu

---

## Åpne Rapporten

### Alternativ 1: Power BI Desktop (Lokal Fil)

1. Åpne **Power BI Desktop**
2. Klikk **File → Open**
3. Naviger til: `C:\Users\chris\Desktop\fraktalpowerbi\theme.pbip`
4. Klikk **Open**

### Alternativ 2: Power BI Service (Online)

1. Gå til [https://app.powerbi.com](https://app.powerbi.com)
2. Logg inn med din organisasjons-konto
3. Klikk på **Workspaces** (venstre sidebar)
4. Velg **DyrWatt Analytics** workspace
5. Klikk på rapporten **Magasin og Vær Dashboard**

### Første Gangs Pålogging

Hvis du blir spurt om tilkobling til datakilden:
- **Server**: `localhost`
- **Database**: `DyrWatt_Analytics`
- **Autentisering**: Windows Authentication

---

## Navigasjon

### Rapportsider

Rapporten består av 7 sider. Du navigerer mellom sider ved å:
- Klikke på **sidemenyen** nederst i rapporten
- Bruke **piltaster** (← →) på tastaturet

### Sidemeny

```
[ 1. Executive Dashboard ]  [ 2. Magasin ]  [ 3. Tilsig ]  [ 4. Vær ]
[ 5. Year-over-Year ]  [ 6. Alarmer ]  [ 7. Månedlig ]
```

### Zoom og Visning

- **Zoom in/ut**: `Ctrl + +` / `Ctrl + -` (eller zoom-slider nederst til høyre)
- **Fit to page**: Klikk **View → Fit to Page**
- **Fullskjerm**: Trykk `F11` (eller **View → Fullscreen**)

---

## Rapportsider Oversikt

### Side 1: Executive Dashboard

**Formål**: Rask oversikt med viktigste KPIer for daglig ledelsesrapportering.

**Nøkkelinformasjon**:
- Gjennomsnittlig fyllingsgrad for alle områder
- Total magasininnhold (GWh)
- Antall kritiske områder
- Year-over-Year endring
- Netto endring (fylles eller tømmes magasinene?)

**Bruksscenarioer**:
- Daglig morgenmøte: Få øyeblikkelig status
- Executive briefing: Presenter nøkkeltall for ledelse

---

### Side 2: Magasin Detaljanalyse

**Formål**: Detaljert analyse av magasindata per område.

**Nøkkelinformasjon**:
- Fyllingsgrad heatmap (visuell oversikt over tid)
- Gauge charts per område (NO1-NO5)
- Distribusjon av fyllingsgrad (histogram)
- Best/worst presterende områder

**Bruksscenarioer**:
- Identifisere hvilke områder som trenger oppmerksomhet
- Sammenligne fyllingsgrad mellom områder
- Analysere distribusjon og variabilitet

---

### Side 3: Tilsig & Tapping

**Formål**: Analyse av vannbalanse.

**Nøkkelinformasjon**:
- Waterfall chart (visualiserer vannbalansen)
- Tilsig trend (vanninngang)
- Tapping trend (produksjon)
- Korrelasjon mellom tilsig og tapping

**Bruksscenarioer**:
- Forstå hvorfor magasinet fylles eller tømmes
- Planlegge produksjon basert på tilsig
- Analysere vannbalansen per område

---

### Side 4: Vær & Nedbør

**Formål**: Analyse av værdata og korrelasjon med tilsig.

**Nøkkelinformasjon**:
- Kart over Norge med nedbør per værstasjon
- Nedbør trend over tid
- Korrelasjon mellom nedbør og tilsig
- Våteste/tørreste stasjoner

**Bruksscenarioer**:
- Forstå hvordan nedbør påvirker tilsig
- Identifisere geografiske mønstre
- Forutse tilsig basert på værprognoser

---

### Side 5: Year-over-Year

**Formål**: Historiske sammenligninger.

**Nøkkelinformasjon**:
- Sammenligning av nåværende år vs forrige år
- Månedlige YoY-endringer
- Sesongindeks (avvik fra årsgjennomsnitt)

**Bruksscenarioer**:
- Evaluere om vi er foran eller bak i forhold til tidligere år
- Identifisere sesongmønstre
- Benchmarking mot historiske data

---

### Side 6: Ekstremverdier & Alarmer

**Formål**: Overvåkning av kritiske verdier og alarmer.

**Nøkkelinformasjon**:
- Antall kritiske områder (fyllingsgrad < 30%)
- Risiko score per område
- Estimert tid til kritisk nivå
- Største endringer siste dager

**Bruksscenarioer**:
- Identifisere umiddelbar risiko
- Prioritere hvilke områder som trenger handling
- Overvåke trender mot kritiske nivåer

---

### Side 7: Månedlig Oppsummering

**Formål**: Aggregerte månedlige data for rapportering.

**Nøkkelinformasjon**:
- Matrix tabell med alle KPIer per måned
- Månedlig fyllingsgrad trend
- Månedlig vannbalanse (tilsig vs tapping)

**Bruksscenarioer**:
- Månedlige rapporter til ledelse
- Langsiktig trendanalyse
- Aggregerte statistikker for planlegging

---

## Hvordan Bruke Filters

### Global Filters (Slicers)

Global filters finnes på **alle sider** og påvirker alle visualiseringer.

#### Dato Slicer

**Plassering**: Øverst til høyre på alle sider

**Bruk**:
1. Klikk på slicer-feltet
2. Velg **start-dato** (f.eks. 2025-08-01)
3. Velg **slutt-dato** (f.eks. 2025-11-19)
4. Alle visuals oppdateres automatisk

**Tips**: Bruk predefinerte perioder:
- Last 7 days
- Last 30 days
- Last 90 days

---

#### Område Slicer

**Plassering**: Øverst (ved siden av dato slicer)

**Bruk**:
1. Klikk på checkbox for området(e) du vil se:
   - NO1 (Sør-Øst Norge - Oslo)
   - NO2 (Sør-Vest Norge - Kristiansand)
   - NO3 (Midt-Norge - Trondheim)
   - NO4 (Nord-Norge - Tromsø)
   - NO5 (Vest-Norge - Bergen)
2. Du kan velge flere områder samtidig
3. Klikk **Select All** for å velge alle

**Tips**: Sammenlign to områder ved å velge kun disse to

---

### Visual-Specific Filters

Noen visuals har egne filters som kun påvirker den visuelle.

**Eksempel**: Gauge charts på Side 2 viser kun ett område hver

**Hvordan**:
- Klikk på visual
- Gå til **Filters pane** (høyre side)
- Se hvilke filters som er aktive

---

## Forstå KPIer og Metrics

### Fyllingsgrad (%)

**Definisjon**: Andel av magasinets kapasitet som er fylt med vann.

**Formel**: (Magasininnhold / Kapasitet) × 100

**Tolkning**:
- **< 30%**: Kritisk lav - Rød alarm
- **30-50%**: Lav - Oransje advarsel
- **50-80%**: Normal - Grønn OK
- **> 80%**: Høy - Blå (overfylt)

**Målnivå**: 70% anses som optimalt

---

### Magasininnhold (GWh)

**Definisjon**: Mengde vann lagret i magasinet, målt i gigawatt-timer (GWh).

**Kontekst**: 1 GWh tilsvarer 1 million kWh

**Eksempel**: 10 000 GWh kan forsyne 500 000 husstander i én måned

---

### Tilsig (GWh)

**Definisjon**: Vanninngang til magasinet (fra nedbør, snøsmelting, elver).

**Sesongvariasjoner**:
- **Vår (mars-mai)**: Høyt tilsig (snøsmelting)
- **Sommer (juni-aug)**: Moderat tilsig (nedbør)
- **Høst (sept-nov)**: Moderat til lavt tilsig
- **Vinter (des-feb)**: Lavt tilsig (frossen vannføring)

---

### Tapping (GWh)

**Definisjon**: Vannuttaking fra magasinet for kraftproduksjon.

**Påvirkning**:
- Høy tapping = Økt kraftproduksjon = Lavere magasinnivå
- Lav tapping = Mindre kraftproduksjon = Høyere magasinnivå

---

### Netto Endring (GWh)

**Definisjon**: Forskjell mellom tilsig og tapping.

**Formel**: Netto Endring = Tilsig - Tapping

**Tolkning**:
- **Positiv**: Magasinet fylles opp (tilsig > tapping)
- **Negativ**: Magasinet tømmes (tapping > tilsig)
- **Null**: Balanse (tilsig = tapping)

---

### YoY (Year-over-Year) Endring

**Definisjon**: Sammenligning med samme periode i fjor.

**Formel**: YoY Endring = I år - I fjor

**Eksempel**:
- Fyllingsgrad i dag: 65%
- Fyllingsgrad samme dag i fjor: 60%
- YoY Endring: +5 prosentpoeng

**Tolkning**:
- Positiv: Bedre stilling enn i fjor
- Negativ: Dårligere stilling enn i fjor

---

### Nedbør (mm)

**Definisjon**: Nedbørsmengde målt i millimeter (mm).

**Kontekst**: 1 mm nedbør = 1 liter vann per m²

**Korrelasjon**: Økt nedbør → Økt tilsig (med 1-14 dagers forsinkelse)

---

## Vanlige Oppgaver

### Oppgave 1: Sjekk Daglig Status

**Mål**: Få rask oversikt over dagens situasjon.

**Steg**:
1. Åpne rapporten
2. Gå til **Side 1: Executive Dashboard**
3. Sjekk de 5 KPI cards øverst:
   - Gjennomsnittlig fyllingsgrad
   - Total magasininnhold
   - Kritiske områder (skal være 0)
   - YoY endring
   - Netto endring (fylles eller tømmes?)
4. Sjekk **Fyllingsgrad Trend** linjegrafen
   - Går trenden oppover eller nedover?
5. Sjekk **Magasin per Område** søylediagram
   - Hvilke områder har lavest fylling?

**Forventet tid**: 1-2 minutter

---

### Oppgave 2: Identifiser Områder med Lav Fyllingsgrad

**Mål**: Finne hvilke områder som trenger oppmerksomhet.

**Steg**:
1. Gå til **Side 6: Ekstremverdier & Alarmer**
2. Sjekk **Kritisk Alert Count** card
   - Hvis > 0, se tabell under
3. Sjekk **Kritiske Områder Tabell**
   - Ser hvilke områder som har status "Kritisk" eller "Lav"
   - Noter **Dager til Kritisk Nivå** kolonne
4. Gå til **Side 2: Magasin Detaljanalyse**
   - Klikk på området i **Område Slicer**
   - Se detaljert historikk i gauge chart og heatmap

**Forventet tid**: 3-5 minutter

---

### Oppgave 3: Sammenlign Med Forrige År

**Mål**: Evaluere om vi er foran eller bak i forhold til i fjor.

**Steg**:
1. Gå til **Side 5: Year-over-Year**
2. Sjekk **YoY Sammenligning** linjegrafen
   - Blå linje: I år
   - Grå stiplet linje: I fjor
   - Er blå linjen over eller under grå?
3. Sjekk **YoY Endring per Måned** søylediagram
   - Grønne søyler: Bedre enn i fjor
   - Røde søyler: Dårligere enn i fjor
4. Sjekk **YoY % Endring Matrix**
   - Prosentvis endring per område og måned

**Forventet tid**: 3-5 minutter

---

### Oppgave 4: Forstå Vannbalansen

**Mål**: Analysere tilsig og tapping for å forstå hvorfor magasinet endres.

**Steg**:
1. Gå til **Side 3: Tilsig & Tapping**
2. Sjekk **Vannbalanse Waterfall Chart**
   - Visualiserer hvordan tilsig og tapping påvirker magasinnivået
3. Sjekk **Tilsig Trend** og **Tapping Trend** area charts
   - Sammenlign de to grafene
   - Er tilsig høyere enn tapping?
4. Sjekk **Tilsig vs Tapping Scatter Plot**
   - Punkter over diagonal: Tilsig > Tapping (fylles)
   - Punkter under diagonal: Tapping > Tilsig (tømmes)
5. Sjekk **Vannbalanse Ratio** card
   - > 1.0: Fylles
   - < 1.0: Tømmes

**Forventet tid**: 5-7 minutter

---

### Oppgave 5: Analysere Værpåvirkning

**Mål**: Forstå hvordan nedbør påvirker tilsig.

**Steg**:
1. Gå til **Side 4: Vær & Nedbør**
2. Sjekk **Norge Kart** med nedbør per stasjon
   - Større bobler = mer nedbør
   - Mørkere farge = høyere gjennomsnitt
3. Sjekk **Nedbør vs Tilsig** combo chart
   - Blå søyler: Nedbør
   - Grønn linje: Tilsig
   - Er det korrelasjon? (tilsig øker 1-7 dager etter nedbør)
4. Sjekk **Våteste Stasjon** og **Tørreste Stasjon** cards
5. Sjekk **Korrelasjon Tabell**
   - Positiv korrelasjon = nedbør gir økt tilsig

**Forventet tid**: 5-7 minutter

---

### Oppgave 6: Lage Månedlig Rapport

**Mål**: Eksportere data for månedlig rapportering til ledelse.

**Steg**:
1. Gå til **Side 7: Månedlig Oppsummering**
2. Bruk **Dato Slicer** for å velge ønsket måned
3. Sjekk **Månedlig Matrix Tabell**
   - Inneholder alle KPIer per måned og område
4. Eksporter tabell:
   - Klikk **...** (tre prikker) øverst til høyre på visual
   - Velg **Export data**
   - Velg **Excel** eller **CSV**
5. Åpne eksportert fil i Excel
6. Formater og legg til kommentarer
7. Send til ledelse

**Forventet tid**: 10-15 minutter

---

## Tolke Visualiseringer

### Line Chart (Linjediagram)

**Bruk**: Vise trend over tid

**Hvordan lese**:
- **X-akse**: Tid (datoer)
- **Y-akse**: Verdi (f.eks. fyllingsgrad %)
- **Linje oppover**: Økende trend
- **Linje nedover**: Synkende trend
- **Flat linje**: Stabil

**Interaksjon**:
- Hover over linje for å se eksakt verdi
- Klikk på legend for å skjule/vise linjer

---

### Column Chart (Søylediagram)

**Bruk**: Sammenligne verdier mellom kategorier

**Hvordan lese**:
- **X-akse**: Kategorier (f.eks. NO1-NO5)
- **Y-akse**: Verdi (f.eks. GWh)
- **Høyere søyle**: Høyere verdi

**Interaksjon**:
- Klikk på søyle for å filtrere andre visuals

---

### Matrix (Tabell med Grupper)

**Bruk**: Vise data i tabell-format med grupper

**Hvordan lese**:
- **Rader**: Kategorier (f.eks. måneder)
- **Kolonner**: Metrics (f.eks. fyllingsgrad, tilsig)
- **Celler**: Verdier
- **Farger**: Conditional formatting (rød/grønn)

**Interaksjon**:
- Klikk **+** for å ekspandere grupper
- Sorter ved å klikke på kolonneheader

---

### Gauge Chart (Målediagram)

**Bruk**: Vise enkeltverdi i forhold til mål

**Hvordan lese**:
- **Nål**: Gjeldende verdi
- **Target linje**: Målnivå (70%)
- **Farger**:
  - Rød: Kritisk (< 30%)
  - Oransje: Lav (30-50%)
  - Grønn: Normal (50-80%)
  - Blå: Høy (> 80%)

---

### Heatmap (Varmekart)

**Bruk**: Vise mønstre i data over tid og kategorier

**Hvordan lese**:
- **Rader**: Tid (f.eks. uker)
- **Kolonner**: Kategorier (f.eks. områder)
- **Farger**:
  - Rød: Lave verdier
  - Gul: Middels verdier
  - Grønn: Høye verdier

**Interaksjon**:
- Hover over celle for å se eksakt verdi

---

### Waterfall Chart

**Bruk**: Visualisere hvordan en verdi endres gjennom flere steg

**Hvordan lese**:
- **Start søyle**: Initial verdi
- **Grønne søyler**: Økning (f.eks. tilsig)
- **Røde søyler**: Reduksjon (f.eks. tapping)
- **Slutt søyle**: Endelig verdi

**Eksempel**: Magasininnhold → +Tilsig → -Tapping → Nytt magasininnhold

---

### Scatter Plot (Punktdiagram)

**Bruk**: Vise korrelasjon mellom to variabler

**Hvordan lese**:
- **X-akse**: Første variabel (f.eks. tilsig)
- **Y-akse**: Andre variabel (f.eks. tapping)
- **Punkter**: Hver observasjon
- **Diagonal linje**: Perfekt korrelasjon (X=Y)

**Tolkning**:
- Punkter på linje: Perfekt korrelasjon
- Punkter spredt: Lav korrelasjon

---

## Alarmer og Varsler

### Fyllingsgrad Status

Systemet kategoriserer fyllingsgrad i 4 nivåer:

| Status | Fyllingsgrad | Farge | Betydning |
|--------|--------------|-------|-----------|
| **Kritisk** | < 30% | Rød | Umiddelbar handling påkrevd |
| **Lav** | 30-50% | Oransje | Oppmerksomhet anbefalt |
| **Normal** | 50-80% | Grønn | Alt OK |
| **Høy** | > 80% | Blå | Høy fyllingsgrad, overvåk tapping |

---

### Kritisk Alert Count

**Definisjon**: Antall områder med fyllingsgrad < 30%

**Tolkning**:
- **0**: Ingen kritiske områder
- **1-2**: Noen områder trenger oppmerksomhet
- **3+**: Flere områder i kritisk tilstand

**Aksjon**:
- Gå til **Side 6: Ekstremverdier & Alarmer**
- Sjekk **Kritiske Områder Tabell**
- Evaluer tiltak (øke tilsig, redusere tapping)

---

### Dager til Kritisk Nivå

**Definisjon**: Estimert antall dager før magasinet når 30% basert på gjeldende trend.

**Formel**: (Gjeldende fylling - 30%) / Gjennomsnittlig daglig endring (siste 7 dager)

**Tolkning**:
- **< 7 dager**: Umiddelbar risiko
- **7-30 dager**: Moderat risiko
- **> 30 dager**: Lav risiko
- **999 (eller blank)**: Ingen risiko (fylles opp)

**Aksjon**:
- Hvis < 7 dager: Evaluer tiltak umiddelbart
- Hvis 7-30 dager: Planlegg tiltak

---

### Risiko Score

**Definisjon**: Kombinert score (0-100) basert på fyllingsgrad, trend, og stabilitet.

**Komponenter**:
- Fyllingsgrad (lav fylling = høy score)
- Trend (synkende trend = høy score)

**Tolkning**:
- **0-25**: Lav risiko (Grønn)
- **25-50**: Moderat risiko (Gul)
- **50-75**: Høy risiko (Oransje)
- **75-100**: Kritisk risiko (Rød)

---

## Tips og Triks

### Tip 1: Bruk Keyboard Shortcuts

- **→ / ←**: Naviger mellom rapportsider
- **Ctrl + F**: Søk i rapporten
- **F11**: Fullskjerm
- **Ctrl + +/-**: Zoom inn/ut

---

### Tip 2: Hover for Detaljer

Hover med musen over **alle visualiseringer** for å se:
- Eksakte verdier
- Tilleggsinformasjon
- Custom tooltips

---

### Tip 3: Klikk for Cross-Filtering

Klikk på en verdi i en visual (f.eks. NO1 i et søylediagram) for å:
- Filtrere alle andre visuals på siden
- Se kun data for valgt kategori

**For å tilbakestille**: Klikk på samme verdi igjen

---

### Tip 4: Bruk Bokmerker (Bookmarks)

Hvis rapporten har bokmerker (laget av utvikler):
1. Klikk **View → Bookmarks pane**
2. Velg et bokmerke fra listen
3. Rapporten hopper til forhåndsdefinert visning

---

### Tip 5: Eksporter Data

Eksporter data fra **alle visuals**:
1. Klikk **...** (tre prikker) øverst til høyre på visual
2. Velg **Export data**
3. Velg format:
   - **Summarized data**: Aggregert data (som vist i visual)
   - **Underlying data**: Detaljert data
4. Velg **Excel** eller **CSV**

---

### Tip 6: Bruk Focus Mode

For å fokusere på én visual:
1. Hover over visual
2. Klikk **Focus mode** ikon (øverst til høyre)
3. Visual ekspanderer til fullskjerm
4. Klikk **Back to report** for å gå tilbake

---

### Tip 7: Skriv ut Rapport

For å skrive ut en rapportside:
1. Gå til ønsket side
2. Klikk **File → Print this page**
3. Velg printer eller **Save as PDF**
4. Klikk **Print**

---

### Tip 8: Deling av Rapport

For å dele en side med kolleger:
1. Klikk **File → Export**
2. Velg **PDF** eller **PowerPoint**
3. Velg hvilke sider som skal eksporteres
4. Klikk **Export**
5. Send fil per e-post

---

## Ofte Stilte Spørsmål

### Q1: Hvorfor ser jeg ingen data?

**Mulige årsaker**:
1. **Filters er for restriktive**
   - Sjekk dato slicer (utvid tidsperioden)
   - Sjekk område slicer (velg flere områder)
2. **Data ikke oppdatert**
   - Sjekk siste oppdateringstidspunkt (øverst til høyre)
   - Kontakt IT hvis > 24 timer siden siste oppdatering
3. **Tilkoblingsfeil til database**
   - Kontakt IT support

---

### Q2: Hvordan ser jeg kun ett område?

**Svar**:
1. Bruk **Område Slicer** øverst på siden
2. Fjern hake i alle områder unntatt det du vil se
3. Alternativt: Klikk på området i en visual (f.eks. søylediagram)

---

### Q3: Kan jeg sammenligne to områder side-ved-side?

**Svar**:
Ja, bruk **Område Slicer**:
1. Velg kun de to områdene du vil sammenligne
2. Alle visuals viser nå kun disse to områdene
3. Bruk **Side 2: Magasin Detaljanalyse** for best sammenligning

---

### Q4: Hvordan eksporterer jeg data til Excel?

**Svar**:
1. Klikk **...** (tre prikker) på ønsket visual
2. Velg **Export data**
3. Velg **Excel**
4. Fil lastes ned automatisk
5. Åpne fil i Excel

---

### Q5: Hvorfor ser jeg "Blank" i noen visuals?

**Mulige årsaker**:
1. **Ingen data for valgt periode/område**
   - Utvid dato slicer
2. **Measures kan ikke beregnes**
   - F.eks. YoY endring krever data fra i fjor
3. **Divisjon med null**
   - F.eks. hvis tapping = 0, kan ikke beregne ratio

---

### Q6: Hva betyr "YoY"?

**Svar**: Year-over-Year = Sammenligning med samme periode i fjor.

**Eksempel**: Hvis vi er i november 2025, sammenligner YoY med november 2024.

---

### Q7: Hvordan tolker jeg waterfall chart?

**Svar**:
- **Start søyle** (venstre): Initial magasininnhold
- **Grønne søyler**: Økning (tilsig legges til)
- **Røde søyler**: Reduksjon (tapping trekkes fra)
- **Slutt søyle** (høyre): Resulterende magasininnhold

---

### Q8: Kan jeg endre visningen (f.eks. fra diagram til tabell)?

**Svar**: Nei, som sluttbruker kan du ikke endre visual type. Kontakt utvikler hvis du trenger andre visualiseringer.

---

### Q9: Hvordan oppdaterer jeg data?

**Svar**:
- **Power BI Desktop**: Klikk **Home → Refresh**
- **Power BI Service**: Data oppdateres automatisk kl 06:00 hver dag

---

### Q10: Kan jeg lage mine egne measures?

**Svar**:
- **Power BI Desktop**: Ja, hvis du har redigeringstilgang
- **Power BI Service**: Nei, kontakt utvikler for nye measures

---

## Kontakt og Support

### IT Support

**E-post**: [it-support@dyrwatt.no](mailto:it-support@dyrwatt.no)
**Telefon**: +47 XX XX XX XX
**Åpningstider**: Man-Fre 08:00-16:00

### Data Engineering Team

**E-post**: [data-team@dyrwatt.no](mailto:data-team@dyrwatt.no)

**For spørsmål om**:
- Datakilder
- Nye measures
- Tilpasninger av rapporten

### Power BI Administrator

**E-post**: [powerbi-admin@dyrwatt.no](mailto:powerbi-admin@dyrwatt.no)

**For spørsmål om**:
- Tilganger og rettigheter
- Publisering til Power BI Service
- Scheduled refresh

---

## Tilbakemelding

Vi verdsetter din tilbakemelding! Hvis du har:
- Forslag til forbedringer
- Bugs eller feil
- Ønsker om nye funksjoner

Send e-post til: [data-feedback@dyrwatt.no](mailto:data-feedback@dyrwatt.no)

---

**Dokumentasjon versjon**: 1.0
**Sist oppdatert**: 2025-11-19
**Forfatter**: Data Engineering Team
**Godkjent av**: DyrWatt AS Ledelse

---

**Neste steg**: Se `README.md` for teknisk prosjektoversikt
