# airport
# ✈️ JKIA Aviation Recovery Analysis: Beyond the Pandemic

An end-to-end BigQuery analysis of how Jomo Kenyatta International Airport — East Africa's largest aviation hub — recovered from COVID-19, and how its route network changed shape in the process.

**[→ View the live dashboard on Tableau Public](#)**

---

## Overview

Most "COVID recovery" analyses stop at one question: did traffic come back? This one goes further — using monthly passenger data across 41 international routes (June 2019–August 2022), it asks *which* routes came back, *how fast*, and *what changed permanently* about the airport's network in the process.

The short version: JKIA didn't just recover. It regionalized.

## Key Findings

| Metric | Result |
|---|---|
| Overall traffic recovery | **140%** of pre-COVID (July 2019) peak by mid-2022 |
| Routes back to 2019 levels | **30 of 41** |
| Top-5 route dominance | **48.3% → 38.3%** (traffic diversified, didn't concentrate) |
| Regional share shift | Europe **+2.3pp**, Middle East **−5.4pp** |
| Never recovered | Zurich (0.00 recovery ratio) |

**The story behind the numbers:** intra-African and Gulf-hub routes — Addis Ababa, Dubai — gained share as airlines consolidated around high-certainty regional connections, while several long-haul European and Middle Eastern routes never returned to pre-pandemic volumes. JKIA's post-COVID network looks meaningfully more regional than it did in 2019.

## Tech Stack

- **Warehouse:** Google BigQuery
- **Transformation:** GoogleSQL — `UNPIVOT`, window functions, CTEs
- **Visualization:** Tableau
- **Version control:** GitHub

## SQL Workflow

A 15-step pipeline, in three phases:

1. **Data engineering & auditing (steps 1–6)** — unpivoted raw grid-format data into a long table, then checksum-validated destination totals against regional reporting to catch transformation errors before any analysis ran on top of them.
2. **Historical benchmarking (steps 7–10)** — calculated recovery ratios against the July 2019 baseline and mapped seasonal traffic cycles.
3. **Advanced insights (steps 11–15)** — regional share-shift analysis, route concentration (Gini-style dominance scoring), and month-over-month momentum classification.

## Dashboard

![JKIA passenger traffic recovery, 2019–2022](images/jkia-recovery-status.png)
*Monthly passenger traffic, recovery status, and route-level recovery ratios.*

![Route volatility and regional share shift](images/jkia-route-volatility.png)
*Route volatility, post-COVID recovery momentum, and regional share shift.*

## What I'd Extend Next

- Bring in airline-level data to see whether regionalization is a network effect or a handful of carriers repositioning
- Add cargo traffic as a comparison — passenger and freight recovery often diverge
- Automate the monthly refresh so the Tableau dashboard updates itself instead of a manual pull

---

**Data source:** [add source + link]
**Contact:** Catherine Waweru — [kathyweru85@gmail.com](mailto:kathyweru85@gmail.com)
