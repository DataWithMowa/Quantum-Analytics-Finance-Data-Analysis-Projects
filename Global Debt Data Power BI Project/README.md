# Global Debt Data Dashboard

<img width="942" height="463" alt="Global Data Dasboard" src="https://github.com/user-attachments/assets/0a315b38-165b-47e3-8755-30ca45b84c27" />

### 📊 Project Overview

**How much does the world actually owe — and who owes the most?**

This project analyzes 24,399 debt records across 190 countries and 5 debt types, spanning 1950 to 2022. The goal was to understand how global debt has shifted over seven decades, which debt type carries the heaviest load, and which countries sit furthest outside the norm.

#### The Approach

Using Power BI, I combined five separate debt datasets — Central Government, General Government, Household, Non-Financial Corporate, and Private debt — into a single table, cleaned the country names, built KPI measures, and put together an interactive dashboard to explore how debt levels move across countries, debt types, and decades.

### 🕹️ Interactive Dashboard Demo

See the dashboard filters, dynamic metrics, and charts in action below (42-second walkthrough):

https://github.com/user-attachments/assets/92821202-99bb-4e30-9f81-3cbd7974ebd3

### To see the full live analysis: [Click here](https://lnkd.in/p/e3VCX8e7)

### 💡 Key Data Insights & Discoveries

* The global average debt-to-GDP ratio across all countries, debt types, and years is 51.6%.
* **Non-Financial Corporate debt carries the heaviest average load at 67.4%** — well ahead of Private debt (54.6%), General Government debt (52.5%), Central Government debt (47.9%), and Household debt, the lowest at 37.3%.
* Global average debt has climbed sharply since the 1970s — from 29.0% in 1970, to 55.0% by 1990, dipping slightly around the 2008 financial crisis (53.8%), then spiking to 70.6% in 2020 as the pandemic hit.
* At the country level, a small group of nations carry extreme debt-to-GDP ratios well above 100% — Eritrea (177.2%), Liberia (169.8%), and Sudan (156.5%) top the list, though each is based on a smaller number of recorded years than most other countries.

### 🛠️ Power BI Skills & Dashboard Setup

To turn five separate debt datasets into one clear, comparable picture, I used Power BI's data cleaning, data modeling, DAX, and dashboard features:

* **Data Cleaning & Preparation (Power Query):** Loaded five CSV files — `central_government_debt`, `general_government_debt`, `household_debt`, `non-financial_corporate_debt`, and `private_debt` — each with years stored as columns (1950–2022, wide format).
  * **Unpivoted** the year columns in each file, turning them into two columns, `Years` and `Debt %`, so every country-year combination became its own row
  * Added a **Debt Type** column to each file (e.g. "Central Government"), tagging every row before combining
  * Fixed corrupted characters in country names caused by encoding issues — e.g. "Türkiye", "São Tomé and Príncipe", "Côte d'Ivoire" — and trimmed and cleaned extra whitespace from `country_name`
  * **Combined all five tables** into one long `Debt Data` table using `Table.Combine`

<img width="1582" height="726" alt="Screenshot 2026-10-02 142843" src="https://github.com/user-attachments/assets/a89ff2c1-ebdf-4487-b0c8-9b30a5673993" />

* **Data Modeling:** Built a small star schema around the combined `Debt Data` table: a `Countries` dimension table and a `YEARTABLE` date dimension, both connected with one-to-many relationships, so every chart can slice debt by country or by year.

<img width="1534" height="725" alt="Screenshot 2026-10-02 143120" src="https://github.com/user-attachments/assets/30c72cd5-23dd-4496-bac0-1a7f3990abeb" />

* **DAX Measures:** Kept all measures in one dedicated `Measures (2)` table:

| Measure | What it does | DAX |
|---|---|---|
| **Countries Covered** | Counts every distinct country in the dataset | `DISTINCTCOUNT('Debt Data'[country_name])` |
| **Global AVG Debt %** | Average debt-to-GDP ratio across all rows | `AVERAGE('Debt Data'[Debt %])` |
| **Top Country** | Ranks countries by average debt %, returns the highest | `VAR CountryAverages = ADDCOLUMNS(VALUES('Debt Data'[country_name]), "AvgDebt", CALCULATE(AVERAGE('Debt Data'[Debt %]))) VAR TopCountry = TOPN(1, CountryAverages, [AvgDebt], DESC) RETURN MAXX(TopCountry, 'Debt Data'[country_name])` |
| **Highest AVG Debt Year** | Intended to return the year with the highest average debt % | `VAR YearAverages = ADDCOLUMNS(VALUES('Debt Data'[Years]), "AvgDebt", CALCULATE(AVERAGE('Debt Data'[Years]))) VAR TopYear = TOPN(1, YearAverages, [AvgDebt], DESC) RETURN MAXX(TopYear, 'Debt Data'[Years])` |

<img width="360" height="196" alt="Screenshot 2026-10-02 143258" src="https://github.com/user-attachments/assets/c1af679c-e331-4403-bb55-c75e57ffb4e3" />

* **Key Metric Tracking:** Created KPI cards to highlight the main numbers, including **Countries Covered (190), Global AVG Debt % (51.6%), and Top Country by Average Debt (Eritrea).**

<img width="1005" height="97" alt="Screenshot 2026-10-02 143340" src="https://github.com/user-attachments/assets/3701a126-6bd0-4705-88f5-e9522f0de71c" />

* **Chart Analysis:** Built visuals to compare average debt % by debt type, trace the global average debt trend from 1950 to 2022, and rank countries by average debt-to-GDP ratio.

*It's all in the dashboard image*

* **Interactive Slicers:** Added slicers for **Country** and **Years**, so users can filter the whole dashboard down to a specific country or time period.

<img width="330" height="160" alt="Screenshot 2026-10-02 143558" src="https://github.com/user-attachments/assets/17c8fedc-b78d-4f9c-8870-cebe8f491802" />

### 📈 Strategic Recommendations & Next Steps

* Flag Non-Financial Corporate debt as the segment needing the closest monitoring — it carries the highest average debt-to-GDP ratio of all five categories.
* Use 2008 and 2020 as reference points for stress-testing future debt scenarios, since both periods show clear, data-backed spikes tied to real economic shocks.
* Treat the "highest debt" country rankings with caution when a country has very few recorded years — a small number of data points can push an average higher than it would be with full historical coverage.
* Fix the **Highest AVG Debt Year** measure, which currently averages the Years column itself instead of ranking years by their average Debt %, so it doesn't yet return a meaningful result.

### 📂 How to Open and Explore the Dashboard

1. You can download the full file here: [Global_Debt_Data_Dashboard.pbix](https://github.com/DataWithMowa/Quantum-Analytics-Finance-Data-Analysis-Projects/tree/main/Global%20Debt%20Data%20Power%20BI%20Project/Full%20Project)
2. Open the file locally using Power BI Desktop.
3. Go to the Dashboard page.
4. Use the Country, Debt Type and Years slicers to filter all the charts by a specific country, Dent type or time period.
5. Hover over the debt-by-type chart to compare how Government, Corporate, Household, and Private debt differ.

### 🤝 Connect & Support

Thank you for taking the time to go through this project! If you have any questions or feedback, please reach out directly:

* 💼 **LinkedIn:** [Mowaninuola Umarudeen](https://www.linkedin.com/in/mowaninuolaumarudeen/)
* 📧 **Email:** [mowatheanalyst@gmail.com](mailto:mowatheanalyst@gmail.com)

*📈 **Did you find this useful?** Consider giving this repository a ⭐ **Star** if it helped you!*
