# Crop Residue Burning & Bioenergy Dashboard

**Punjab & Haryana \| 44 Districts \| 2015--2023 VIIRS Fire Data**

[![Streamlit
App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://your-app-url.streamlit.app)

------------------------------------------------------------------------

## 🔗 Live Dashboard

👉 [Launch Dashboard](https://your-app-url.streamlit.app)\
*(Replace with your actual Streamlit link)*

------------------------------------------------------------------------

## 📌 What This Project Does

This project builds a **complete spatial + machine learning pipeline**
to identify **priority districts for Compressed Biogas (CBG) plant
investment** using:

-   Satellite fire data (VIIRS)
-   Crop production statistics
-   Environmental variables

------------------------------------------------------------------------

## 📊 Three Analytical Modules

  -----------------------------------------------------------------------
  Module                Notebooks                   Output
  --------------------- --------------------------- ---------------------
  **A --- Fire Risk**   NB01--07                    Burning Risk Score
                                                    (BRS 0--100) using
                                                    fire frequency +
                                                    FRP + Mann-Kendall
                                                    trend

  **B --- Bioenergy**   NB08--10                    Bioenergy Potential
                                                    Score (BPS 0--100) +
                                                    ₹ revenue/year

  **C ---               NB11--16                    K-Means zones +
  Environmental**                                   PCA-based Burning
  *(revised)*                                       Severity Index (BSI
                                                    0--100)
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 🎯 Final Decision Logic

BSI ≥ 70th percentile AND Residue ≥ 70th percentile → Plant Zone\
BSI ≥ 70th percentile only → Policy Zone\
Below thresholds → Low Priority

------------------------------------------------------------------------

## 📁 Repository Structure

crop-residue-bioenergy-dashboard/ │ ├── app/ │ └── dashboard.py ├──
data/ │ └── processed/ ├── outputs/ │ └── maps/ ├── requirements.txt ├──
.gitignore └── README.md

------------------------------------------------------------------------

## 💻 Local Setup

git clone
https://github.com/YOUR_USERNAME/crop-residue-bioenergy-dashboard.git\
cd crop-residue-bioenergy-dashboard\
pip install -r requirements.txt\
streamlit run app/dashboard.py

------------------------------------------------------------------------

## 🛠️ Tech Stack

-   NASA FIRMS VIIRS\
-   GeoPandas\
-   scikit-learn\
-   Plotly\
-   Streamlit

------------------------------------------------------------------------

## 🎓 Academic Context

M.Sc. Semester 2 Research Project \| Dehradun, India
