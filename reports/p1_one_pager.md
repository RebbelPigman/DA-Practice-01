# P1 one-pager — insurance charges

**Question:** What is associated with higher insurance charges, and how sure are we?

**Source:** Brett Lantz, *Machine Learning with R* insurance file (also on Kaggle), `insurance.csv`, n = 1,338 people. Public practice data — not an insurer project.

**Three numbers (from the Excel Summary / analysis):**
- Smokers average about **$32,050** in charges vs about **$8,434** for non-smokers.
- People with BMI ≥ 30 (obese) average about **$15,552** vs about **$10,409** for normal-weight (BMI < 25).
- Males average slightly more than females (~$13,957 vs ~$12,570).

**Chart claim:** Charts that split average charges by smoker status show the largest gap; smoking dominates sex and region for this file.

**Limit:** This is a practice cross-section with no medical history, no follow-up over time, and no claim that these associations cause charges. Results are for learning Excel / analysis, not pricing advice.

**Goal Seek scenario:** To lift total book charges by 10%, a smoker-only surcharge of about **20.2%** makes `new_total` match the 10% target while non-smoker charges stay unchanged. Scenario only.

**BMI what-if:** A toy grid (BMI 20–40, simple linear charge formula) shows charges rising with BMI; it is a stand-in slope, not a real medical model.
