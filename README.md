# airport
JKIA Aviation Recovery Analysis: Beyond the Pandemic
An end-to-end BigQuery & Looker Studio project analyzing the resilience and regionalization of East Africa's largest aviation hub.

✈️ Project Overview
This project analyzes passenger traffic at Jomo Kenyatta International Airport (JKIA) from June 2019 to August 2022. While the primary catalyst for change was the COVID-19 pandemic, this analysis digs deeper to uncover stories of Regional Concentration, Route Resilience, and Market Momentum.

🛠️ The Tech Stack
Data Warehouse: Google BigQuery (SQL)
Transformation: GoogleSQL (UNPIVOT, Window Functions, CTEs)
Visualization: Looker Studio
Version Control: GitHub
📈 Key Narratives & Findings
The Rise of the Regional Hub: While international long-haul routes lagged, intra-African routes (East & Central Africa) saw a significant increase in total market share, signaling JKIA's transition into a regional powerhouse.
Route Dominance vs. Diversification: Our analysis showed that the "Top 10" busiest routes now command a larger percentage of total traffic than pre-pandemic, suggesting airlines are consolidating around high-certainty hubs like Dubai and Addis Ababa.
Recovery Velocity: We engineered a custom metric to track how many months it took for specific cities to return to 2019 levels. Some routes showed "Elastic Demand" (recovering in <12 months), while others remained "effectively dead" (under 1% recovery).
Momentum Signals: Using 3-month rolling averages, we identified that as of Q3 2022, the airport's recovery was in an "Accelerating" phase, outperforming its own short-term growth trends.
💾 SQL Workflow
The analysis follows a 15-step rigorous workflow:

Steps 1-6: Data Engineering & Auditing. Unpivoting raw grid data and performing checksums to ensure destination totals match regional reporting.
Steps 7-10: Historical Benchmarking. Calculating recovery ratios and seasonal cycles.
Steps 11-15: Advanced Insights. Regional shift analysis, route concentration (Gini-style dominance), and MoM momentum status.
📊 Visualizations
Link to your Looker Studio Dashboard here
(Include screenshots of your Map, Seasonality Heatmap, and Momentum Gauges)
