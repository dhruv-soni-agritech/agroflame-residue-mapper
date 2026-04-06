Crop Residue Burning & Bioenergy Dashboard
Punjab & Haryana | 44 Districts | 2015--2023 VIIRS Fire Data
[![Streamlit](https://s9fczme59zkhxbeiend3ye.streamlit.app/)](https://crop-residue.streamlit.app/)
---
🔗 Live Dashboard
👉 Launch Dashboard
Explore an interactive dashboard to analyze fire risk, bioenergy
potential, and environmental stress zones across districts.
---
📌 Project Overview
This project develops a comprehensive spatial + machine learning
pipeline to identify optimal districts for Compressed Biogas (CBG)
plant installation in Punjab and Haryana.
Crop residue burning is a major contributor to seasonal air pollution in
North India. Instead of burning, this biomass can be converted into
bioenergy (CBG). This project helps in:
Identifying high-risk burning districts
Estimating bioenergy potential
Mapping priority zones for intervention
---
⚙️ Methodology
The workflow integrates remote sensing, statistical analysis, and
machine learning:
1. Fire Data Processing
Source: NASA FIRMS VIIRS (2015--2023)
Extract fire count, fire radiative power (FRP), and seasonal
patterns
2. Burning Risk Score (BRS)
Combines:
Fire frequency (40%)
Fire intensity (30%)
Trend (30%) using Mann-Kendall test
Output: BRS (0--100)
3. Bioenergy Potential (BPS)
Uses crop production data
Applies:
Residue-to-product ratio (RPR)
Burn fraction
Recovery efficiency (~70%)
Converts biomass → energy → CBG → revenue
4. Environmental Analysis (BSI)
Features:
fire_count
residue
avg_temp
rainfall (inverted)
PCA used to derive weights
Output: Burning Severity Index (BSI 0--100)
5. Clustering (K-Means)
Groups districts into zones:
High Stress / High Opportunity
Moderate
Low Priority
6. Final Decision Logic
Combines BSI and residue thresholds:
```{=html}
<!-- -->
```
    BSI ≥ 70th percentile AND Residue ≥ 70th percentile → Plant Zone  
BSI ≥ 70th percentile only → Policy Zone  
Else → Low Priority

---
📊 Key Insights
High BSI districts indicate high biomass availability
Top districts show strong potential for CBG production
PCA confirms environmental variables capture major variance
Only a few districts show statistically significant fire trends
---
📁 Repository Structure
    crop-residue-bioenergy-dashboard/
│
├── app/
│   └── dashboard.py
│
├── data/
│   └── processed/
│       ├── burning_risk_scores.csv
│       ├── bioenergy_scores.csv
│       ├── bsi_scores.csv
│       ├── district_clusters.csv
│       ├── env_features.csv
│       ├── fire_stats.csv
│       └── fire_trends.csv
│
├── outputs/
│   └── maps/
│
├── requirements.txt
├── .gitignore
└── README.md

---
💻 Local Setup
``` bash
git clone https://github.com/YOUR\_USERNAME/crop-residue-bioenergy-dashboard.git
cd crop-residue-bioenergy-dashboard
pip install -r requirements.txt
streamlit run app/dashboard.py
```
---
🚀 Deployment
Deployed using Streamlit Cloud for easy access and visualization.
---
🛠️ Tech Stack
NASA FIRMS VIIRS\
GeoPandas\
pandas, numpy\
scikit-learn\
pymannkendall\
Plotly\
Streamlit
---
📚 Data Sources
NASA FIRMS (Fire Data)
GADM (District Boundaries)
Punjab & Haryana Statistical Abstracts
NASA POWER (Weather Data)
MNRE / ICAR (RPR values)
---
🎓 Academic Context
M.Sc. Semester 2 Research Project | Dehradun, India
This project demonstrates how satellite data and machine learning can be
combined to create actionable insights for sustainable energy and
environmental management.
