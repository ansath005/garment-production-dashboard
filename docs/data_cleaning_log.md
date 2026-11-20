# Data Cleaning Log – Garment Production Performance Dashboard

Dataset: UCI Machine Learning Repository, "Productivity Prediction of Garment Employees" (dataset 597, CC BY 4.0)
Original rows: 1,197 | Final rows: 1,197 | Final columns: 17

All cleaning was done in Power Query. The original CSV file was never edited.

| # | Column | Issue found | Rows affected | Action taken | Reason |
|---|---|---|---|---|---|
| 1 | date | Text in US format (M/D/YYYY) | All 1,197 | Changed to Date using English (US) locale | Correct dates for trend analysis |
| 2 | department | Misspelled "sweing", extra spaces, lowercase | 691 | Trimmed, cleaned, capitalized, replaced "Sweing" with "Sewing" | One clean name per department, so charts are not split |
| 3 | wip | Missing values, all in Finishing department | 506 | Kept as null (empty) | Finishing does not track WIP; replacing with 0 or the average would create false data |
| 4 | All columns | Checked for duplicate rows | 0 | No action needed | Duplicates would be counted twice and distort averages |
| 5 | actual_productivity | Values above 1 (max 1.12) | 37 | Kept; added `above_standard_flag` column | Teams can work faster than the standard time; values are realistic |
| 6 | targeted_productivity | Target of 0.07 (all others 0.6 to 0.8) | 1 | Replaced 0.07 with 0.7 | Likely typing error; documented as an assumption |
| 7 | no_of_workers | Decimal values (e.g. 30.5, 57.5) | – | Kept | Likely part-shift workers; does not affect the analysis |
| 8 | idle_time | Mostly zero; max 300 minutes | 18 non-zero | Kept | Real stoppage events; small sample noted for later analysis |
| 9 | incentive | 604 zeros; a few very large values (max 3,600 BDT) | Few | Kept | Real bonus payments; averages may be skewed, so the median is also reported |
| 10 | record_id (new) | Added a unique row number | All 1,197 | Index column starting from 1 | Trace exact records in tables |
