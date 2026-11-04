# Strat-A — CVA project ladder

Use this to make `CVA.md` true. One through-line, six projects. Each project keeps every skill from the one before it and adds a new layer. Start at P1. Do not skip to the capstone.

Rule from `CVA.md`: only put a line on the CV if you can open the file and talk through it. Mark the matching row in `skills.md` `[x]` the day the project is done, not the day you read this file.

Synced: 22 Sep 2026

---

## What “value” means here

Analytics intern ads in SA (see `job options.md`) filter on three things you can control:

1. A public notebook or SQL file a recruiter can open in two minutes.
2. Skills that match the advert word-for-word (Excel, SQL, pandas, charts, a plain-language finding).
3. Numbers — row counts, R², “before vs after the join” — not module titles.

This ladder is built only from the **CVA hard-skill block**. It does not add deep learning, Spark, Hadoop, or production Power BI until you can demo them. Power BI is an optional last week because junior ads ask for it constantly; it is not required to finish Strat-A.

---

## CVA hard skills (closed set)

These are the skills the last project must use. Nothing else is in scope.

| Group | Skills |
|---|---|
| Analysis | EDA · descriptive statistics · correlation · probability on the data · hypothesis testing · simple linear regression · multiple regression · R² |
| Python | Python 3 · Jupyter · pandas · NumPy · Matplotlib · Seaborn · scikit-learn (intro) |
| Stores | Excel (formulas, tables, charts, Goal Seek, what-if) · SQL SELECT, JOINs, subqueries, views · relational modelling and normalisation · Access queries |
| Delivery | Git / GitHub · written reporting · AI ethics and responsible use |

Soft skills that fall out of the work (do not build a separate project for them): problem solving, requirements translation, technical documentation.

---

## One dataset, six layers

**Question you will keep asking:** *What is associated with higher line-item sales in this retailer, and how sure are we?*

**Dataset:** Kaggle “Supermarket sales” (Aung Pyae) — about 1,000 invoice lines, 3 branches, product line, unit price, quantity, date, payment, rating. Free, small, clean enough to start, messy enough to clean.

Download once. Put the raw CSV in `data/raw/` and never edit that copy. Every later project reads from the same files.

Honest framing on GitHub and on the CV: “practice retail dataset, public.” Do not imply you worked for the chain.

If the download is blocked, use any similarly sized tabular retail file with at least date, category, branch/store, quantity, and a money column. Do not switch datasets mid-ladder.

You will add two lookup tables yourself in P4 (store / branch attributes, product-line attributes) so the SQL work is a real schema, not one flat file.

---

## Repo

One GitHub repo for the whole ladder, not six.

```
retail-sales-analysis/
  README.md                 # question, data source, how to rerun, findings
  data/raw/                 # original csv only
  data/interim/             # cleaned tables
  data/warehouse/           # sqlite file from P4
  excel/                    # P1 workbook
  access/                   # P4 .accdb if you do the Access day
  notebooks/
    02_clean_eda.ipynb
    03_describe_correlate.ipynb
    05_model_infer.ipynb
  sql/
    00_schema.sql
    01_load_notes.md
    02_analysis.sql
  reports/
    p1_one_pager.md
    p5_findings.md
    p6_analyst_brief.md
    ethics.md
  requirements.txt
```

First commit is the empty tree + README that states the question. Then commit at the end of every day listed below.

---

## How to use a day

Each day is one sitting: **60–90 minutes**. Stop when the day’s “done when” line is true. If you miss a weekday, use one weekend block to catch one day — do not batch three days. The point is a trail of commits, not a Sunday marathon.

Tools you already have from modules: Excel / Office, Anaconda, Jupyter, Python 3, pandas, Matplotlib, Seaborn. Add SQLite (comes with Python) for P4. Access is the ITSMA tool — one day only. scikit-learn in P5.

Do **not** start a second stats project. P3 and P5 are allowed to be the 234-ITSPA project if the lecturer’s dataset can carry the same methods. If ITSPA forces a different table, keep this retail repo as the public portfolio and treat ITSPA as the assessed twin. Same methods, two write-ups.

