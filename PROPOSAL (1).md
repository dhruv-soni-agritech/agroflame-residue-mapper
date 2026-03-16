# Project Proposal — MSc (AA) Project Work 2026

**Group Name:** ***EmberGrid*
Members:** Maharshi Patel, Dhruv Soni, Tushar Vadodariya, Yash Daslaniya, Gopal Patidar, Mehul Chaudhary

**Project Working Title:** **AgroFlame: Crop Residue Burning Risk \& Bioenergy Potential Mapper — Punjab \& Haryana District-Level Analysis**



### 1\. Problem Statement



###### What is the "Spatial Problem"?

Punjab and Haryana together account for over **25 million fire events per year** during the October–November kharif harvest window and the April–May rabi harvest season — the highest agricultural burning concentration in the world. Farmers burn paddy and wheat straw in the field because they lack affordable alternatives, yet this burning generates transboundary air pollution (PM2.5, carbon monoxide) that drives the annual Delhi-NCR air quality crisis, draws Supreme Court and NGT interventions, and destroys an estimated 35 MT/year of biomass that could otherwise power compressed biogas (CBG) or 2G ethanol plants.

The core spatial gap is this: **no publicly accessible, district-level, multi-year tool currently integrates burning risk, bioenergy potential, and farmer participation viability into a single prioritisation score**. Policy agencies, energy investors, and agri-tech companies have no data-driven way to identify *which* of the 44 districts to target first for bioenergy plant installation or farmer incentive schemes.

Our tool, AgroFlame, addresses this by combining 9 years of VIIRS satellite fire data, verified state crop production statistics, and four accessible socioeconomic proxies into a **Composite Action Priority Score (CAPS)** for every district in Punjab and Haryana.

### 

###### Target User

* **Energy investors / CBG plant developers** evaluating which districts have the highest recoverable biomass
* **State government officers** (Punjab and Haryana Renewable Energy Directorates) identifying where farmer outreach is most urgent
* **Researchers and NGOs** tracking whether burning trends are improving or worsening at the district level
* **Academic instructors** demonstrating spatial data science on a high-policy-relevance dataset



### 2\. Technical Stack \& Libraries



###### GUI Framework

**Streamlit** — selected for its ability to deploy a clean, interactive dashboard without frontend code. Users will select a district from a dropdown and instantly see its full burning profile, bioenergy estimates, and priority ranking.

###### 

###### Core Geospatial Logic

* **GeoPandas** — loading GADM Level-2 district shapefile, spatial join of VIIRS fire point data to district polygons, and all geometric operations
* **Folium** — interactive choropleth maps embedded in the Streamlit dashboard (burning risk, bioenergy potential, CAPS ranking)
* **Shapely** — geometric operations for bounding-box clipping and centroid extraction per district

###### 

###### The "Advanced" Components

**ML Track — Scikit-learn:**

* KMeans (k=3) for farmer typology clustering using 4 socioeconomic features (farm income, marginal farm %, FPO count, insurance enrolment)
* StandardScaler + PCA for feature normalisation and importance weighting in the Participation Viability Index (PVI)
* XGBoost regressor for bioenergy potential estimation and feature importance ranking

**Web \& API Track — Requests:**

* **NASA FIRMS API** — fetching VIIRS 375m active fire CSV data for South Asia (2015–2023), no login required
* **NASA POWER API** — fetching daily weather data (rainfall, temperature, humidity, wind speed) per district centroid via requests.get() calls to *power.larc.nasa.gov/api/temporal/daily/point*
* **pymannkendall** — Mann-Kendall statistical trend test on the 9-year per-district fire count time series to detect statistically significant burning increase or decrease
* 

### Data Sources

|#|Dataset|Source / URL|Format|Use|
|-|-|-|-|-|
|1|VIIRS 375m Active Fire (S-NPP + NOAA-20), 2015–2023|firms.modaps.eosdis.nasa.gov/download|CSV|Burning Risk Module|
|2|Punjab Statistical Abstract (District Crop Production)|esopb.gov.in|Excel|Bioenergy Module|
|3|Haryana Statistical Abstract (District Crop Production)|esaharyana.gov.in|Excel|Bioenergy Module|
|4|ICAR/MNRE Biomass Atlas (RPR + LHV Coefficients)|mnre.gov.in/biomass-power|PDF → lookup table|Bioenergy Module|
|5|NASA POWER Daily Weather API|power.larc.nasa.gov|JSON via API|Weather features|
|6|NSSO 77th Round — Situation Assessment of Agricultural Households 2019|mospi.gov.in|PDF + data tables|Participation Module|
|7|Agricultural Census 2015–16 (Operational Holdings by Size)|agcensus.dacfw.nic.in|PDF tables|Participation Module|
|8|FPO Registered District Data|data.gov.in|CSV/Excel|Participation Module|
|9|PMFBY District Insurance Enrolment Reports|pmfby.gov.in|Excel|Participation Module|
|10|GADM India District Shapefile (Level 2)|gadm.org/download\_country.html|GeoJSON / SHP|All spatial joins|
|11|FAO FAOSTAT India State Crop Production (backup validation)|faostat.fao.org|CSV|Validation|

