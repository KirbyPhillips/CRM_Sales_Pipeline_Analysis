# CRM Sales Pipeline Analysis 2024

### Note: 

This repository outlines the full technical process used in the analysis. Key business insights and recommendations are provided at the end, with a comprehensive report available in this folder and on my website: www.primepeakinsights.com

## Table of Contents

This repository is structured to walk you through the end-to-end process of the CRM Sales Pipeline Analysis, from the initial business problem through to the final insights and reflections. Each section builds on the previous one, following the same logical flow used to plan and execute the analysis itself.

1. [Project Overview](#project-overview)
2. [Tools and Technologies](#tools-and-technologies)
3. [The SCAN Framework](#the-scan-framework)
4. [Project Phases](#project-phases)
5. [Insights for the Business](#insights-for-the-business)
6. [In Hindsight](#in-hindsight)
7. [Concluding notes](#concluding-notes)
---

## 1. Project Overview

In 2024, a European B2B sales organisation operating across 9 countries and 14 industries had no consolidated view of its CRM pipeline performance. Lead data covering 3,000 prospects registered between January and May 2024 existed as a single flat, denormalised export with no analytical structure. Key information such as agent performance, conversion rates, deal values, and stage progression could not be interrogated or acted upon.

As a result, stakeholders were unable to answer critical business questions, such as:

- Which agents were converting leads and which were consistently losing deals?
- Where in the funnel were opportunities stalling before becoming customers?
- Which industries and organisation sizes were generating the highest deal values?

This project was built to bridge that gap by transforming a raw CRM export into a structured, end-to-end Power BI analytics solution. The goal was to enable clear visibility into pipeline health, support data-driven sales management decisions, and uncover actionable insights across agents, industries, products, and geographies.

---

## 2. Tools and Technologies

The following tools and technologies were used to complete this analysis:

| Tool | Purpose |
|---|---|
| Microsoft Excel | Dataset inspection and preliminary diagnosis |
| Power BI Desktop | Data modelling, DAX measures, and report building |
| Power Query | Data transformation and normalisation; Calendar table generation |
| DAX | 34 measures and 3 calculated columns across 4 display folders |
| JSON | Custom Ocean Mist theme applied across all visual types |

---

## 3. The SCAN Framework

This analysis was planned and executed using the SCAN Framework, a personal structured methodology developed to bring clarity to every stage of the process.

| Step | Description |
|---|---|
| S - Scope the Situation | - Defined the business problem and the cost of inaction. <br><br> - **HOW?** A European B2B sales organisation had no consolidated view of its CRM pipeline across 9 countries and 14 industries. With 3,000 leads existing as a single flat export, stakeholders could not identify where deals were stalling, which agents were underperforming, or how accurately the team was forecasting close dates. |
| C - Confirm the Core Metrics | - Established the four analytical themes: Pipeline Health, Agent Performance, Revenue Intelligence, and Win/Loss Diagnostics. <br><br> - **HOW?** Mapped 34 DAX measures and 3 calculated columns to directly answer the 12 business questions defined in the brief, ensuring every metric served a specific analytical purpose. |
| A - Build the Architecture | - Normalised the raw CRM export and built a star schema with 7 tables, 8 manually built relationships, and 34 DAX measures organised into 4 display folders. <br><br> - **HOW?** Every technical decision, from the Power Query dimension tables to the custom DIM_Calendar spanning January 2024 to December 2025, was made to support the four analytical themes confirmed in the C step. |
| N - Narrate the Story | - Designed all 5 report page backgrounds and structured each page around a single focused analytical narrative. <br><br> - **HOW?** Applied the custom Ocean Mist JSON theme across all visual types and used cohesion, aesthetic, rhythm, and emphasis to ensure every design decision served the story the data was telling. |

---

## 4. Project Phases

A structured, end-to-end workflow was followed to transform a raw CRM export into a scalable and insight-driven Power BI solution. The project spans data preparation, modelling, optimisation, and dashboard design, with each phase building toward actionable business insights.

### PHASE 1: Data Preparation (Power Query)

**Steps:**

- Reviewed the raw `CRM Sales Pipeline Dataset.xlsx` file across 2 sheets: CRM_data and Data Dictionary
- Connected to the source file in Power Query and loaded `CRM_data` as the base query `FACT_CRM`
- Normalised the flat file by creating 5 dimension tables using the Reference method in Power Query: `DIM_Agents`, `DIM_Industries`, `DIM_OrgSize`, and `DIM_Products`
- Built `DIM_Countries` as a direct Excel source connection (independent of `FACT_CRM`) to allow clean removal of `Latitude` and `Longitude` from the fact table without breaking references
- Removed ghost columns `Column18` and `Column19` from `FACT_CRM`
- Fixed the `Lattitude` typo, renaming it to `Latitude` in `DIM_Countries`
- Renamed columns for consistency and clarity: `Deal Value, $` to `Deal Value`, `Probability, %` to `Probability`, `Owner` to `Agent`, and standardised all date column names to Title Case
- Removed `Latitude` and `Longitude` from `FACT_CRM` as these now belong exclusively to `DIM_Countries`
- Built `DIM_Calendar` from scratch in Power Query using M code, spanning January 2024 to December 2025 with 12 columns: Date, Year, Month Number, Month Name, Month Short, Month Initial, Quarter, Weekday Number (Monday=1, Sunday=7), Weekday, Weekday Short, Weekday Initial, and Year-Month

**Data prep image:**

---

### Characteristics of the Dataset

| Property | Detail |
|---|---|
| Source | CRM Sales Pipeline Dataset.xlsx |
| Period | January 1 to May 31, 2024 |
| Total Leads | 3,000 |
| Countries | 9 (Austria, Belgium, France, Germany, Italy, Netherlands, Portugal, Spain, Switzerland) |
| Industries | 14 |
| Organisation Sizes | 5 (Micro, Small, Medium, Large, Enterprise) |
| Products | 3 (SAAS, Services, Custom Solution) |
| Sales Agents | 8 |
| Pipeline Statuses | 7 (New, Qualified, Disqualified, Sales Accepted, Opportunity, Customer, Churned Customer) |
| Opportunity Stages | 6 (Opened, Initial Contact, Nurturing, Proposal Sent, Won, Lost) |
| Currency | USD |

---

### PHASE 2: Data Model Setup (Power BI)

**Steps:**

- Loaded all 7 queries into Power BI via Power Query: `FACT_CRM`, `DIM_Agents`, `DIM_Countries`, `DIM_Industries`, `DIM_OrgSize`, `DIM_Products`, and `DIM_Calendar`
- Disabled auto-detect relationships and auto date/time tables for complete manual control
- Built a star schema with 8 relationships: 5 active and 3 date relationships (1 active, 2 inactive) connecting `DIM_Calendar` to `FACT_CRM` via `Lead Acquisition Date`, `Expected Close Date`, and `Actual Close Date`
- Marked `DIM_Calendar` as the official date table with `Date` as the key column
- Renamed `DIM_Date` to `DIM_Calendar` after loading

### Data Model Layout

The data model in this analysis consisted of 3 key areas: tables, star schema, and relationships.

#### 1) Tables

This is a summary of the table structure:

| Table | Rows | Columns | Description |
|---|---|---|---|
| FACT_CRM | 3,000 | 15 | Central fact table — all lead records, measures, and foreign keys |
| DIM_Agents | 8 | 1 | 8 unique sales agents |
| DIM_Countries | 9 | 3 | Country name, latitude, and longitude |
| DIM_Industries | 14 | 1 | 14 industry categories |
| DIM_OrgSize | 5 | 1 | 5 organisation size bands from Micro to Enterprise |
| DIM_Products | 3 | 1 | 3 product types: SAAS, Services, and Custom Solution |
| DIM_Calendar | 730 | 12 | Date table built in Power Query spanning January 2024 to December 2025 |
| _Measures | 0 | - | Dedicated measures table with 34 DAX measures and 3 calculated columns |

#### 2) Star Schema

This is a summary of the data model's star schema layout:

```
FACT_CRM (3,000 rows, 15 columns)
|
|-- DIM_Agents (8 rows, 1 column)        [via Agent]
|-- DIM_Countries (9 rows, 3 columns)    [via Country]
|-- DIM_Industries (14 rows, 1 column)   [via Industry]
|-- DIM_OrgSize (5 rows, 1 column)       [via Organization Size]
|-- DIM_Products (3 rows, 1 column)      [via Product]
|-- DIM_Calendar (730 rows, 12 columns)  [via Lead Acquisition Date — Active]
|-- DIM_Calendar (730 rows, 12 columns)  [via Expected Close Date — Inactive]
|-- DIM_Calendar (730 rows, 12 columns)  [via Actual Close Date — Inactive]
```

This image below shows all the above mentioned components of this data model in a logical star schema design. It illustrates the relationship between the central FACT_CRM table and supporting dimension tables, structured to enable scalable and efficient analytical reporting.

**Data model image:**

#### 3) Relationships

This is a summary of the relationship cardinality:

| From | To | Column | Cardinality | Status |
|---|---|---|---|---|
| DIM_Agents | FACT_CRM | Agent | One to Many | Active |
| DIM_Countries | FACT_CRM | Country | One to Many | Active |
| DIM_Industries | FACT_CRM | Industry | One to Many | Active |
| DIM_OrgSize | FACT_CRM | Organization Size | One to Many | Active |
| DIM_Products | FACT_CRM | Product | One to Many | Active |
| DIM_Calendar | FACT_CRM | Lead Acquisition Date | One to Many | Active |
| DIM_Calendar | FACT_CRM | Expected Close Date | One to Many | Inactive |
| DIM_Calendar | FACT_CRM | Actual Close Date | One to Many | Inactive |

--- 

#### Data Model Performance

The data model is structured as a star schema with one central fact table (FACT_CRM) and 6 dimension tables (DIM_Agents, DIM_Countries, DIM_Industries, DIM_OrgSize, DIM_Products, and DIM_Calendar), built entirely from a single flat CRM export. The model covers 3,000 leads across 9 countries, 14 industries, 5 organisation sizes, and 3 product types, with 34 DAX measures organised into 4 display folders and 3 calculated columns.

**This data model allows this analysis to:**

- Track Total Leads (3,000), Won Deals (83), Lost Deals (61), and Total Deal Value ($8.3M) at lead level and aggregate across any combination of agent, country, industry, organisation size, product, and status.
- Analyse pipeline health month-over-month across all 5 lead acquisition months using the active relationship between DIM_Calendar and FACT_CRM via Lead Acquisition Date.
- Forecast weighted pipeline income by multiplying each lead's Deal Value by its close Probability, producing a risk-adjusted revenue estimate of $3.7M against a total pipeline value of $8.3M.
- Activate inactive date relationships using USERELATIONSHIP() in DAX to analyse performance by Expected Close Date or Actual Close Date, enabling forecast accuracy and sales cycle duration analysis.
- Compare agent performance across Win Rate, Conversion Rate, average deal value, median deal value, and sales cycle duration — surfacing performance gaps such as David Wilson at 33.3% Win Rate versus Sarah Davis at 72.7% on a comparable lead volume.
- Measure sales cycle efficiency using 3 calculated columns: Days to Expected Close, Days to Actual Close, and Close Date Variance — enabling precise diagnosis of forecast accuracy at the individual lead level.

---

### PHASE 3: Model Optimisation

**Steps:**

- Created 34 DAX measures organised into 4 display folders in a dedicated `_Measures` table: Pipeline Health, Agent Performance, Revenue Intelligence, and Win/Loss Diagnostics
- Created 3 calculated columns in `FACT_CRM` grouped in a dedicated Calculated Columns display folder: `Days to Expected Close`, `Days to Actual Close`, and `Close Date Variance`
- Standardised all column naming conventions to Title Case across all tables
- Hidden all foreign key columns from report view in `FACT_CRM`: `Country`, `Industry`, `Agent`, `Organization Size`, `Product`, `Status Sequence`, and `Stage Sequence`
- Added descriptions to all model objects including tables, columns, and measures explaining their purpose and the logic behind each DAX calculation in plain English
- Set Sort By Column for all date and categorical display columns in `DIM_Calendar`: Month Name, Month Short, and Month Initial sorted by Month Number; Weekday, Weekday Short, and Weekday Initial sorted by Weekday Number
- Set `Status` to sort by `Status Sequence` and `Stage` to sort by `Stage Sequence` in `FACT_CRM` to ensure correct funnel ordering across all visuals
- Set data categories on `DIM_Countries`: `Country` as Country, `Latitude` as Latitude, and `Longitude` as Longitude to enable map visuals
- Marked `DIM_Calendar` as the official date table with `Date` as the key column

#### Measures Organisation and Model Optimisation

---

### PHASE 4: Report Building (Power BI)

Built 5 report pages, each focused on one of the four analytical themes defined in the brief:

| Page | Name | Business Questions Answered |
|---|---|---|
| 1 | Executive Overview | High-level pipeline summary, lead volume trend, and team performance snapshot |
| 2 | Pipeline Health | Funnel drop-off analysis, stage distribution by month, won and lost deal counts |
| 3 | Agent Performance | Win vs loss ratio, sales cycle duration, and deal value distribution by agent |
| 4 | Revenue Intelligence | Deal value by product, industry, organisation size, and monthly weighted forecast |
| 5 | Win & Loss Analysis | Forecast accuracy, sales cycle by agent and country, and industry win/loss breakdown |

---

### PHASE 5: Design and Theme

**Steps:**

- Applied the custom Ocean Mist JSON theme across all visual types, built by adapting the Emerald Tide theme structure and replacing the full colour palette with the CRM project's 5-colour Ocean Mist palette: `#1B78F2`, `#194073`, `#168BF2`, `#D8E6F2`, and `#91CCD9`
- Manually assigned stage colours for the Opportunity Stage Distribution chart to ensure all 6 stages were clearly distinguishable against the light blue page background
- Added average reference lines to the Agent Performance Overview scatter plot to divide the chart into four performance quadrants
- Set the Y-axis minimum to zero on the Monthly Lead Acquisition Trend area chart to ensure honest representation of lead volume fluctuation
- Applied cohesion, aesthetic, rhythm, and emphasis considerations throughout all 5 report pages

---

<!--
## Measures Library
34 DAX measures and 3 calculated columns organised across 4 display folders:

| Folder | Count | Key Measures |
|---|---|---|
| Pipeline Health | 10 | Total Leads, Leads by Status, New Leads, Qualified Leads, Disqualified Leads, Sales Accepted Leads, Opportunities, Customers, Churned Customers, Leads MoM % |
| Agent Performance | 7 | Won Deals, Lost Deals, Win Rate %, Conversion Rate %, Avg Days to Actual Close, Avg Days to Expected Close, Close Date Variance (Days) |
| Revenue Intelligence | 10 | Total Deal Value, Avg Deal Value, Median Deal Value, Max Deal Value, Min Deal Value, Weighted Pipeline Value, Avg Probability %, Total Won Value, Total Lost Value, Deal Value MoM % |
| Win & Loss Analysis | 7 | Lost Rate %, Disqualification Rate %, Churn Rate %, Avg Lost Deal Value, Avg Won Deal Value, Avg Days to Close - Won, Avg Days to Close - Lost |
| Calculated Columns | 3 | Days to Expected Close, Days to Actual Close, Close Date Variance |
-->

--- 

## 5. Insights for the Business

This section translates key analytical findings into clear, actionable strategies that directly support pipeline growth and revenue conversion. It enables the business to make data-driven decisions by highlighting where to focus resources for the greatest commercial impact moving forward.

| Key Insights | Recommendations | Business Impact |
|---|---|---|
| Only 19.7% of Opportunity leads (867) convert to customers (171) — the single largest bottleneck in the pipeline | Address the Opportunity-to-Customer bottleneck. Investigate what separates the 83 Won deals from the 784 that did not convert. | ✓ Only 19.7% of Opportunity leads convert, representing the single largest pipeline leak.<br><br>✓ Doubling conversion to 40% would add approximately 174 customers without generating a single new lead. |
| David Wilson carries a comparable lead volume to Sarah Davis (234 vs 249) but records a 33.3% Win Rate and 3.4% Conversion Rate against her 72.7% and 6.0% | Coach David Wilson urgently using Sarah Davis as a benchmark. Both carry similar lead volumes but produce dramatically different results. | ✓ David: 33.3% Win Rate, 3.4% conversion. Sarah: 72.7% Win Rate, 6.0% conversion on a comparable lead volume.<br><br>✓ Matching Sarah's win rate would add an estimated 26 additional won deals per cycle. |
| Lost deals average $3K versus won deals at $2K — the team disproportionately loses its highest-value contracts | Develop a targeted strategy for closing high-value deals where the team currently underperforms. | ✓ Lost deals average $3K versus won deals at $2K. Larger contracts are disproportionately lost.<br><br>✓ Improving close rates on high-value deals grows Total Won Value without requiring more leads. |
| Custom solution has the highest median deal value ($1,664) but the fewest leads in the portfolio (558, 18.6%) | Grow the Custom solution pipeline with targeted prospecting and marketing investment. | ✓ Custom solution has the highest median deal value ($1,664) but fewest leads (558, 18.6% of portfolio).<br><br>✓ A 20% increase in Custom solution leads adds approximately 112 high-value opportunities to the pipeline. |
| Enterprise and Large organisations have median deal values of $13K and $12K respectively but represent only 4.4% of the total portfolio (133 leads) | Prioritise Enterprise and Large organisation leads. Small increases in this segment have outsized revenue impact. | ✓ Enterprise median deal value is $13K; Large is $12K, versus $1K for Micro and Small.<br><br>✓ Enterprise and Large are only 4.4% of the portfolio. Ten more Enterprise leads adds approximately $130K in pipeline value. |
| All 5 months show negative Close Date Variance — deals consistently close earlier than forecast, indicating systematically conservative expected close dates | Review and recalibrate expected close date forecasting methodology across the sales team. | ✓ All 5 months show negative Close Date Variance, with deals consistently closing earlier than forecast.<br><br>✓ Accurate forecasting improves pipeline planning, resource allocation, and executive reporting credibility. |

--- 

## 6. In Hindsight

This analysis covers the full scope of the CRM dataset as it was provided, spanning 3,000 leads across 9 European countries between January and May 2024. Looking back, there are genuine things I would do differently.

**5 months is not enough data.**
- You cannot draw meaningful conclusions about seasonality or pipeline trends from 5 months. In hindsight, I would have scoped the brief around what the data could actually support and been more explicit about that limitation upfront.

**The Exec Overview was built last.**
- That was the wrong order. It should have been built first, before any other page, so that every subsequent page was designed to support one clear executive story rather than assembled from whatever already existed.

**David Wilson was identified but not investigated.**
- The analysis flags him as the clearest underperformer and stops there. A stronger piece of work would have dug into what specifically separates his lost deals from Sarah Davis's won deals — by industry, product, deal value range, and organisation size. The data supports it. I just did not build it.

**Close Date Variance was treated as a team problem.**
- But it might not be. Some agents forecast accurately. Others consistently miss. I would have broken this down by agent rather than reporting it as a single team-wide finding.

**The probability values were never validated.**
- 40% was the most common probability across the entire dataset — almost certainly a default entry, not a genuine assessment. Building a $3.7M weighted forecast on top of that is a significant flaw. In a future iteration, I would build the forecast on historical close rates by stage instead.

---

## 7. Concluding notes

The interactive dashboard of this project can be viewed [here](https://bit.ly/4trOdf8).   
For any inquiries, reach out to the author through the information provided below.

---

## Author

**Kirby Phillips**

Power BI Data Analyst | 
[LinkedIn](https://www.linkedin.com/in/kirbykphillips/) | [Website](https://www.primepeakinsights.com)