---

## Ladder at a glance

| Project | New skills | Reuses | Days | CVA bullet it fills |
|---|---|---|---|---|
| P1 Excel snapshot | Excel tables, formulas, charts, Goal Seek / what-if, short write-up | — | 3 | Excel analysis line |
| P2 Clean-and-see | Python, Jupyter, pandas, NumPy, Matplotlib, Seaborn, cleaning, EDA, Git | write-up | 4 | Exploratory notebook |
| P3 Describe and correlate | descriptive stats, correlation, probability on the sample | P2 | 3 | stats / EDA depth |
| P4 SQL workbook | modelling, normalisation, SELECT, JOINs, subquery, view, Access | P1–P3 tables | 4 | SQL analysis workbook |
| P5 Model and test | simple + multiple regression, R², hypothesis tests, scikit-learn intro | P2–P4 | 5 | ITSPA / modelling line |
| P6 Analyst brief | ethics note, recruiter README, all skills in one pass | all of the above | 4 | profile + project swap |

Total: **23 evenings**. At five evenings a week that is about five weeks. Fit P5 around the ITSPA submission date.

---

## P1 — Excel snapshot

**Outcome:** a workbook a manager could open. No Python yet.

**New:** Excel formulas, tables, charts, Goal Seek, what-if, half-page finding.

### Day 1 — Load and shape
- Import the raw CSV into Excel. Convert the used range to a Table. Name it `Sales`.
- Check row count against the source. Add a `LineTotal` column (`Unit price * Quantity`, or use the file’s tax-inclusive total if that is what you will model later — pick one and stay with it).
- Freeze the header. Add a `Month` column from the date.
- **Done when:** Table has a name, a calculated column, and the row count written in a `Notes` sheet.

### Day 2 — Describe with Excel
- Pivot: total sales and mean rating by branch, by product line, by payment type.
- Two charts: sales by product line; sales by month (or week if the date range is short).
- `AVERAGE`, `MEDIAN`, `MIN`, `MAX`, `COUNT` on `LineTotal` and `Quantity` in a `Summary` sheet.
- **Done when:** Summary sheet has those five numbers and both charts sit on a `Charts` sheet with titles that state the claim (“Branch C is not the sales leader”).

### Day 3 — What-if and write
- Goal Seek: what unit-price change on one product line would lift that line’s total by 10%. Write the before/after on `Notes`.
- One what-if table: quantity 1–10 vs line total at the median unit price.
- Write `reports/p1_one_pager.md`: question, source, three numbers, one chart claim, one limitation (sample is small / one quarter / three branches).
- **Done when:** the one-pager is 150–250 words and you can explain Goal Seek without the ribbon.

**CVA paste (after P1):**
> Excel sales snapshot — built a table, pivots, and charts on a 1,000-line retail file; used Goal Seek to test a 10% lift on one product line.

---

## P2 — Clean-and-see notebook

**Outcome:** a Jupyter notebook that replaces the Excel glance with a repeatable clean.

**New:** Python 3, Jupyter, pandas, NumPy, Matplotlib, Seaborn, cleaning, EDA, Git.

### Day 4 — Repo and load
- Create the GitHub repo. First commit: README with the question and data citation.
- `notebooks/02_clean_eda.ipynb`: `read_csv` from `data/raw/`. `shape`, `dtypes`, `head`, missing-value count.
- Save a copy of the frame. Do not overwrite raw.
- **Done when:** the notebook runs top to bottom on a cold kernel and GitHub shows the commit.

### Day 5 — Clean
- Dates to datetime. Strip column names. Flag missing and duplicate invoice IDs.
- Decide and document each fix in a markdown cell (“dropped N duplicate invoices”; “left rating missing as missing”).
- Write `data/interim/sales_clean.csv`.
- Print row count before and after. That number goes in the README later.
- **Done when:** interim CSV exists and the notebook states every rule you applied.