## 

### 3\. Proposed GUI Architecture



###### Input Section

* **District Dropdown** — Select any of the 44 Punjab/Haryana districts from a searchable st.selectbox
* **Year Range Slider** — st.slider to filter the 9-year fire time series (2015–2023)
* **Weight Customiser (Advanced)** — Three st.number\_input fields to manually adjust BRS / BPS / PVI weights in the CAPS formula (defaults: 35%, 40%, 25%)



###### Processing Section

When the user selects a district and hits **"Generate Profile"**:

1. Filters the fire dataframe to the selected district and year range
2. Pulls the precomputed BRS, BPS, PVI, and CAPS scores from the master district dataframe
3. Renders the district polygon highlighted on the Folium interactive map
4. Computes bioenergy revenue potential in ₹ crore/year dynamically



###### Output / Visualisation

* **Folium Choropleth Map** — full 44-district view with colour gradient by CAPS score; selected district highlighted with popup
* **Fire Trend Line Chart** — 9-year fire count time series for the selected district, with Mann-Kendall trend arrow (increasing / decreasing / no trend)
* **Bioenergy Bar Chart** — rice straw vs wheat straw recoverable biomass and ₹ revenue potential
* **Score Card Panel** — BRS, BPS, PVI, CAPS displayed as st.metric cards with delta vs state average
* **Policy Recommendation Box** — rule-based text output classifying the district as: Priority Investment Zone / Policy Intervention Zone / Monitoring Zone



### 4\. GitHub Repository Setup



**Repo URL:** *https://github.com/dhruvhokgames-web/agroflame-residue-mapper*

**Initial Folder Structure (confirmed):**


agroflame-residue-mapper/
├── data/
│   ├── fire/            # VIIRS CSV files (2015–2023, filtered to Punjab+Haryana)
│   ├── crop/            # Punjab + Haryana Statistical Abstract Excel files
│   ├── socioeconomic/   # NSSO, Agri Census, FPO, PMFBY cleaned CSVs
│   └── spatial/         # GADM Level-2 GeoJSON + OSM Shapefiles
├── docs/
│   ├── writeup.pdf      # 3–5 page project report
│   └── architecture.png # System architecture diagram
├── src/
│   ├── gui/
│   │   └── dashboard.py     # Streamlit app layout and event handling
│   ├── logic/
│   │   ├── burning\_risk.py   # Module A: VIIRS processing, BRS computation
│   │   ├── bioenergy.py      # Module B: Residue estimation, GEP/NBP/revenue
│   │   ├── participation.py  # Module C: K-Means clustering, PVI scoring
│   │   └── caps.py           # Module D: Composite score integration
│   └── utils/
│       ├── api\_fetcher.py    # NASA POWER API calls per district centroid
│       ├── spatial\_utils.py  # GeoPandas helpers, spatial join functions
│       └── preprocessor.py   # Data cleaning, normalisation, merging
├── main.py              # Entry point: launches Streamlit dashboard
├── requirements.txt     # All pip dependencies
└── PROPOSAL.md          # This file


### 5\. Preliminary Task Distribution

|Member|Primary Responsibility|Secondary Responsibility|
|-|-|-|
|Member 1|**Module A — Burning Risk** (VIIRS fire data pipeline, spatial join to districts, Mann-Kendall trend test, BRS computation)|Data download \& folder setup|
|Member 2|**Module B — Bioenergy Potential** (Statistical Abstract Excel cleaning, RPR/LHV coefficient application, GEP/NBP/revenue calculation)|Validation against PAU/HAU published estimates|
|Member 3|**Module C — Participation Viability** (NSSO + Agri Census + FPO + PMFBY data cleaning, K-Means clustering, PVI scoring)|Documentation \& README.md|
|Member 4|**Module D — CAPS Integration** (combining BRS + BPS + PVI, district ranking, Top-10 identification, policy zone classification)|Presentation visuals|
|Member 5|**GUI — Streamlit Dashboard** (dropdown, sliders, Folium map integration, score card panels, policy recommendation box)|UI/UX testing across screen sizes|
|Member 6|**API Integration \& System Lead** (NASA POWER API fetcher, api\_fetcher.py, Git repository management, `requirements.txt`, integration testing)|Code review \& final submission|



### 6\. Submission Instructions



*As per project guidelines:*

1. This file is saved as `PROPOSAL.md` in the root of the GitHub repository.
2. Instructor `prasunkgupta@gmail.com` has been added as a collaborator via **Settings → Collaborators → Add people → "prasunkgupta"**.



*Proposal submitted: March 16, 2026*

