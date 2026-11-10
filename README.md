# Insurance cost analysis

A personal data-analysis practice project on the public Kaggle / *Machine Learning with R* insurance dataset (1,338 people). Not client work and not an insurer project.

## Question

What is associated with higher insurance charges, and how sure are we?

## Data

- `data/raw/insurance.csv` — the raw file, never edited. Columns: age, sex, bmi, children, smoker, region, charges.
- `excel/insurance_day_3.xlsx` — Excel work (sheets Insured / Summary / Charts / Notes). The `Insured` table adds two **Excel-only** helper columns that are not in the CSV: `smoker_flag` (1 = smoker) and `bmi_class` (underweight < 18.5, normal < 25, overweight < 30, obese ≥ 30).
- Source: [stedy/Machine-Learning-with-R-datasets](https://github.com/stedy/Machine-Learning-with-R-datasets) `insurance.csv`; book: Brett Lantz, *Machine Learning with R*; also listed at https://www.kaggle.com/datasets/mirichoi0218/insurance

## How to rerun

- **Excel:** open `excel/insurance_day_3.xlsx` and read `reports/p1_one_pager.md`.
- **Python:** `pip install -r requirements.txt`, then open `notebooks/02_clean_eda.ipynb` in Jupyter and run Kernel → Restart & Run All. The notebook reads `data/raw/insurance.csv` into a working copy; the raw file is never overwritten, and the cleaned data exists only in the notebook's memory (no clean file is written).

## Findings

- **Rows:** 1,338 raw → 1,337 after dropping 1 exact duplicate row (no missing values).
- **Smoking:** smokers average about $32,050 vs about $8,441 for non-smokers (notebook, 1,337 rows); the cheapest smoker (≈ $12,829) pays more than three-quarters of non-smokers (Q3 ≈ $11,363).
- **Skew:** charges are right-skewed — mean ≈ $13,279 vs median ≈ $9,386, so the median is the fairer "typical" charge.
- **Observational, not causal:** these are associations in a practice cross-section, not proof that smoking or BMI causes higher charges.

## File map

```
data/raw/insurance.csv          raw data (do not edit)
excel/insurance_day_3.xlsx      Excel workbook: Insured / Summary / Charts / Notes
excel/insurance_day_2.xlsx      earlier Excel version
notebooks/02_clean_eda.ipynb    Python load, clean (drop duplicate) and EDA plots (matplotlib)
reports/p1_one_pager.md         Excel one-page write-up
requirements.txt                pinned Python packages
```

## Limits

- Practice cross-section: no medical history, no follow-up over time.
- Associations only, not causation; not pricing advice.
- The Excel workbook uses all 1,338 rows (duplicate kept), while the notebook uses 1,337, so averages differ slightly between them (e.g. non-smoker average $8,434 in Excel vs $8,441 in the notebook).
- The Goal Seek surcharge and BMI what-if grid in Excel are scenarios / toy formulas, not models.

## Progress (tutor log)

### Day 1 — Load and shape (on point)

- CSV loaded into Excel as Table **`Insured`** (1,338 rows — matches source).
- Helper columns (Excel only): `smoker_flag`, `bmi_class`.
- Sheets set up: Insured / Summary / Charts / Notes.

### Day 2 — Describe with Excel (on point)

- **Summary:** AVERAGE, MEDIAN, MIN, MAX, COUNT on age, bmi, children, charges, smoker_flag (median BMI ≈ 30.4; smoker share ≈ 20.5% → 274 smokers).
- **Charts:** average charges by sex, smoker vs non-smoker, and BMI class (normal / overweight / obese) with AVERAGEIF formulas (all 1,338 rows): smokers ≈ $32,050 vs non-smokers ≈ $8,434; obese ≈ $15,552 vs normal-weight ≈ $10,409; males ≈ $13,957 vs females ≈ $12,570.
- **Notes:** qualitative relations (smoker ≫ non-smoker; male slightly > female; obese > non-obese).

### Day 3 — What-if and write (done)

- Goal Seek / solved surcharge ≈ **20.2%** on smoker charges for a 10% lift in total book charges (non-smokers unchanged; scenario only).
- Manual BMI what-if grid (toy slope, BMI 20–40) on Notes; not a medical model.
- Write-up: `reports/p1_one_pager.md`.

### Day 4 — P2 load and inspect notebook (done)

- New notebook `notebooks/02_clean_eda.ipynb`: loads raw CSV with pandas into a working copy.
- Inspected `shape`, `head`, `dtypes`, missing values, duplicates.
- **Shape:** 1,338 rows × 7 columns. **Missing values:** none.
- **Types:** age, children int; bmi, charges float; sex, smoker, region text (need encoding before modelling).
- **Duplicates:** 1 exact duplicate row (all 7 columns identical, charges to the cent — likely double entry). Plan: drop on Day 5, 1,338 → 1,337 rows, because it would shift means slightly and could leak across a train/test split.
- Reruns top to bottom on a cold kernel.

### Day 5 — P2 clean (done)

- Dropped the 1 exact duplicate row in the notebook's working copy with `drop_duplicates()` and `reset_index(drop=True)` (no clean file is saved).
- **Shape:** 1,338 → 1,337 rows × 7 columns; duplicates now 0.
- Reason: a double-counted person would shift means slightly and could leak across a train/test split.

### Day 6 — P2 first EDA plots (done, matplotlib)

- **Histogram of charges:** right-skewed (long tail of high charges up to ≈ $63,770); mean ≈ $13,279 > median ≈ $9,386, so the median is the fairer "typical" charge.
- **Boxplot charges by smoker:** about 3/4 of non-smokers pay less than the cheapest smokers, so smoking is the strongest single driver.
- **Scatter bmi vs charges (coloured by smoker):** BMI barely moves non-smoker charges, but smokers with BMI over ~30 jump much higher, which is an interaction effect between smoking and BMI.

### Day 7 — What the chart is saying (done)

- Supervisor note in `notebooks/02_clean_eda.ipynb` (Day 7 markdown): right-skew (mean ≈ $13,279 vs median ≈ $9,386); smoker min ≈ $12,829 sits above non-smoker Q3 ≈ $11,363; smoker × BMI ≥ 30 mean ≈ $41,558 vs non-smoker ≈ $8,441.
- Pinned env: `requirements.txt` (pandas 3.0.4, numpy 2.5.2, matplotlib 3.11.1, jupyterlab 4.6.3). Seaborn is not used for any chart, so it is not pinned.