### Day 6 — Plots
- Histogram of line total. Box plot of line total by branch. Bar of mean quantity by product line. Correlation heatmap of the numeric columns (preview for P3).
- One Seaborn plot and one Matplotlib plot — both, so you can name both in an interview.
- Each figure has a title that is a sentence, not a column name.
- **Done when:** four figures render on a cold rerun.

### Day 7 — What the chart is saying
- Half-page markdown cell: what you would tell a supervisor. No jargon without a number next to it.
- `requirements.txt` with pinned pandas, numpy, matplotlib, seaborn, jupyter.
- Push. Paste the repo URL into `CVA.md` under the exploratory notebook heading.
- **Done when:** a classmate can clone, install, and rerun Day 3 without asking you.

**CVA paste (after P2):**
> Exploratory notebook — loaded and cleaned a 1,000-line retail file in pandas; plotted distributions and a correlation heatmap; wrote a short “what the chart is saying” note.

---

## P3 — Describe and correlate

**Outcome:** the same table, now with statistics you can defend. Maps to 234-ITSPA LO1 (describe, histogram, quartiles, correlation).

**New:** descriptive statistics, correlation, probability language on *this* sample (not a textbook coin-flip).

### Day 7 — Describe
- Quartiles, IQR, mean vs median for line total and unit price. Say whether the money column is skewed.
- Histogram + box plot you already have: add a one-sentence read of shape (skew, outliers).
- Optional stem-and-leaf on a 50-row sample if ITSPA still wants that picture.
- **Done when:** a `Summary` table in the notebook has n, mean, median, Q1, Q3, IQR, min, max.

### Day 9 — Correlation
- Pearson correlation: unit price vs quantity, quantity vs line total, rating vs line total.
- Scatter + fitted line for the pair you will later regress (start with quantity vs line total *only if* it is not a tautology — if line total is price × qty, do **not** regress those two; use rating vs line total, or unit price vs quantity, or a date-derived volume vs rating).
- Write why you rejected any pair that is just an identity.
- **Done when:** you can say “r = …, so …” in one sentence without opening the cell.

### Day 10 — Probability on the sample
- Treat line total as the empirical distribution. Estimate P(line total > sample 75th percentile) from the data and from a normal approximation. Say which you trust and why (CLT note: n is large; the money column may still be skewed).
- One binomial picture if it fits: e.g. payment-type share as p, then P(at least k card payments in a sample of n). Keep it short.
- Push. Update `reports/p1_one_pager.md` or start `reports/p3_notes.md` with the three statistics you would quote in an interview.
- **Done when:** you have one empirical probability and one correlation, each with a caveat.

**CVA paste (after P3):**
> Descriptive analysis — quartiles and correlation on the cleaned retail table; reported r and a plain-language caveat on skew.

---

## P4 — SQL workbook

**Outcome:** the flat file becomes a small schema. This is the SQL bullet `CVA.md` is waiting for.

**New:** relational modelling, normalisation, SELECT, JOINs, subqueries, views, Access queries.

### Day 11 — Model
- Draw three tables on paper or in README: `branch` (branch id, city if present, extra attribute you add), `product_line` (id, name), `sale` (invoice id, date, branch id, product_line id, unit price, quantity, rating, payment, line total).
- Normalise: no repeating product-line strings in `sale` after this day. Keys and a one-line integrity note (a sale must belong to one branch).
- **Done when:** the README has the three-table sketch and primary / foreign keys.

### Day 12 — Build
- `sql/00_schema.sql`: CREATE TABLE statements, types, keys.
- Load from `data/interim/sales_clean.csv` into SQLite (`data/warehouse/retail.sqlite`) with a short Python cell or the sqlite3 CLI. Document the load in `sql/01_load_notes.md`.
- Check counts: `sale` rows vs the interim CSV. Write both numbers.
- **Done when:** the database file is in git-lfs *or* you publish only the `.sql` and a note that the db is rebuilt from interim CSV (prefer rebuildable over a binary).

