# 🦟 Malaria Surveillance Dashboard — Nigeria (2020–2023)

An interactive two-page Power BI dashboard analyzing four years of quarterly malaria surveillance data across all 36 Nigerian states and the Federal Capital Territory. The project covers the full analytics workflow — data cleaning, DAX measure development, dashboard design, and insight extraction — using a synthetic dataset built for capstone/portfolio purposes.



![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)




![Power Query](https://img.shields.io/badge/Power%20Query-217346?style=flat&logo=microsoftexcel&logoColor=white)




![Status](https://img.shields.io/badge/status-complete-brightgreen)



---

## 📊 Dashboard Preview

### Page 1 — National Overview


![Page 1 - National Overview](./Screenshot%20page%201.png)



### Page 2 — Geographic & Data Quality


![Page 2 - Geographic Data Quality](./Screenshot%20frame%202.png)
---

## 🎯 Project Overview

This project analyzes 600 rows of quarterly malaria data (37 states × 4 years × 4 quarters, plus a partial 2024 Q1 sample) to answer:

- How has malaria burden changed nationally from 2020–2023?
- Which states and zones carry the highest burden?
- Does ITN (insecticide-treated net) coverage actually correlate with lower incidence?
- What role does seasonality/rainfall play?
- How much of the burden falls on children under 5 and pregnant women?
- Where are the data-quality gaps in reporting?

---

## 🧹 Data Cleaning (Power Query)

- Verified completeness (0 missing values) and uniqueness (0 duplicates) across all 14 original columns
- Trimmed whitespace and standardized text fields (State, Geopolitical_Zone, Quarter, Quarter_Months)
- Corrected data types (Whole Number for counts, Decimal for rates/percentages)
- Added a Quarter_Num column so Q1–Q4 sort chronologically, not alphabetically
- Merged FCT into North Central via Table.ReplaceValue, matching Nigeria's official 6-zone structure (confirmed: 6 distinct values, 0% errors)
- Validated logical integrity (deaths never exceed cases; all percentages fall within 0–100%)
- Identified and excluded incomplete 2024 data (8 of 37 states, Q1 only) from all KPI totals and trends
- Added a Burden_Tier column (Low / Moderate / High / Very High) based on incidence thresholds

---

## 🧮 DAX Measures

Total Cases = SUM(Malaria_Data[Reported_Malaria_Cases])

Total Population = 
CALCULATE(
    SUM(Malaria_Data[Estimated_Population]),
    FILTER(
        Malaria_Data,
        Malaria_Data[Year] = 2023 && Malaria_Data[Quarter] = "Q4"
    )
)

Cases per 100k = 
DIVIDE(
    SUM(Malaria_Data[Reported_Malaria_Cases]),
    SUM(Malaria_Data[Estimated_Population]),
    0
) * 100000

Avg Incident Rate = AVERAGE(Malaria_Data[Incidence_Rate_per_1000])

Avg ITN Coverage % = AVERAGE(Malaria_Data[ITN_Coverage_Pct])

Under-5 Cases Share = AVERAGE(Malaria_Data[Under5_Cases_Pct])

Cases Fatality Rate % = 
DIVIDE(
    SUM(Malaria_Data[Malaria_Deaths]),
    SUM(Malaria_Data[Reported_Malaria_Cases]),
    0
) * 100

Design principle: percentage/rate fields always use AVERAGE, never SUM (summing a rate produces a meaningless inflated number). Fields affected by row repetition across quarters (like population) use a filtered CALCULATE to avoid duplication.

---

## 🔍 Key Findings

| Finding | Detail |
|---|---|
| Burden is declining | National incidence fell 15.2%, from 45.5 to 38.6 per 1,000 (2020→2023) |
| ITN coverage rose, but correlation is weak | Coverage climbed 50.8%→67.7%, yet correlates with incidence at only r = −0.09 |
| Rainfall is a stronger driver | Rainfall correlates with incidence at r = 0.53 — Q3 (rainy season) incidence is ~68% higher than Q1 |
| Burden concentrates in the South | South South & South East average >55/1,000, more than double North Central/FCT |
| Vulnerable groups carry ~50% of all cases | Under-5s: 37.8% avg share · Pregnant women: 12.0% avg share — both statistically flat over time |
| Reporting quality varies by state | Cross River, Imo, and Jigawa show the lowest health facility reporting rates |
| Case volume and deaths scale together | Lagos leads a near-linear trend, consistent with a flat 0.27% national case fatality rate |

Full findings, methodology, and limitations are documented in /docs/Malaria_Dashboard_Documentation.pdf.

---

## 📁 Repository Structure

├── Malaria_Trend_on_Powerbi.pbix          # Power BI dashboard file
├── data/
│   └── Malaria_Nigeria_CLEANED.xlsx        # Cleaned dataset + summary sheets
├── docs/
│   └── Malaria_Dashboard_Documentation.pdf # Full write-up: cleaning, DAX, findings
├── screenshots/
│   ├── page1_national_overview.png
│   └── page2_geographic_quality.png
└── README.md

---

## ⚠️ Limitations

- Dataset is synthetic/illustrative, built for analytics practice — not sourced from NMEP, NHMIS/DHIS2, or NDHS real-world systems
- 2024 data is partial (8 of 37 states, Q1 only) and excluded from all totals
- Under-5 and pregnant-women case counts are derived estimates (percentage × reported cases), not independently verified sub-group counts

---

## 🛠️ Tools Used

Power BI Desktop · Power Query (M) · DAX

---

## 👤 Author

Bello Tijani Olarewaju (Teejay)
[LinkedIn](https://www.linkedin.com/in/bello-tijani-668535210)
