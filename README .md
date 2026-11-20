# Garment Production Performance Dashboard (Power BI)

A Power BI dashboard that helps a garment factory manager answer four questions:

1. Are teams meeting their productivity targets? Which teams and departments miss most often?
2. How does productivity change over time (by week, day of the week and month)?
3. What is linked to lost productivity: idle time, WIP, overtime, style changes or team size?
4. Where should management focus improvement efforts?

**Tools:** Power BI Desktop, Power Query, DAX, Excel
**Data:** 1,197 daily team records from a garment factory (1 Jan – 11 Mar 2015)

---

## Dashboard pages

### 1. Overview
KPI cards, weekly actual vs target trend, productivity by day of week, and % of team-days meeting target by month.

![Overview](screenshots/Overview.png)

### 2. Team Performance
Productivity gap by team, % of target met by team, a team × department heatmap, and a department comparison.

![Team Performance](screenshots/02_team_performance.png)

### 3. Productivity Loss Drivers
Stoppages and style changes compared fairly (Sewing only), WIP and overtime-per-worker scatter charts with trend lines, and the 10 worst team-days.

![Loss Drivers](screenshots/03_loss_drivers.png)

### 4. Key Findings
A one-page summary of findings and recommendations for management.

![Key Findings](screenshots/04_key_findings.png)

A full PDF export is included: [Garment_Production_Dashboard.pdf](Garment_Production_Dashboard.pdf)

---

## What I did

**1. Data cleaning (Power Query)**
- Converted US-format dates correctly using the English (US) locale
- Fixed misspelled and inconsistent department names ("sweing", extra spaces)
- Kept 506 missing WIP values as blank, because they are all in Finishing, which does not track WIP
- Checked for duplicates (none), and reviewed outliers: kept productivity values above 1, corrected one target of 0.07 to 0.7 (likely typing error)
- Full details: [Data cleaning log](docs/data_cleaning_log.md)

**2. Data model**
- Built a separate date table (DimDate) with a Saturday-start week, because the factory works Saturday to Thursday
- One-to-many relationship from DimDate to the Production table
- Helper columns: productivity gap, met-target flag, team name, stoppage flag, overtime per worker

**3. DAX measures**
- 15 measures, including average productivity, productivity gap, % of team-days meeting target, idle events and overtime per worker
- Full list with code: [DAX measures](docs/dax_measures.md)

**4. Validation**
- Built a hidden "Measure Check" page and checked that totals match across pages (for example, 506 Finishing + 691 Sewing = 1,197 team-days)
- Found and fixed a missing visual filter by checking that group totals added up

---

## Key findings

1. Average productivity was **73.5%** against a **73.0%** target, but **27.0%** of team-days missed their target.
2. Finishing averaged **75.3%** but met target on only **59.7%** of days, compared with **82.8%** in Sewing. **8 of the 10** worst team-days were in Finishing.
3. Four Finishing teams met target on fewer than half of their days: **Team 06 (31.4%)**, **Team 08 (35.8%)**, **Team 07 (41.5%)** and **Team 11 (48.3%)**.
4. The share of team-days meeting target fell from **78.0%** in January to **65.9%** in February.
5. In Sewing, stoppage days averaged **49.2%** productivity vs **72.8%** without stoppage (18 stoppage days). Days with one style change averaged **61.5%**, about **4.7 points below** their already-reduced target.

## Recommendations

1. Focus on Finishing Teams 06, 07, 08 and 11: review daily results and check whether the 80% target is realistic.
2. Record a reason for every low-output day and every stoppage, so the right problem can be fixed first.
3. Plan style changes better: group similar styles and prepare before the change (SMED, a Lean quick-changeover method).

Full analysis: [Insights and recommendations](docs/insights.md)

## Limitations

- Data covers only 1 Jan – 11 Mar 2015, and March is incomplete.
- Some groups are small (18 stoppage days, 33 two-style-change days).
- Results show association, not cause.
- Team numbers repeat across departments, so teams were compared within each department.

---

## Repository contents

```
├── Garment_Production_Dashboard.pbix   Power BI file
├── Garment_Production_Dashboard.pdf    PDF export of all pages
├── garment_theme.json                  Custom colour theme
├── data/raw/                           Original dataset (unchanged)
├── docs/
│   ├── data_cleaning_log.md
│   ├── dax_measures.md
│   └── insights.md
└── screenshots/                        Images of each page
```

## How to open the dashboard

1. Install [Power BI Desktop](https://www.microsoft.com/power-bi/desktop) (free, Windows).
2. Download `Garment_Production_Dashboard.pbix` and open it.
3. If Power BI asks for the data file, point it to `data/raw/garments_worker_productivity.csv`.

No Windows? Open the PDF or view the screenshots above.

---

## Data source and licence

*Productivity Prediction of Garment Employees* (dataset 597), UCI Machine Learning Repository.
https://archive.ics.uci.edu/dataset/597/productivity+prediction+of+garment+employees

Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The dataset is used unchanged in `data/raw/`; all cleaning was done in Power Query.

---

**Author:** [Ansath Ahamed] – [GitHub](https://github.com/ansath005) · [LinkedIn](https://www.linkedin.com/in/ansath-ahamed-227775279)