### Day 13 — Query
- `sql/02_analysis.sql` must contain, in this order:
  1. SELECT with a WHERE and a GROUP BY (sales by branch).
  2. JOIN `sale` to `branch` and to `product_line`.
  3. A subquery (branches whose total sales exceed the average branch total).
  4. A VIEW `v_branch_month` (branch, month, sales, n_invoices).
- After each statement, comment the row count you got.
- **Done when:** all four run against a fresh load.

### Day 14 — Access twin
- Recreate the three tables in Access (ITSMA evidence). One multi-table query and one totals query that match two of the SQL statements.
- Screenshot or export the query design to `access/` and write three sentences on how the Access query maps to the SQL JOIN.
- If Access is not installed on this machine, write the query plan on paper and skip the file — do not fake a screenshot. SQL is the portfolio copy; Access is the module copy.
- **Done when:** you can explain JOIN vs subquery without opening the script.

**CVA paste (after P4):**
> SQL analysis workbook — modelled branch / product / sale, loaded SQLite, wrote joins, a subquery, and a view; checked row counts before and after joins.

---

## P5 — Model and test

**Outcome:** a question, a model, a test, a number, a limit. This is the ITSPA project shape (Q1 already done; Q2–Q7 live here if the lecturer allows the same methods).

**New:** simple linear regression, multiple regression, R², hypothesis testing, scikit-learn intro.

Use statsmodels or scikit-learn for the fit. You need enough of both to say “I fitted it in X; I read coefficients from Y.”

### Day 15 — Question and design
- Write the question at the top of `notebooks/05_model_infer.ipynb`. Example: “Is customer rating associated with line total after accounting for branch and product line?”
- Name the response, the predictors, and what would falsify the claim.
- Train/test split *or* a simple hold-out month if you use scikit-learn. For ITSPA, follow the lecturer’s method if it conflicts — document the conflict in one sentence.
- **Done when:** the question is one sentence and every column in the model is listed.

### Day 16 — Simple regression
- One predictor. Plot scatter + line. Report slope, intercept, R², and residual vs fitted in two sentences (“the line explains … of the variance; residuals fan out / do not”).
- If ITSPA Q1 already did this on another table, still run it here so the public repo stands alone.
- **Done when:** you can read the slope in the units of the columns (“one rating point is associated with R… on the line total”).

### Day 17 — Multiple regression
- Add branch and/or product line (dummy variables) plus one numeric predictor.
- Report each coefficient, R² vs the simple model, and whether the added variables were worth it in plain language.
- Feature note: you selected these columns because of the P3 correlations and the P4 schema — write that. That is the “feature selection” you are allowed to claim at intro level.
- **Done when:** a coefficient table sits next to a two-sentence comparison of the two models.

### Day 18 — Inference
- Hypothesis test on the slope you care about (H0: no association). Report the p-value, α you chose (0.05 unless ITSPA says otherwise), and the decision in words.
- One confidence interval for a mean or for a coefficient — whichever ITSPA is on that week.
- Type I / Type II in one paragraph tied to this decision (“if I am wrong, the store prints a promotion that does nothing”).
- **Done when:** the notebook has H0, Ha, α, p, decision, and a business cost of being wrong.

### Day 19 — Write the finding
- `reports/p5_findings.md`: question, method, three numbers (n, R², p), conclusion a non-technical reader can use, two limits (sample, identity columns you refused to model, missing confounders).
- If this *is* the ITSPA submission, copy the required headings from the brief on top of this structure. Do not fight the rubric.
- Push. Swap the first project bullet on `CVA.md`.
- **Done when:** you can talk for three minutes with the notebook closed.

**CVA paste (after P5):**
> Retail sales model — cleaned the table in pandas, fitted simple and multiple regression, reported coefficients, R² and a hypothesis test, and wrote a plain-language conclusion.

---

## P6 — Analyst brief (all skills)

**Outcome:** one folder a hiring manager can skim. Every skill in the closed set appears at least once.

**New as a deliverable:** ethics / responsible-use note, recruiter-grade README. No new library required.

