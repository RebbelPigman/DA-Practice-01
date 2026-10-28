# Insurance cost analysis

**Question:** What is associated with higher insurance charges, and how sure are we?

Public practice file, 1,338 people. Not an insurer project and not client work.

## Data

- File: `data/raw/insurance.csv` (do not edit)
- Excel workbook: `excel/insurance.xlsx` (Insured / Summary / Charts / Notes)
- Columns: age, sex, bmi, children, smoker, region, charges (+ `smoker_flag`, `bmi_class` helper columns)
- Source: [stedy/Machine-Learning-with-R-datasets](https://github.com/stedy/Machine-Learning-with-R-datasets) `insurance.csv`
- Book: Brett Lantz, *Machine Learning with R*
- Also listed at: https://www.kaggle.com/datasets/mirichoi0218/insurance

## How to rerun

Work lives in Excel Online on `excel/insurance.xlsx`. One-pager: `reports/p1_one_pager.md`.

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

## Findings

- **Smoking** shows the largest gap: smokers average about $32,050 vs about $8,434 for non-smokers.
- **Obesity (BMI ≥ 30 / obese class)** averages about $15,552 vs about $10,409 normal-weight.
- **Sex** differs only slightly (males a bit higher than females).
- **Goal Seek scenario:** smoker-only surcharge ≈ **20.2%** lifts total book charges 10%; non-smokers unchanged. Scenario only.
- **BMI what-if:** toy grid (BMI 20–40) shows charges rising with BMI; not a medical model.
- **Limit:** practice cross-section; no medical history; associations ≠ causation.
