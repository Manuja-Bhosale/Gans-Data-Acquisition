
# 🛴 Gans ETL Pipeline (Data Acquisition)


![Python](https://img.shields.io/badge/Python-3.11-blue?style=flat-square)

![Pandas](https://img.shields.io/badge/Pandas-2.0-orange?style=flat-square)

![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat&logo=mysql&logoColor=white)

An end-to-end ETL (Extract → Transform → Load) data pipeline built to help the fictional e-scooter startup Gans predict where its scooters need to be, by collecting and storing city, weather, and flight data from three external sources.
---

**🧠 The Problem**
E-scooter sharing companies live or die by scooter placement. 

**Demand is asymmetric:**

🏔️ Users ride uphill, walk downhill

☀️ Morning commuters flow from residential areas → city centres

🌧️ Rain drastically reduces ridership

✈️ Tourist arrivals drive demand near landmarks and airports

This pipeline builds the data foundation for future predictive modelling by continuously collecting the signals that matter.
---

**🔁 Pipeline Overview**

```
Wikipedia (scrape) ──┐
OpenWeatherMap (API) ─┼──▶ Pandas Transform ──▶  MySQL
AeroDataBox (API)  ──┘
```
---
**📦 Data Sources**

Source	Method	Data Collected

Wikipedia	Web Scraping (BeautifulSoup4)	City population, coordinates, country

OpenWeatherMap	REST API (free tier)	Current weather + 5-day forecast

AeroDataBox	REST API (RapidAPI)	Upcoming flight arrivals per airport

---

**🗄️ Database Schema**
Four tables designed for append-only operation (historical rows are never deleted):

`cities` — static city metadata (population, lat/lon, country)

`weather` — current weather snapshots per city

`weather\\\\\\\_forecast` — 5-day / 3-hour forecast entries

`flights` — upcoming arrivals at each city's main airport

---

**🗂️ Project Structure**
```
gans-etl/
├── main.py                   # Pipeline orchestrator
├── requirements.txt
├── .env.example              # Credential template (safe to commit)
├── scraping/
│   └── cities\\\\\\\_scraper.py     # Wikipedia → city metadata
├── apis/
│   ├── weather\\\\\\\_api.py        # OpenWeatherMap current + forecast
│   └── flights\\\\\\\_api.py        # AeroDataBox arrivals
└── database/
    └── database.py           # SQLAlchemy schema + loaders
```
---
**🧩 Skills Demonstrated**

`Web Scraping` ·

`REST APIs` ·

`Pandas ETL` ·

`SQL Schema Design` ·

`Error Handling` ·

`Environment Config` ·

`Data Modelling` · 

`Pipeline Orchestration`

---

**🔧 Challenges & Solutions**

Inconsistent Wikipedia infoboxes — markup varies city-to-city. Solved with defensive parsing that skips missing fields gracefully.

API rate limits & failures — wrapped every city in try/except so one bad response never aborts the entire run.

Historical data preservation — used `if\\\\\\\_exists="append"` throughout so every pipeline run adds new rows, enabling time-series analysis later.

Dual database support — identical codebase runs on SQLite locally (zero setup) and MySQL in production, controlled by a single env var.

---
**📈 Next Steps**

[ ] Schedule with cron / Airflow for continuous collection

[ ] Add neighbourhood-level scooter position data

[ ] Build predictive model on top of collected features

[ ] Deploy to cloud (AWS RDS + Lambda / GCP)

---
Built as part of the WBS Coding School Data Engineering curriculum.
