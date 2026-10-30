# Insurance cost analysis

**Question:** What is associated with higher insurance charges, and how sure are we?

Public practice file, 1,338 people. Not an insurer project and not client work.

## Data

- File: `data/raw/insurance.csv` (do not edit)
- Excel workbook: `excel/insurance.xlsx` (Insured / Summary / Charts / Notes)
- Columns: age, sex, bmi, children, smoker, region, charges (+ `smoker_flag`, `bmi_class` helper columns in Excel)
- Source: [stedy/Machine-Learning-with-R-datasets](https://github.com/stedy/Machine-Learning-with-R-datasets) `insurance.csv`
- Book: Brett Lantz, *Machine Learning with R*
- Also listed at: https://www.kaggle.com/datasets/mirichoi0218/insurance

## How to rerun

- **P1 (Excel):** work lives in Excel Online on `excel/insurance.xlsx`. One-pager: `reports/p1_one_pager.md`.
- **P2 (Python):** open `notebooks/02_clean_eda.ipynb` in Jupyter and run Kernel → Restart & Run All. Reads `data/raw/insurance.csv` into a copy; the raw file is never overwritten.

## Progress

### Day 1 — Load and shape (on point)

- CSV loaded into Excel as Table **`Insured`** (1,338 rows — matches source).
- Helper columns: `smoker_flag`, `bmi_class`.
- Sheets set up: Insured / Summary / Charts / Notes.
- *(strat-A supermarket template adapted to insurance — no Sales/LineTotal/Month.)*

### Day 2 — Describe with Excel (on point)

- **Summary:** AVERAGE, MEDIAN, MIN, MAX, COUNT on age, bmi, children, charges, smoker_flag (median BMI ≈ 30.4; smoker share ≈ 20.5% → 274 smokers).
- **Charts:** average charges by sex, smoker vs non-smoker, and BMI class (normal / overweight / obese) with AVERAGEIF formulas.
- **Notes:** qualitative relations (smoker ≫ non-smoker; male slightly > female; obese > non-obese).

### Day 3 — What-if and write (done)

- Goal Seek / solved surcharge ≈ **20.2%** on smoker charges for a 10% lift in total book charges.
- Manual BMI what-if grid (toy slope) on Notes.
- Write-up: `reports/p1_one_pager.md`.

### Day 4 — P2 load and inspect notebook (done)

- New notebook `notebooks/02_clean_eda.ipynb`: loads raw CSV with pandas into a working copy.
- Inspected `shape`, `head`, `dtypes`, missing values, duplicates.
- **Shape:** 1,338 rows × 7 columns. **Missing values:** none.
- **Types:** age, children int; bmi, charges float; sex, smoker, region text (need encoding before modelling).
- **Duplicates:** 1 exact duplicate row (all 7 columns identical, charges to the cent — likely double entry). Plan: drop on Day 5, 1,338 → 1,337 rows, because it would shift means slightly and could leak across a train/test split.
- Reruns top to bottom on a cold kernel.

### Day 5 — P2 clean (done)

- Dropped the 1 exact duplicate row with `drop_duplicates()` and `reset_index(drop=True)`.
- **Shape:** 1,338 → 1,337 rows × 7 columns; duplicates now 0.
- Reason: a double-counted person would shift means slightly and could leak across a train/test split.

### Day 6 — P2 first EDA plots (done)

- **Histogram of charges:** right-skewed (long tail of high charges up to ≈ $63,770); mean ≈ $13,279 > median ≈ $9,386, so the median is the fairer "typical" charge.
- **Boxplot charges by smoker:** about 3/4 of non-smokers pay less than the cheapest smokers, so smoking is the strongest single driver.
- **Scatter bmi vs charges (coloured by smoker):** BMI barely moves non-smoker charges, but smokers with BMI over ~30 jump much higher, which is an interaction effect between smoking and BMI.

## Findings

- **Smoking** shows the largest gap: smokers average about $32,050 vs about $8,434 for non-smokers.
- **Obesity (BMI ≥ 30 / obese class)** averages about $15,552 vs about $10,409 normal-weight.
- **Sex** differs only slightly (males a bit higher than females).
- **Goal Seek scenario:** smoker-only surcharge ≈ **20.2%** lifts total book charges 10%; non-smokers unchanged. Scenario only.
- **BMI what-if:** toy grid (BMI 20–40) shows charges rising with BMI; not a medical model.
- **Data quality:** raw file is complete (no missing values) with 1 exact duplicate row.
- **Limit:** practice cross-section; no medical history; associations ≠ causation.
