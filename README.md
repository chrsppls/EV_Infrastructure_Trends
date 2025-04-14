# Maryland EV Infrastructure Analysis

## Project Overview

This project performs an in-depth analysis of Electric Vehicle (EV) infrastructure and adoption trends, focusing primarily on Maryland, with specific attention to Baltimore. It involves collecting data from various sources, preparing and cleaning the data, performing exploratory analysis, forecasting future trends using statistical and machine learning models, and visualizing the results. The analysis integrates economic indicators (GDP), population data, vehicle registrations, fuel prices, electricity prices, and detailed EV charging station information.

## Data Science Aspects

### 1. Data Acquisition & ETL (Extract, Transform, Load)

Multiple data sources were utilized, requiring diverse acquisition methods:

* **APIs:**
    * Bureau of Economic Analysis (BEA) API for state-level GDP data.
    * U.S. Census Bureau API for state population data (historical and forecasting).
    * National Renewable Energy Laboratory (NREL) API for EV charging station data across the US.
    * Energy Information Administration (EIA) API for fuel and electricity price data.
    * Socrata API (Maryland Open Data) for Maryland-specific vehicle registration data (total, EV by county, EV by zip).
    * St. Louis Federal Reserve (FRED) API for US vehicle sales data.
* **Web Scraping:**
    * BeautifulSoup was used to scrape US vehicle registration data by type from the Alternative Fuels Data Center (AFDC) website.
* **File Handling:**
    * Reading GeoJSON files for Maryland county and Baltimore neighborhood boundaries.
    * Reading data from local Excel files (e.g., US EV Registration data manually downloaded from AFDC).
* **Data Transformation:**
    * Pivoting tables (e.g., GDP and population data) for time-series analysis.
    * Handling missing values (NaN replacement).
    * Type conversions (e.g., string to numeric, string to datetime).
    * Calculating Month-over-Month changes for registration data.
    * Merging and joining different datasets (e.g., combining registration data with fuel prices).

### 2. Geospatial Analysis

* **Coordinate Processing:** Extracting coordinates from GeoJSON files for Baltimore neighborhoods.
* **Point-in-Polygon:** Determining which neighborhood each Baltimore EV charging station belongs to using Shapely library functions (`Point`, `Polygon`, `contains`).
* **Centroid and Area Calculation:** Calculating the centroid and area for each neighborhood polygon.
* **Mapping:** Although not explicitly shown in the final output code, the import of `folium` suggests map visualizations were likely created.

### 3. Machine Learning Utilization & Forecasting

This project heavily relies on time series forecasting models, which are a key application area within machine learning and statistical modeling for predictive tasks.

* **Model Exploration & Preparation:**
    * The analysis involved standard time series preparation steps like checking for stationarity using the Augmented Dickey-Fuller (ADF) test, assessing multicollinearity via correlation matrices and Variance Inflation Factor (VIF), and applying differencing techniques to achieve stationarity before modeling.
* **Forecasting Models:**
    * **ARIMA (AutoRegressive Integrated Moving Average):** Used for forecasting US state population based on historical Census data.
    * **SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors):** This was the primary model employed for forecasting Maryland's EV and Plug-in Hybrid vehicle registrations. It was used both for baseline forecasting (assuming continuation of past trends) and for scenario analysis by incorporating potential policy impacts as exogenous variables (`exog`).
    * **VAR (Vector Autoregression):** Explored for modeling the relationships and forecasting multiple time series simultaneously (EV registrations, Plugin Hybrid registrations, fuel prices, electricity prices) after differencing.
    * **VECM (Vector Error Correction Model):** Initial exploration included checking for cointegration among time series variables and fitting a VECM.
* **Regression Analysis:**
    * **OLS (Ordinary Least Squares):** An attempt was made to use OLS regression to model the impact of various factors (tax benefits, charger density, income, prices) on EV market share, although the final application might differ.
* **Clustering (Potential):**
    * `KMeans` from `sklearn.cluster` was imported, indicating a potential, though perhaps unused, exploration of clustering techniques, possibly for segmenting charging stations or neighborhoods.

### 4. Data Visualization

* **Libraries:** Matplotlib and Seaborn were used extensively.
* **Plot Types:**
    * Bar charts (e.g., chargers added per year, cumulative chargers, forecast comparisons).
    * Line plots (e.g., historical vs. forecasted registrations, differenced series).
    * Heatmaps (correlation matrix).
    * Boxplots.
* **Customization:** Plots included titles, labels, legends, CAGR annotations, date formatting on axes, and subplot arrangements using GridSpec.

## Key Libraries Used

* `pandas`: Data manipulation and analysis.
* `numpy`: Numerical operations.
* `requests`: Fetching data from APIs and web pages.
* `BeautifulSoup4` (`bs4`): Web scraping.
* `sodapy`: Interacting with Socrata Open Data APIs.
* `json`: Handling JSON data.
* `shapely`: Geospatial analysis (points, polygons).
* `statsmodels`: Time series analysis (ARIMA, SARIMAX, VECM, VAR, ADF test, VIF, OLS).
* `sklearn.cluster` (`KMeans`): Clustering algorithm (imported).
* `matplotlib`: Plotting and visualization.
* `seaborn`: Enhanced data visualization.
* `geopy`: Geocoding (imported).
* `area`: Calculating polygon area (imported).
* `geographiclib`: Calculating polygon area.

## How to Run

1.  **Dependencies:** Ensure all imported libraries listed above are installed (`pip install pandas numpy requests beautifulsoup4 sodapy shapely statsmodels scikit-learn matplotlib seaborn lxml geographiclib area geopy openpyxl`).
2.  **API Keys:** Obtain necessary API keys (BEA, Census, NREL, FRED) and store them appropriately (e.g., in a `constants.py` file as suggested by the import `from constants import *`).
3.  **Data Files:**
    * Place the required GeoJSON files (`maryland-counties.geojson`, `baltimore.geojson`) in the same directory or update paths. These seem to be downloaded directly from URLs in the script.
    * Ensure the `US_EV_Registration_Data.xlsx` file is present in the specified `data` subdirectory.
4.  **Execution:** Run the Python script (`.py`) or execute the cells sequentially in a Jupyter Notebook environment. Ensure the output directory (`../data/` relative to the notebook) exists or is created.