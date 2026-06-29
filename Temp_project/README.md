<div align="center">

<!-- BANNER SVG -->
<img src="assets/banner.svg" alt="India Weather Analytics Banner" width="100%"/>

<br/>

# 🌡️ India Weather Analytics Dashboard
### A Power BI Report Tracking Temperature Trends Across 96 Indian Cities (2020–2025)

<br/>

[![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Data Source](https://img.shields.io/badge/Data-Meteostat-0078D4?style=for-the-badge&logo=databricks&logoColor=white)](https://meteostat.net/)
[![Cities](https://img.shields.io/badge/Cities-96-orange?style=for-the-badge)]()
[![Records](https://img.shields.io/badge/Records-7%2C056-green?style=for-the-badge)]()
[![Period](https://img.shields.io/badge/Period-2020–2025-blueviolet?style=for-the-badge)]()

</div>

---

## 📌 Project Overview

This Power BI dashboard answers a deceptively simple question: **How much hotter or colder has India gotten?**

Using monthly weather observations for **96 of India's largest cities** spanning **6 years (January 2020 – December 2025)**, this report surfaces temperature patterns, seasonal rhythms, and city-level extremes through a single, interactive page. The design prioritises comparison — letting analysts cut across cities, years, and months in seconds.

---

## 📊 Dashboard Preview

<div align="center">
<img src="assets/dashboard-mockup.svg" alt="Dashboard Mockup" width="100%"/>
</div>

---

## 🔑 Key Metrics at a Glance

<div align="center">
<img src="assets/kpi-strip.svg" alt="KPI Metrics" width="100%"/>
</div>

| Metric | Value |
|---|---|
| 🌡️ Hottest City (avg. max temp) | **Tiruchirappalli — 34.8°C** |
| ❄️ Coldest City (avg. min temp) | **Srinagar — 8.3°C** |
| 🔥 Peak Month (all-India avg. max) | **May — 36.9°C** |
| 🧊 Coolest Month (all-India avg. max) | **January — 25.2°C** |
| 📆 Total Monthly Records | **7,056** |
| 🏙️ Cities Tracked | **96** |

---

## 📈 Visuals Included

<div align="center">
<img src="assets/visual-guide.svg" alt="Visual Types Guide" width="100%"/>
</div>

The dashboard contains **10 interactive visual types** across a single canvas:

- **Line Chart** — Monthly max temperature trend by year (2020–2025), series-split by year for multi-year comparison
- **Bar Chart** — Top 5 hottest cities ranked by average max temperature
- **Map Visual** — Bubble map of India with temperature intensity by city location
- **Table** — City performance summary with max, min, and average temperatures
- **5 KPI Cards** — Hottest city, warmest month, coldest month, max temp, min temp, avg temp, and data point count
- **3 Slicers** — Filter by date range, city name, and year
- **Decorative Icons** — Custom fire, sun, snow, temperature, calendar, and trend icons embedded as static resources

---

## 🗂️ Data Model

<div align="center">
<img src="assets/data-model.svg" alt="Data Model Diagram" width="85%"/>
</div>

The report connects to a single flat CSV (`popular_cities_weather.csv`) with the following schema:

| Column | Type | Description |
|---|---|---|
| `date` | Text (DD-MM-YYYY) | Monthly observation date |
| `tavg` | Decimal | Average temperature (°C) |
| `tmin` | Decimal | Monthly minimum temperature (°C) |
| `tmax` | Decimal | Monthly maximum temperature (°C) |
| `prcp` | Integer | Total precipitation (mm) |
| `wspd` | Decimal | Average wind speed (km/h) |
| `pres` | Decimal | Average air pressure (hPa) |
| `tsun` | Integer | Total sunshine duration (seconds) |
| `city` | Text | City name |

> **Derived columns added in Power Query:** `Year`, `Month`, `Month Number` for time-intelligence slicing.

---

## 📐 DAX Measures

Key calculated measures powering the KPI cards:

```dax
-- Hottest City Card (dynamic, respects slicers)
Hottest City Card =
VAR _MaxTemp = MAXX(ALL(weather[city]), CALCULATE(AVERAGE(weather[tmax])))
RETURN
    CALCULATE(
        SELECTEDVALUE(weather[city]),
        FILTER(ALL(weather[city]), CALCULATE(AVERAGE(weather[tmax])) = _MaxTemp)
    )

-- TMAX Card (average of max temps for selected context)
TMAX Card = AVERAGE(weather[tmax])

-- TMIN Card
TMIN Card = AVERAGE(weather[tmin])

-- Tavg Card
Tavg Card = AVERAGE(weather[tavg])

-- Warmest Month Card
Warmest Month Card =
VAR _Max = MAXX(ALL(weather[Month]), CALCULATE(AVERAGE(weather[tmax])))
RETURN
    CALCULATE(
        SELECTEDVALUE(weather[Month]),
        FILTER(ALL(weather[Month]), CALCULATE(AVERAGE(weather[tmax])) = _Max)
    )

-- Coldest Month
Coldest Month =
VAR _Min = MINX(ALL(weather[Month]), CALCULATE(AVERAGE(weather[tmin])))
RETURN
    CALCULATE(
        SELECTEDVALUE(weather[Month]),
        FILTER(ALL(weather[Month]), CALCULATE(AVERAGE(weather[tmin])) = _Min)
    )
```

---

## 🏆 Top 5 Hottest Cities

<div align="center">
<img src="assets/top5-chart.svg" alt="Top 5 Hottest Cities Chart" width="90%"/>
</div>

---

## 📅 Seasonal Temperature Pattern

<div align="center">
<img src="assets/seasonal-chart.svg" alt="Seasonal Temperature Chart" width="90%"/>
</div>

India's temperature follows a clear annual arc — peaking in **April–May** (pre-monsoon heat) and dipping to its lowest in **January**. The monsoon months (July–September) show moderate max temps due to cloud cover, despite high humidity.

---

## 🌧️ Notable Findings

- 🔆 **Tiruchirappalli (Tamil Nadu)** tops the heat index consistently, averaging **34.8°C** max across all months
- 🏔️ **Srinagar** records the coldest minimum temperatures at **8.3°C** average — over **26°C** cooler than Kozhikode's minimums
- ☀️ **Jhansi** leads in sunshine hours with an average of **16,560 seconds/month**
- 🌧️ **Mangalore** is the wettest city tracked, averaging **359.6 mm** of precipitation per month
- 📈 **2024** was the warmest year on record in this dataset with avg. tmax of **32.0°C** across all cities
- 🌡️ India's national **May average high** of **36.9°C** marks the annual temperature ceiling

---

## 🚀 How to Use

### Prerequisites
- Microsoft Power BI Desktop (any 2024+ version)
- The `popular_cities_weather.csv` file (included in `/data/`)

### Steps

**1. Clone this repository**
```bash
git clone https://github.com/YOUR_USERNAME/india-weather-analytics.git
cd india-weather-analytics
```

**2. Open the report**
```
Open Power BI Desktop → File → Open → adnantemp.pbix
```

**3. Reconnect the data source**
```
Home → Transform Data → Data Source Settings
→ Update path to /data/popular_cities_weather.csv
→ Close & Apply
```

**4. Interact**
- Use the **Date slicer** to filter by specific months or years
- Use the **City slicer** to isolate one or more cities
- Use the **Year slicer** to compare across individual years
- Click any city in the **bar chart or map** to cross-filter all visuals

---

## 📁 Repository Structure

```
india-weather-analytics/
│
├── 📊 adnantemp.pbix            ← Power BI report file
│
├── 📂 data/
│   └── popular_cities_weather.csv   ← Source dataset (7,056 records)
│
├── 📂 assets/                   ← README graphics
│   ├── banner.svg
│   ├── dashboard-mockup.svg
│   ├── kpi-strip.svg
│   ├── data-model.svg
│   ├── top5-chart.svg
│   ├── seasonal-chart.svg
│   └── visual-guide.svg
│
└── README.md
```

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Report development & DAX |
| **Power Query (M)** | Data transformation & column derivation |
| **DAX** | KPI measures, dynamic card values |
| **Meteostat** | Weather data source |
| **CSV (flat file)** | Data import format |

---

## 👤 Author

**Adnan**
Data & Business Analyst | Power BI Developer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/YOUR_PROFILE)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=for-the-badge&logo=github)](https://github.com/YOUR_USERNAME)

---

## 📄 License

This project is licensed under the MIT License. The weather dataset is sourced from [Meteostat](https://meteostat.net/) under their data usage terms.

---

<div align="center">
<sub>Built with Power BI · Data from Meteostat · 96 cities · 6 years · 7,056 records</sub>
</div>
