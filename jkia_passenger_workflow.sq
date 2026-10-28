-- ============================================================
-- JKIA Passengers by Destination — full BigQuery workflow
-- Project: passenger-data-jkia
-- Dataset:  raw_passenger_data
-- Source table: raw_grid_generation  (the wide table you loaded)
--
-- HOW TO USE THIS FILE:
-- Paste ONE numbered block at a time into a BigQuery SQL Query tab
-- and click Run. Wait for "Query completed" before running the next
-- block. Some blocks create tables, others just return results
-- for you to read.
-- ============================================================


-- ============================================================
-- STEP 1 — Unpivot the wide table into long format, filling the
-- two known blank cells (Cape Town Jun 2019, Other North America
-- Jun 2019) with 0, and tagging every row with row_type and region.
-- Creates a new table: passengers_long
-- Run this once. Re-run any time you reload raw_grid_generation.
-- ============================================================
CREATE OR REPLACE TABLE `passenger-data-jkia.raw_passenger_data.passengers_long` AS
SELECT
  Port AS port,
  month_label,
  passengers,
  PARSE_DATE('%b %Y', month_label) AS month_date,
  CASE
    WHEN Port LIKE 'Total%' AND Port = 'Total International' THEN 'grand_total'
    WHEN Port LIKE 'Total%' THEN 'region_total'
    ELSE 'destination'
  END AS row_type,
  CASE
    WHEN Port IN ('Cairo','Khartoum','Juba','Other North Africa','Total North Africa')
      THEN 'North Africa'
    WHEN Port IN ('Johannesburg','Harare','Lilongwe','Lusaka','Cape Town',
                  'Other South Africa','Total Southern Africa')
      THEN 'Southern Africa'
    WHEN Port IN ('Amsterdam','Brussels','London','Paris','Other Europe','Zurich','Total Europe')
      THEN 'Europe'
    WHEN Port IN ('New York','Other North America','Total North America')
      THEN 'North America'
    WHEN Port IN ('Dubai','Jeddah','Other Middle East','Total Middle East')
      THEN 'Middle East'
    WHEN Port IN ('Mumbai','Bangkok','Other Far East','Total Far East')
      THEN 'Far East'
    WHEN Port IN ('Addis Ababa','Bujumbura','Dar-es-alaam','Entebbe','Kigali','Kinshasa',
                  'Other East & Central Africa','Total East & Central Africa')
      THEN 'East & Central Africa'
    WHEN Port IN ('Zanzibar','Other Indian Ocean Islands','Mauritius','Seychelles',
                  'Total Indian Ocean Islands')
      THEN 'Indian Ocean Islands'
    WHEN Port IN ('Lagos','Abidjan','Douala','Accra','Other West Africa','Total West Africa')
      THEN 'West Africa'
    WHEN Port = 'Other International' THEN 'Ungrouped International'
    WHEN Port = 'Total International' THEN 'International'
    ELSE 'Unknown'
  END AS region
