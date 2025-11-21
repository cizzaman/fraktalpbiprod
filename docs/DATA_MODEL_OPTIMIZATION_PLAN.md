# Data Model Optimization Plan
## Decluttering the Power BI Semantic Model

**Author:** Claude
**Date:** 2025-11-21
**Project:** DyrWatt Analytics - Magasin og Vær Dashboard
**Goal:** Organize data model into clean layers to reduce clutter while maintaining all functionality

---

## Problem Statement

The current data model is cluttered because report developers see:
- ✅ **7 base tables** (3 fact + 4 dimension tables)
- ✅ **6 report views** (4 used + 2 unused)
- ❌ **100+ DAX measures scattered across fact tables**
- ❌ **Duplicate data** (both base tables AND views visible)

This creates confusion about which tables/views to use for building visuals.

---

## Solution: 3-Layer Architecture

### **Layer 1: REPORT LAYER (Visible)**
Tables/views that report developers should use:

| Object | Type | Purpose | Visible |
|--------|------|---------|---------|
| `_Measures` | Measures Table | All 100+ DAX measures in one place | ✅ YES |
| `dim_date` | Dimension | Calendar/date filtering | ✅ YES |
| `dim_omraade` | Dimension | Area filtering (NO1-NO5) | ✅ YES |
| `dim_weather_station` | Dimension | Weather station filtering | ✅ YES |
| `dim_month` | Dimension | Monthly filtering | ✅ YES |
| `report vw_extremes` | Analytical View | Max/min values analysis | ✅ YES |
| `report vw_largest_changes` | Analytical View | Largest changes analysis | ✅ YES |
| `report vw_period_comparison` | Analytical View | Year-over-year comparison | ✅ YES |
| `report vw_weather_daily` | Analytical View | Weather/precipitation trends | ✅ YES |

**Total Visible Objects:** 9 tables/views (clean, purposeful)

### **Layer 2: DATA LAYER (Hidden)**
Base fact tables - hidden but still used by DAX measures:

| Object | Type | Purpose | Visible |
|--------|------|---------|---------|
| `dw fact_magasin` | Fact Table | Daily reservoir data (used by measures) | 🔒 HIDDEN |
| `dw fact_weather` | Fact Table | Daily weather data (used by measures) | 🔒 HIDDEN |
| `dw fact_magasin_monthly` | Fact Table | Monthly aggregates (used by measures) | 🔒 HIDDEN |

### **Layer 3: ARCHIVE LAYER (Hidden)**
Unused views - hidden for future reference:

| Object | Type | Purpose | Visible |
|--------|------|---------|---------|
| `report vw_monthly_summary` | View | Unused - no references found | 🔒 HIDDEN |
| `report vw_area_combined` | View | Unused - no references found | 🔒 HIDDEN |

---

## Implementation Steps

### **Phase 1: Create Centralized Measures Table** ✅ DONE

1. ✅ Created `_Measures.tmdl` table
2. ✅ Added starter measures (8 basic measures as template)
3. ⏳ **TODO:** Migrate ALL remaining measures from `dw fact_magasin` and `dw fact_weather`

**Why underscore prefix?**
- The `_` prefix makes the Measures table appear first in alphabetical lists
- Makes it easy to find in the Fields pane
- Industry best practice for Power BI models

### **Phase 2: Migrate All DAX Measures** ⏳ PENDING

**From `dw fact_magasin` (~90 measures):**
```
Total Magasininnhold (GWh) ✅ DONE
Gjennomsnittlig Fyllingsgrad (%) ✅ DONE
Total Tilsig (GWh) ✅ DONE
Total Tapping (GWh) ✅ DONE
Netto Endring (GWh) ✅ DONE
Kapasitetsutnyttelse (%)
... (~85 more measures to migrate)
```

**From `dw fact_weather` (~10 measures):**
```
Total Nedbør (mm) ✅ DONE
Gjennomsnittlig Daglig Nedbør (mm) ✅ DONE
Maks Døgnnedbør (mm) ✅ DONE
Antall Regnværsdager
Nedbør Siste 7 Dager (mm)
... (~5 more measures to migrate)
```

**Migration Process:**
1. Read full `dw fact_magasin.tmdl` file to extract all measures
2. Read full `dw fact_weather.tmdl` file to extract all measures
3. Copy all measure definitions to `_Measures.tmdl`
4. Organize measures into logical sections with comments
5. Remove measure definitions from original fact tables (keep only columns)

