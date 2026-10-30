# U.S. Domestic Flight Performance & Operational Risk Analysis (2019–2023)

Interactive Tableau dashboards that analyze **3M+ U.S. domestic flights** from the Bureau of Transportation Statistics (BTS) to show how delays, cancellations and route reliability changed before, during and after the COVID-19 pandemic.

**Live dashboards (Tableau Public):**

| Dashboard | What it answers | Link |
|---|---|---|
| Executive Summary & Volume Trends | Flight volume, busiest hubs, busiest weekday, yearly cancellation rate | [Open dashboard](https://public.tableau.com/app/profile/vigneshwari.jayaprakash/viz/U_S_DomesticFlightPerformanceandOperationalRiskAnalysis20192023/ExecutiveSummary?publish=yes) |
| Delay & Operational Risk | Airline delay recovery, high-risk routes, time-of-day and monthly delays, distance vs. delay | [Open dashboard](https://public.tableau.com/app/profile/vigneshwari.jayaprakash/viz/U_S_DomesticFlightPerformanceandOperationalRiskAnalysis20192023/DelayDeepDive?publish=yes) |
| Cancellation Deep Dive | Cancellation causes by year and a running total of cancellations | [Open dashboard](https://public.tableau.com/app/profile/vigneshwari.jayaprakash/viz/U_S_DomesticFlightPerformanceandOperationalRiskAnalysis20192023/CancellationAnalysis?publish=yes) |

*Course project for IFT 533 (Data Visualization), Arizona State University. Team: Nikita Kumari, Vigneshwari Jayaprakash, Lochan Raju Shivaji, Vaishnavi Reddy Kunduri.*

---

## Key Findings

- **Pandemic disruption:** the cancellation rate spiked in 2020 and fell back by 2021, while flight volume dropped sharply in 2020 and recovered over the following years.
- **Seasonal delays:** average departure delay peaks in June and July (about 14.8 minutes) and is lowest in September (about 5.7 minutes), so summer delays run roughly 2.5x higher than fall.
- **Time of day matters:** flights that depart in the morning arrive early on average (−1.5 minutes), while night departures arrive 17+ minutes late as delays build up through the day.
- **Hubs and traffic:** Atlanta is the busiest origin airport, and Thursday is the busiest day of the week.
- **Delay recovery varies by airline:** some carriers make up departure delays in the air, while others carry them through to arrival.
- **High-risk routes:** a small set of origin–destination pairs show a high share of flights delayed more than 30 minutes.

---

## Who It's For

| User | How they use the dashboards |
|---|---|
| Airline operations managers | Compare fleet performance with competitors, find root causes of cancellations, reduce late-aircraft delays |
| Airport authorities | Spot peak delay times and congested routes to plan staffing and ground resources |
| Frequent flyers & travel agencies | Pick reliable airlines and routes, and avoid high-delay months and airports |
| Policy makers (FAA / DOT) | Track post-pandemic recovery and systemic National Airspace System (NAS) issues |

---

## Questions the Dashboards Answer

1. What is the annual flight cancellation rate from 2019 to 2023?
2. Which airports are the main hubs by departing flight volume?
3. What is the average departure delay for each month?
4. Which day of the week has the most air traffic?
5. What are the main causes of cancellations, and how did they change over time?
6. Does flight distance relate to how likely or how long a delay is?
7. How do arrival delays differ by time of day (morning, afternoon, evening, night)?
8. Which airlines recover best from departure delays (high departure delay but lower arrival delay)?
9. How did flight volume and cancellations after the pandemic (2020–2023) compare with 2019?
10. Which origin–destination pairs are most often delayed by more than 30 minutes?
11. Do certain months or seasons have unusually many weather-related delays?
12. For a selected airport or airline, how did delay patterns change across seasons and years?

---

## Visualizations (11)

| # | Chart | Type | Questions |
|---|---|---|---|
| 1 | Yearly Cancellation Percentage Trend | Line chart | Q1, Q9 |
| 2 | Top 10 Busiest Origin Airports | Bar chart with Top-N slider | Q2 |
| 3 | Average Departure Delay by Month | Line chart, peak month highlighted | Q3, Q11 |
| 4 | Flights by Day of Week | Bar chart, busiest day highlighted | Q4 |
| 5 | Cancellation Reasons Over Time | 100% stacked bar chart | Q5, Q9, Q11 |
| 6 | Distance vs. Delay | Scatter plot colored by delay severity | Q6 |
| 7 | Time of Day vs. Average Arrival Delay | Bar chart with color gradient | Q7 |
| 8 | Airline Delay Recovery | Stacked bars (departure vs. arrival delay) | Q8 |
| 9 | Flight Volume Comparison | Bars + cancellation-rate line (dual axis) | Q1, Q9 |
| 10 | High-Delay Routes | Ranked bar chart (% delayed > 30 min) | Q10, Q12 |
| 11 | Running Total of Cancellations | Line chart using a table calculation | Q1, Q9, Q11 |

---

## Interactivity

- **Year filter** (2019–2023) for comparing pandemic and pre-pandemic years
- **Airline filter** (multi-select) to compare carriers or focus on one fleet
- **Origin / destination airport filters** for airport managers and specific routes
- **Cancellation reason filter** (Carrier, Weather, NAS, Security)
- **Delay threshold parameter** (0–180 minutes) that redefines what counts as a "high delay" on the routes chart
- **Time-of-day filter** (Morning, Afternoon, Evening, Night)
- **Top-N slider** for the busiest airports chart
- Tooltips on every chart

---

## Dataset

- **Source:** [Flight Delay and Cancellation Dataset (2019–2023)](https://www.kaggle.com/datasets/patrickzel/flight-delay-and-cancellation-dataset-2019-2023) on Kaggle, originally from the U.S. Bureau of Transportation Statistics
- **Size:** about 3M rows (sample), 32 columns
- **Key fields:**
  - *Date and time:* `FL_DATE`, `CRS_DEP_TIME`, `DEP_TIME`, `CRS_ARR_TIME`, `ARR_TIME`
  - *Categories:* `AIRLINE_CODE`, `ORIGIN`, `DEST`, `CANCELLATION_CODE`
  - *Metrics:* `DEP_DELAY`, `ARR_DELAY`, `DISTANCE`, `CANCELLED`, `DIVERTED`, `TAXI_OUT`, `TAXI_IN`
  - *Delay causes:* `DELAY_DUE_CARRIER`, `DELAY_DUE_WEATHER`, `DELAY_DUE_NAS`, `DELAY_DUE_SECURITY`, `DELAY_DUE_LATE_AIRCRAFT`

The raw dataset is not stored in this repository because of its size. Download it from Kaggle to rebuild the workbook.

---

## Data Preparation (in Tableau)

1. **Duplicate data source** so calculations could be tested without changing the original connection.
2. **Time conversion:** HHMM integer columns converted to real times, for example
   `MAKETIME(INT([CRS_DEP_TIME]/100), INT([CRS_DEP_TIME]%100), 0)`, then grouped into Morning, Afternoon, Evening and Night buckets.
3. **Null handling:**
   - Delay columns: `IFNULL([DEP_DELAY], 0)` (null means no recorded delay)
   - Categorical columns: `IFNULL([ORIGIN], "Unknown")`
   - Rows missing key IDs such as flight number were excluded
4. **Derived date fields:** Year, Month and Day of Week from `FL_DATE`.
5. **Semantic cleanup:** `CANCELLED` and `DIVERTED` treated as 0/1 flags; cancellation codes mapped to readable labels.
6. **Calculated fields:**
   - Cancellation rate: `SUM([CANCELLED]) / COUNT([FL_NUMBER])`
   - Route: `[ORIGIN] + " → " + [DEST]`
   - High delay flag: `[ARR_DELAY] > [Delay Threshold]`
7. **Table calculations:** running total of cancellations, rankings and percent of total.

---

## Tools

Tableau Desktop · Tableau Public · Calculated fields · Table calculations · Parameters · Filters and dashboard actions

---

## Repository Contents

| Path | Description |
|---|---|
| `dashboards/` | Tableau workbook (`.twbx`) |
| `reports/` | Phase 1 (requirements), Phase 2 (design and sketches), Phase 3 (implementation) reports |
| `screenshots/` | Dashboard screenshots |

*(Adjust this table to match the files you upload.)*

---

## Notes and Limitations

- Results come from a sampled version of the BTS data, so counts are lower than official BTS totals.
- In the 2020 cancellation breakdown, most cancellations show the "Security" code. That likely reflects how cancellation codes were mapped in the sample rather than real security events, so the 2020 cause mix should be read with caution.

---

## References

- [U.S. DOT – Air Travel Consumer Report / On-Time Performance](https://www.transportation.gov/policy/aviation-policy/on-time-performance)
- [Bureau of Transportation Statistics](https://www.bts.gov/)
- [FAA – Air Traffic by the Numbers](https://www.faa.gov/air_traffic/by_the_numbers)
- [Tableau Help](https://www.tableau.com/support/help)