FROM (
  SELECT
    Port,
    COALESCE(`Jun 2019`, 0) AS `Jun 2019`,  COALESCE(`Jul 2019`, 0) AS `Jul 2019`,
    COALESCE(`Aug 2019`, 0) AS `Aug 2019`,  COALESCE(`Sep 2019`, 0) AS `Sep 2019`,
    COALESCE(`Oct 2019`, 0) AS `Oct 2019`,  COALESCE(`Nov 2019`, 0) AS `Nov 2019`,
    COALESCE(`Dec 2019`, 0) AS `Dec 2019`,  COALESCE(`Jan 2020`, 0) AS `Jan 2020`,
    COALESCE(`Feb 2020`, 0) AS `Feb 2020`,  COALESCE(`Mar 2020`, 0) AS `Mar 2020`,
    COALESCE(`Apr 2020`, 0) AS `Apr 2020`,  COALESCE(`May 2020`, 0) AS `May 2020`,
    COALESCE(`Jun 2020`, 0) AS `Jun 2020`,  COALESCE(`Jul 2020`, 0) AS `Jul 2020`,
    COALESCE(`Aug 2020`, 0) AS `Aug 2020`,  COALESCE(`Sep 2020`, 0) AS `Sep 2020`,
    COALESCE(`Oct 2020`, 0) AS `Oct 2020`,  COALESCE(`Nov 2020`, 0) AS `Nov 2020`,
    COALESCE(`Dec 2020`, 0) AS `Dec 2020`,  COALESCE(`Jan 2021`, 0) AS `Jan 2021`,
    COALESCE(`Feb 2021`, 0) AS `Feb 2021`,  COALESCE(`Mar 2021`, 0) AS `Mar 2021`,
    COALESCE(`Apr 2021`, 0) AS `Apr 2021`,  COALESCE(`May 2021`, 0) AS `May 2021`,
    COALESCE(`Jun 2021`, 0) AS `Jun 2021`,  COALESCE(`Jul 2021`, 0) AS `Jul 2021`,
    COALESCE(`Aug 2021`, 0) AS `Aug 2021`,  COALESCE(`Sep 2021`, 0) AS `Sep 2021`,
    COALESCE(`Oct 2021`, 0) AS `Oct 2021`,  COALESCE(`Nov 2021`, 0) AS `Nov 2021`,
    COALESCE(`Dec 2021`, 0) AS `Dec 2021`,  COALESCE(`Jan 2022`, 0) AS `Jan 2022`,
    COALESCE(`Feb 2022`, 0) AS `Feb 2022`,  COALESCE(`Mar 2022`, 0) AS `Mar 2022`,
    COALESCE(`Apr 2022`, 0) AS `Apr 2022`,  COALESCE(`May 2022`, 0) AS `May 2022`,
    COALESCE(`Jun 2022`, 0) AS `Jun 2022`,  COALESCE(`Jul 2022`, 0) AS `Jul 2022`,
    COALESCE(`Aug 2022`, 0) AS `Aug 2022`
  FROM `passenger-data-jkia.raw_passenger_data.raw_grid_generation`
)
UNPIVOT(passengers FOR month_label IN (
  `Jun 2019`, `Jul 2019`, `Aug 2019`, `Sep 2019`, `Oct 2019`, `Nov 2019`, `Dec 2019`,
  `Jan 2020`, `Feb 2020`, `Mar 2020`, `Apr 2020`, `May 2020`, `Jun 2020`, `Jul 2020`,
  `Aug 2020`, `Sep 2020`, `Oct 2020`, `Nov 2020`, `Dec 2020`,
  `Jan 2021`, `Feb 2021`, `Mar 2021`, `Apr 2021`, `May 2021`, `Jun 2021`, `Jul 2021`,
  `Aug 2021`, `Sep 2021`, `Oct 2021`, `Nov 2021`, `Dec 2021`,
  `Jan 2022`, `Feb 2022`, `Mar 2022`, `Apr 2022`, `May 2022`, `Jun 2022`, `Jul 2022`, `Aug 2022`
));


-- ============================================================
-- STEP 2 — Sanity check. Expect exactly:
--   destination   1599 rows   41 ports
--   region_total   351 rows    9 regions
--   grand_total     39 rows    1
-- ============================================================
SELECT row_type, COUNT(*) AS n_rows, COUNT(DISTINCT port) AS n_ports
FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
GROUP BY row_type;


-- ============================================================
-- STEP 3 — Audit 1: region totals vs sum of their destinations.
-- Expect ZERO rows back (already confirmed clean).
-- ============================================================
WITH dest_sum AS (
  SELECT region, month_label, SUM(passengers) AS summed_from_destinations
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'destination'
  GROUP BY region, month_label
),
reported AS (
  SELECT region, month_label, passengers AS reported_total
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'region_total'
)
SELECT r.region, r.month_label, d.summed_from_destinations, r.reported_total,
       d.summed_from_destinations - r.reported_total AS diff
FROM reported r
JOIN dest_sum d USING (region, month_label)
WHERE ABS(d.summed_from_destinations - r.reported_total) > 0.5
ORDER BY ABS(diff) DESC;