### **Phase 3: Hide Base Fact Tables** ⏳ PENDING

Add `isHidden` property to base fact tables:

**File:** `dw fact_magasin.tmdl`
```tmdl
table 'dw fact_magasin'
	isHidden                           ← ADD THIS LINE
	lineageTag: 9fd5f720-5277-41f8-b35e-344d200a22be
	...
```

**File:** `dw fact_weather.tmdl`
```tmdl
table 'dw fact_weather'
	isHidden                           ← ADD THIS LINE
	lineageTag: cb8d6bb8-ae0f-47f4-a59e-ed91543d1491
	...
```

**File:** `dw fact_magasin_monthly.tmdl`
```tmdl
table 'dw fact_magasin_monthly'
	isHidden                           ← ADD THIS LINE
	lineageTag: [existing-lineage-tag]
	...
```

### **Phase 4: Hide Unused Report Views** ⏳ PENDING

Add `isHidden` property to unused views:

**File:** `report vw_monthly_summary.tmdl`
```tmdl
table 'report vw_monthly_summary'
	isHidden                           ← ADD THIS LINE
	lineageTag: 8a47ee2d-867a-4275-9581-5f191d820c16
	...
```

**File:** `report vw_area_combined.tmdl`
```tmdl
table 'report vw_area_combined'
	isHidden                           ← ADD THIS LINE
	lineageTag: f14a88ff-0473-4e67-9be6-8492627f85f2
	...
```

### **Phase 5: Update Model Reference** ⏳ PENDING

Add `_Measures` table to model.tmdl:

**File:** `fraktalprod.SemanticModel/definition/model.tmdl`
```tmdl
model Model
	culture: en-US
	...

annotation PBI_QueryOrder = ["_Measures","dw dim_date","dw dim_omraade",...] ← ADD _Measures FIRST

ref table '_Measures'              ← ADD THIS LINE
ref table 'dw dim_date'
ref table 'dw dim_omraade'
...
```

### **Phase 6: Testing & Validation** ⏳ PENDING

1. Open the .pbip file in Power BI Desktop
2. Verify in Model View:
   - ✅ `_Measures` table is visible and contains all measures
   - ✅ Dimension tables are visible (dim_date, dim_omraade, etc.)
   - ✅ Active report views are visible (vw_extremes, vw_largest_changes, etc.)
   - ❌ Base fact tables are hidden (fact_magasin, fact_weather, fact_magasin_monthly)
   - ❌ Unused views are hidden (vw_monthly_summary, vw_area_combined)
3. Check Report View - verify all 7 pages still work:
   - Page 1: Executive Overview
   - Page 2: Magasin Analyse
   - Page 3: Tilsig & Tapping
   - Page 4: Vær & Nedbør
   - Page 5: Year-over-Year
   - Page 6: Ekstremverdier
   - Page 7: Månedlig Oppsummering
4. Test all visuals render correctly
5. Test all slicers and filters work
6. Test all DAX measures calculate correctly

---

## Before & After Comparison

### **BEFORE (Current State)**
Fields Pane shows 13 tables:
```
📊 dw fact_magasin (450 rows + 90 measures) ← CLUTTERED
📊 dw fact_weather (900 rows + 10 measures) ← CLUTTERED
📊 dw fact_magasin_monthly (20 rows)
📐 dim_date
📐 dim_omraade
📐 dim_weather_station
📐 dim_month
📄 report vw_extremes
📄 report vw_area_combined ← UNUSED
📄 report vw_largest_changes
📄 report vw_monthly_summary ← UNUSED
📄 report vw_period_comparison
📄 report vw_weather_daily
```
**Problems:**
- 13 tables visible (confusing!)
- Measures scattered across fact tables
- Unused views cluttering the list
- Unclear which tables to use

### **AFTER (Optimized State)**
Fields Pane shows 9 objects:
```
📊 _Measures (100+ measures organized) ← CLEAN!
📐 dim_date
📐 dim_omraade
📐 dim_month
📐 dim_weather_station
📄 report vw_extremes
📄 report vw_largest_changes
📄 report vw_period_comparison
📄 report vw_weather_daily
```
**Benefits:**
- ✅ **9 objects** instead of 13 (31% reduction)
- ✅ **All measures in one place** (easy to find)
- ✅ **Clear purpose** for each visible object
- ✅ **Cleaner Fields pane** in Power BI Desktop
- ✅ **Better user experience** for report developers
- ✅ **Best practice architecture**