### Day 20 — Ethics pass
- `reports/ethics.md` (one page): whose behaviour is in the file (customers at a practice retailer), what you will not claim (no individual scoring, no “this branch should fire cashiers”), how you handled rating and invoice IDs, what would be different if this were live company data (consent, retention, who sees the dashboard).
- Tie one sentence to the AI-ethics module: a model that looks precise can still be used to punish a branch on noise.
- **Done when:** the note has data source, risk, and a rule you actually followed in P2–P5.

### Day 21 — Rerun pack
- Walk the path: raw CSV → P2 clean → P4 schema load → P5 notebook.
- Fix anything that only works on your machine (hard-coded paths, missing seed, chart that needs a prior cell).
- README sections: Question · Data · How to rerun · Findings in four bullets · File map · Limits.
- **Done when:** a cold clone + `pip install -r requirements.txt` reruns P2 and P5.

### Day 23 — Brief
- `reports/p6_analyst_brief.md` (one page): audience = intern supervisor. What we asked, what we did (Excel → clean → SQL → model), what we found, what we would do next week (more stores, a proper hold-out, a dashboard).
- Screenshot two charts into the brief or link the notebook cells.
- **Done when:** the brief is ≤ 400 words and contains at least one Excel number, one SQL row count, and one model number.

### Day 24 — CV and skills
- Update `CVA.md` project block with real links and the three pastes above. Delete any project line you still cannot defend.
- Mark the matching CVA skills `[x]` in `skills.md`. Leave Power BI, deep learning, Spark untouched.
- Pin the repo on GitHub. File name for applications stays `Rebbel-Pigman-Data-Analyst-Intern.pdf`.
- **Done when:** the CVA project section has dates, tools, and a URL.

**CVA paste (after P6, first project bullet — swap to match the advert):**
> End-to-end retail analysis — Excel snapshot, pandas clean, SQLite schema with joins and a view, multiple regression with a reported R² and p-value, and a one-page brief for a non-technical reader.

---

## Optional week — Power BI (not in the closed set)

Only after P6. Junior SA ads ask for it; `CVA.md` says do not list it until you can open the file.

- Day A: load `v_branch_month` (export CSV from the view) into Power BI Desktop.
- Day B: one page — sales by branch, sales by product line, a date slicer, one card for n and one for total sales.
- Day C: publish to a free workspace *or* keep the `.pbix` and a 60-second screen recording. Add one line to CVA only after you can rebuild the page from the CSV.

Tableau Public is an acceptable stand-in if Power BI is not on the machine.

---

## After each project — 15-minute close

1. Commit message in the form `p3: correlation and skew note`.
2. One line in the README findings list.
3. Tick the skills in `skills.md`.
4. Say the finding out loud. If you need the notebook open to remember the number, the write-up is not done.

---

## Interview pocket card

Keep four numbers in your head before any analytics intern call:

| Number | Where it lives |
|---|---|
| Row count raw → clean | P2 |
| A join count that still makes sense | P4 |
| r or R² for the model you will discuss | P3 / P5 |
| p-value and the decision in words | P5 |

If they ask “what would you do next,” the honest answer is: more branches, a time hold-out, and a dashboard — which is the optional week.

---

## Do not build

- A second unrelated Kaggle dump “for the portfolio.” One thread beats five orphans.
- Neural nets, k-means, genetic algorithms, Spark. They are in `skills.md` as later modules. They weaken a Y2 analyst CV.
- A live scraper, an app, or an AWS pipeline. That is Strat-B / CVB territory.
- Fake company names or invented metrics.

---

## Order if time is short

ITSPA deadline first.

1. P5 methods on the *assessed* dataset so Q2–Q7 ship.
2. P2 + P4 on the retail file so the public repo has a notebook and a `.sql` file.
3. P1 if Excel is named in the advert you are sending this week.
4. P6 before the next application batch.
5. P3 can fold into P2 if you are down to ten days — do not fold P4 or P5.

Against a 200 : 1 pile (`job options.md`), the filter is: applied early, one attached project, words that match the advert, four lines on why that team. This ladder is the attached project.