-- ============================================================
-- STEP 4 — Audit 2: Total International vs sum of all regions.
-- Expect exactly 2 rows: Jun 2019 (diff ~4750), Jul 2019 (diff ~6548).
-- ============================================================
WITH region_sum AS (
  SELECT month_label, SUM(passengers) AS summed_regions
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'region_total'
  GROUP BY month_label
),
ungrouped AS (
  SELECT month_label, SUM(passengers) AS other_intl
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE region = 'Ungrouped International'
  GROUP BY month_label
),
grand AS (
  SELECT month_label, passengers AS reported_grand_total
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'grand_total'
)
SELECT g.month_label,
       r.summed_regions + u.other_intl AS computed_grand_total,
       g.reported_grand_total,
       (r.summed_regions + u.other_intl) - g.reported_grand_total AS diff
FROM grand g
JOIN region_sum r USING (month_label)
JOIN ungrouped u USING (month_label)
ORDER BY ABS(diff) DESC;


-- ============================================================
-- STEP 5 — Duplicate check. Expect ZERO rows.
-- ============================================================
SELECT port, month_label, COUNT(*) AS n
FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
GROUP BY port, month_label
HAVING COUNT(*) > 1;


-- ============================================================
-- STEP 6 — Null / negative value scan. Expect ZERO rows.
-- ============================================================
SELECT *
FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
WHERE passengers IS NULL OR passengers < 0;


-- ============================================================
-- STEP 7 — ANALYSIS: pre-COVID vs recovery-period average, per
-- destination. This is the "winners and losers" table — recovery
-- ratio > 1 means the route now carries MORE traffic than in 2019.
-- ============================================================
WITH precovid AS (
  SELECT port, region, AVG(passengers) AS precovid_avg
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'destination'
    AND month_date BETWEEN '2019-06-01' AND '2019-12-01'
  GROUP BY port, region
),
recent AS (
  SELECT port, AVG(passengers) AS recent_avg
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'destination'
    AND month_date BETWEEN '2022-06-01' AND '2022-08-01'
  GROUP BY port
)
SELECT
  p.port, p.region,
  ROUND(p.precovid_avg, 0) AS precovid_avg,
  ROUND(r.recent_avg, 0) AS recent_avg,
  ROUND(SAFE_DIVIDE(r.recent_avg, p.precovid_avg), 2) AS recovery_ratio
FROM precovid p
JOIN recent r USING (port)
ORDER BY recovery_ratio DESC;


-- ============================================================
-- STEP 8 — Destinations effectively dead since COVID.
-- Instead of requiring EXACTLY zero passengers from 2021 onward,
-- this flags any destination whose total 2021+ volume is under
-- 1% of its pre-COVID volume.
-- ============================================================
WITH precovid AS (
  SELECT port, region, SUM(passengers) AS precovid_sum
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'destination'
    AND month_date < '2020-01-01'
  GROUP BY port, region
),
recovery AS (
  SELECT port, SUM(passengers) AS recovery_sum
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'destination'
    AND month_date >= '2021-01-01'
  GROUP BY port
)
SELECT
  p.port, p.region,
  p.precovid_sum,
  r.recovery_sum,
  ROUND(SAFE_DIVIDE(r.recovery_sum, p.precovid_sum), 4) AS recovery_share
FROM precovid p
JOIN recovery r USING (port)
WHERE p.precovid_sum > 500                              -- had a real pre-COVID route
  AND SAFE_DIVIDE(r.recovery_sum, p.precovid_sum) < 0.01 -- under 1% of it came back
ORDER BY recovery_share ASC;


-- ============================================================
-- STEP 9 — ANALYSIS: consistently top-5 destinations, year by
-- year. Look for ports that appear in the top 5 in every year,
-- including 2020 — that's your "never lost rank" finding.
-- ============================================================
WITH yearly AS (
  SELECT port, EXTRACT(YEAR FROM month_date) AS yr, AVG(passengers) AS yearly_avg
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'destination'
  GROUP BY port, yr
),
ranked AS (
  SELECT *, RANK() OVER (PARTITION BY yr ORDER BY yearly_avg DESC) AS rnk
  FROM yearly
)
SELECT port, yr, ROUND(yearly_avg,0) AS yearly_avg, rnk
FROM ranked
WHERE rnk <= 5
ORDER BY yr, rnk;


-- ============================================================
-- STEP 10 — ANALYSIS: calendar-month seasonality, excluding 2020.
-- Expect August highest, February lowest.
-- ============================================================
SELECT
  FORMAT_DATE('%b', month_date) AS calendar_month,
  ROUND(AVG(passengers), 0) AS avg_passengers
FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
WHERE row_type = 'grand_total'
  AND EXTRACT(YEAR FROM month_date) != 2020
GROUP BY calendar_month, EXTRACT(MONTH FROM month_date)
ORDER BY EXTRACT(MONTH FROM month_date);


-- ============================================================
-- STEP 11 — Build your Tableau export. This is the table you'll
-- download as CSV (or connect Google Sheets to) and bring into
-- Tableau Public as a static extract. Destination-level only.
-- ============================================================
CREATE OR REPLACE TABLE `passenger-data-jkia.raw_passenger_data.tableau_export` AS
SELECT port, region, month_label, month_date, passengers
FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
WHERE row_type = 'destination';


-- ============================================================
-- STEP 12 — Region share of total traffic, pre-COVID vs now.
-- Supports the "Kenya's air links are regionalizing" thread:
-- did African/regional routes take a bigger slice of the pie
-- post-COVID, even while long-haul volumes lagged?
-- ============================================================
WITH precovid AS (
  SELECT region, SUM(passengers) AS precovid_sum
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'destination'
    AND month_date BETWEEN '2019-06-01' AND '2019-12-01'
  GROUP BY region
),
recent AS (
  SELECT region, SUM(passengers) AS recent_sum
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'destination'
    AND month_date BETWEEN '2022-06-01' AND '2022-08-01'
  GROUP BY region
),
precovid_total AS (SELECT SUM(precovid_sum) AS t FROM precovid),
recent_total AS (SELECT SUM(recent_sum) AS t FROM recent)
SELECT
  p.region,
  ROUND(100 * p.precovid_sum / pt.t, 1) AS precovid_share_pct,
  ROUND(100 * r.recent_sum / rt.t, 1) AS recent_share_pct,
  ROUND(100 * r.recent_sum / rt.t, 1) - ROUND(100 * p.precovid_sum / pt.t, 1) AS pct_point_change
FROM precovid p
JOIN recent r USING (region)
CROSS JOIN precovid_total pt
CROSS JOIN recent_total rt
ORDER BY pct_point_change DESC;


-- ============================================================
-- STEP 13 — Months to full recovery, per destination.
-- For each named destination, find the first post-2020 month
-- where its passengers matched or exceeded its own 2019 monthly
-- average.
-- ============================================================
WITH precovid_avg AS (
  SELECT port, region, AVG(passengers) AS precovid_monthly_avg
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
  WHERE row_type = 'destination'
    AND month_date BETWEEN '2019-06-01' AND '2019-12-01'
  GROUP BY port, region
),
candidates AS (
  SELECT
    l.port, l.month_date, l.passengers, p.precovid_monthly_avg
  FROM `passenger-data-jkia.raw_passenger_data.passengers_long` l
  JOIN precovid_avg p USING (port)
  WHERE l.row_type = 'destination'
    AND l.month_date >= '2020-06-01'
    AND l.passengers >= p.precovid_monthly_avg
),
first_recovery AS (
  SELECT port, MIN(month_date) AS first_recovered_month
  FROM candidates
  GROUP BY port
)
SELECT
  p.port, p.region,
  ROUND(p.precovid_monthly_avg, 0) AS precovid_monthly_avg,
  f.first_recovered_month,
  DATE_DIFF(f.first_recovered_month, DATE('2020-03-01'), MONTH) AS months_since_covid_start
FROM precovid_avg p
LEFT JOIN first_recovery f USING (port)
ORDER BY f.first_recovered_month ASC NULLS LAST;