---

## Benefits of This Approach

### **1. Cleaner Model**
- Report developers see only what they need
- Fields pane is organized and purposeful
- No confusion about which tables to use

### **2. Better Performance**
- Hiding tables doesn't affect query performance
- Measures still reference hidden fact tables (works perfectly)
- Relationships remain intact

### **3. Easier Maintenance**
- All measures in one centralized location
- Easy to find and update DAX formulas
- Clear separation between data layer and presentation layer

### **4. Follows Best Practices**
- Industry standard pattern for Power BI models
- Recommended by Microsoft Power BI team
- Makes model easier to document and train users

### **5. Preserves Functionality**
- ✅ All existing visuals continue to work
- ✅ All DAX measures function identically
- ✅ All relationships remain active
- ✅ No data loss or corruption
- ✅ Report views still accessible to visuals
- ✅ Hidden tables still used by measures

---

## Why This Works

### **How Hidden Tables Work in Power BI:**

1. **Hiding a table** = Removing it from the Fields pane UI
2. **Hidden tables are still in the model**
   - They still store data
   - They still participate in relationships
   - DAX measures can still reference them
   - They still refresh normally

3. **Hidden tables reduce clutter without breaking functionality**
   - Users don't see them in report development
   - Power Query still loads them
   - Relationships still work
   - Measures still calculate correctly

### **Example:**
```dax
// This measure works even though 'dw fact_magasin' is hidden
measure 'Total Magasininnhold (GWh)' =
    SUM('dw fact_magasin'[magasininnhold_GWh])
```

The measure references the hidden table - this is **intentional and correct**!

---

## Next Steps

### **Option A: Full Automated Migration** (Recommended)
I can automatically:
1. Extract ALL 100+ measures from fact tables
2. Create complete `_Measures.tmdl` with all measures organized
3. Remove measures from fact table files
4. Add `isHidden` properties to appropriate tables
5. Update model.tmdl with correct references
6. Generate validation checklist

**Time:** ~5 minutes
**Risk:** Low (I'll validate all syntax)
**Benefit:** Complete optimization in one go

### **Option B: Manual Step-by-Step**
I provide you with:
1. Instructions for each file to edit
2. Exact lines to add/remove
3. You make changes manually in your preferred editor

**Time:** ~30 minutes
**Risk:** Medium (manual copy/paste errors possible)
**Benefit:** You control every change

### **Option C: Partial Optimization**
Just hide the base fact tables now:
- Add `isHidden` to 3 fact tables
- Add `isHidden` to 2 unused views
- Keep measures where they are (in fact tables)

**Time:** ~2 minutes
**Risk:** Very low
**Benefit:** Quick win, reduces clutter by ~40%

---

## Recommendation

**I recommend Option A (Full Automated Migration)** because:
- ✅ Achieves maximum decluttering (9 visible objects vs 13)
- ✅ Follows industry best practices
- ✅ All measures centralized = easier maintenance
- ✅ I can validate all DAX syntax during migration
- ✅ Complete solution in one session

Would you like me to proceed with **Option A** and fully optimize your data model?

---

## Files to be Modified

### **Created:**
- ✅ `_Measures.tmdl` (starter created, needs full measures)
- ✅ `docs/DATA_MODEL_OPTIMIZATION_PLAN.md` (this file)

### **To Modify:**
- `dw fact_magasin.tmdl` (remove measures, add isHidden)
- `dw fact_weather.tmdl` (remove measures, add isHidden)
- `dw fact_magasin_monthly.tmdl` (add isHidden)
- `report vw_monthly_summary.tmdl` (add isHidden)
- `report vw_area_combined.tmdl` (add isHidden)
- `_Measures.tmdl` (add all remaining measures)
- `model.tmdl` (add reference to _Measures)

### **No Changes:**
- `dim_date.tmdl` (keep visible)
- `dim_omraade.tmdl` (keep visible)
- `dim_weather_station.tmdl` (keep visible)
- `dim_month.tmdl` (keep visible)
- `report vw_extremes.tmdl` (keep visible)
- `report vw_largest_changes.tmdl` (keep visible)
- `report vw_period_comparison.tmdl` (keep visible)
- `report vw_weather_daily.tmdl` (keep visible)
- `relationships.tmdl` (no changes needed)
- All report page files (no changes needed)

---

**End of Optimization Plan**
