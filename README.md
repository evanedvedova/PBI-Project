# Measuring Hope: Data Analytics in Cancer Trials

## 1. Project Overview
This project, **Measuring Hope: Data Analytics in Cancer Trials**, analyzes clinical trial data with a focus on **oncology-related studies** and their characteristics.

The primary goal is to explore:
- clinical trial phases,
- intervention types (with emphasis on biologics),
- reasons for trial termination.

The report was developed in **Microsoft Power BI Desktop**.

---

## 2. Data Source
- **Source:** AACT (Aggregate Analysis of ClinicalTrials.gov)
- **Provider:** Clinical Trials Transformation Initiative (CTTI)
- **Website:** https://aact.ctti-clinicaltrials.org/
- **Format:** Pipe-delimited (`|`) text files

---

## 3. Data Transformation (Power Query)

All data preparation was performed in Power Query using the Advanced Editor.  
Transformations focus on isolating oncology-related clinical trials and simplifying the raw AACT schema.

### 3.1 Oncology Trial Identification (`oncology_only_nct_id`)
Steps:
1. Import `browse_conditions.txt`
2. Promote headers and enforce data types
3. Create `Is_Oncology` flag using keyword-based text matching on the `downcase_mesh_term` field. A trial was classified as oncology-related
  if the condition text contained any of the following terms: 
  cancer, carcinoma, adenocarcinoma, sarcoma, lymphoma, leukemia, myeloma, melanoma, glioma, blastoma, mesothelioma, neoplasm, malignan.
4. Filter rows where `Is_Oncology = true`
5. Remove unnecessary columns
6. Deduplicate records by `nct_id`

**Result:** Distinct list of oncology trial identifiers.

---

### 3.2 Studies Table (`studies`)
Steps:
1. Import `studies.txt`
2. Promote headers and apply correct data types
3. Remove unused metadata columns
4. Join with `oncology_only_nct_id` on `nct_id`
5. Retain analytically relevant fields

**Result:** Cleaned fact table containing oncology trials only.

---

### 3.3 Interventions Table (`interventions`)
Steps:
1. Import `interventions.txt`
2. Promote headers and enforce data types
3. Join with `oncology_only_nct_id`
4. Remove free-text description fields

**Result:** Structured intervention data for oncology trials.

---

## 4. Data Model
- **Model type:** Relational
- **Primary key:** `nct_id`
- **Core tables:**
  - `studies`
  - `interventions`
- Single-direction relationships to ensure model stability

---

## 5. Measures & Calculations
DAX measures calculate:
- total number of oncology trials
- trial counts by phase
- trial counts by intervention type
- distribution of termination reasons (`why_stopped`)

All measures are fully filter-aware.

---

## 6. Report Pages 

### Homepage
- Page navigation

### Phases
- Trial distribution by clinical phase
- Comparative phase analysis

### Interventions
- Breakdown of intervention types

### Top Biologics
- Focused analysis of biologic interventions in 2025

### Termination
- Key termination drivers

---

## 8. Assumptions & Limitations
- Oncology classification is keyword-based and may not capture all relevant trials
- `why_stopped` values are self-reported and not standardized
- Analysis is descriptive and does not imply causality

---