-- ============================================================
-- STEP 14 — Route dominance shift (Top 5 and Top 10 combined).
-- ============================================================
WITH
  stats AS (
    SELECT
      port,
      region,
      SUM(CASE WHEN month_date BETWEEN '2019-06-01' AND '2019-12-01' THEN passengers ELSE 0 END) AS precovid_vol,
      SUM(CASE WHEN month_date BETWEEN '2022-06-01' AND '2022-08-01' THEN passengers ELSE 0 END) AS recent_vol
    FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
    WHERE row_type = 'destination'
    GROUP BY 1, 2
  ),
  ranked_precovid AS (
    SELECT port, precovid_vol, precovid_vol / SUM(precovid_vol) OVER () AS share,
           RANK() OVER (ORDER BY precovid_vol DESC) AS rnk
    FROM stats
  ),
  ranked_recent AS (
    SELECT port, recent_vol, recent_vol / SUM(recent_vol) OVER () AS share,
           RANK() OVER (ORDER BY recent_vol DESC) AS rnk
    FROM stats
  ),
  precovid_agg AS (
    SELECT
      5 AS top_n,
      ROUND(100 * SUM(CASE WHEN rnk <= 5 THEN share ELSE 0 END), 1) AS precovid_dominance_pct
    FROM ranked_precovid
    UNION ALL
    SELECT
      10 AS top_n,
      ROUND(100 * SUM(CASE WHEN rnk <= 10 THEN share ELSE 0 END), 1)
    FROM ranked_precovid
  ),
  recent_agg AS (
    SELECT
      5 AS top_n,
      ROUND(100 * SUM(CASE WHEN rnk <= 5 THEN share ELSE 0 END), 1) AS recent_dominance_pct
    FROM ranked_recent
    UNION ALL
    SELECT
      10 AS top_n,
      ROUND(100 * SUM(CASE WHEN rnk <= 10 THEN share ELSE 0 END), 1)
    FROM ranked_recent
  )
SELECT
  CONCAT('Top ', CAST(p.top_n AS STRING), ' Routes') AS category,
  p.precovid_dominance_pct,
  r.recent_dominance_pct,
  r.recent_dominance_pct - p.precovid_dominance_pct AS percentage_point_shift
FROM precovid_agg p
JOIN recent_agg r USING (top_n)
ORDER BY p.top_n;


-- ============================================================
-- STEP 15 — Recovery Momentum (Month-over-Month Growth)
-- Calculates the 3-month rolling growth rate for the grand total
-- traffic to identify the "speed" of recovery.
-- ============================================================
WITH
  monthly_totals AS (
    SELECT month_date, passengers AS total_passengers
    FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
    WHERE row_type = 'grand_total'
  ),
  momentum_calc AS (
    SELECT
      month_date,
      total_passengers,
      LAG(total_passengers, 1) OVER (ORDER BY month_date) AS prev_month_vol,
      AVG(total_passengers)
        OVER (ORDER BY month_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)
        AS rolling_3mo_avg
    FROM monthly_totals
  )
SELECT
  month_date,
  total_passengers,
  ROUND(
    100 * (total_passengers - prev_month_vol) / SAFE_DIVIDE(prev_month_vol, 1),
    1)
    AS mom_growth_pct,
  ROUND(rolling_3mo_avg, 0) AS rolling_3mo_avg,
  CASE
    WHEN total_passengers > rolling_3mo_avg THEN 'Accelerating'
    WHEN total_passengers < rolling_3mo_avg THEN 'Slowing/Plateauing'
    ELSE 'Steady'
  END AS momentum_status
FROM momentum_calc
WHERE month_date >= '2021-01-01'
ORDER BY month_date DESC;


-- ============================================================
-- STEP 16 — Route volatility: steady demand vs month-to-month swings.
-- Uses coefficient of variation (stdev / mean) on 2019-only months.
-- ============================================================
SELECT
  port, region,
  ROUND(AVG(passengers), 0) AS avg_2019,
  ROUND(STDDEV(passengers), 0) AS stdev_2019,
  ROUND(SAFE_DIVIDE(STDDEV(passengers), AVG(passengers)), 2) AS coefficient_of_variation
FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
WHERE row_type = 'destination'
  AND month_date BETWEEN '2019-06-01' AND '2019-12-01'
GROUP BY port, region
HAVING AVG(passengers) > 300   -- ignore tiny routes, CV is noisy there
ORDER BY coefficient_of_variation ASC;


-- ============================================================
-- STEP 17 — Region-level export (parallel to tableau_export).
-- For any chart built at the region level so you never mix
-- region-total rows into a destination-level chart.
-- ============================================================
CREATE OR REPLACE TABLE `passenger-data-jkia.raw_passenger_data.tableau_export_regions` AS
SELECT port AS region_row_label, region, month_label, month_date, passengers
FROM `passenger-data-jkia.raw_passenger_data.passengers_long`
WHERE row_type IN ('region_total', 'grand_total');
