# 🌦️ WeatherAUS – July Rainfall Analysis

An Exploratory Data Analysis (EDA) and data cleaning project on the well-known Australian weather dataset **weatherAUS.csv**, with a specific focus on **July** data, plus an interactive dashboard built with **Dash / Plotly**.

---

## 📋 Table of Contents
- [Overview](#-overview)
- [Dataset](#-dataset)
- [Analysis Steps](#-analysis-steps)
- [Libraries Used](#-libraries-used)
- [How to Run](#-how-to-run)
- [Project Structure](#-project-structure)
- [Results & Notes](#-results--notes)
- [Future Improvements](#-future-improvements)

---

## 🎯 Overview

This notebook performs a full analysis of Australian weather data (`weatherAUS.csv`), focusing specifically on **July** records in order to study rainfall patterns for that month across different years and different locations.

The project covers:
- Data cleaning and intelligent handling of missing values, based on `Location` grouping and related feature grouping.
- Correlation analysis between numerical variables.
- Multiple visualizations (histograms, heatmaps, line plots, bar plots).
- A simple interactive dashboard built with Dash.

---

## 📊 Dataset

- **Source:** [Rain in Australia – Kaggle](https://www.kaggle.com/datasets/jsphyg/weather-dataset-rattle-package)
- **File:** `weatherAUS.csv`
- **Description:** Daily weather observations from multiple Australian weather stations, including columns such as:
  - `MinTemp`, `MaxTemp`, `Rainfall`, `Evaporation`, `Sunshine`
  - `WindGustDir`, `WindGustSpeed`, `WindDir9am`, `WindDir3pm`
  - `WindSpeed9am`, `WindSpeed3pm`, `Humidity9am`, `Humidity3pm`
  - `Pressure9am`, `Pressure3pm`, `Cloud9am`, `Cloud3pm`
  - `Temp9am`, `Temp3pm`, `RainToday`, `RainTomorrow`, `Location`, `Date`

> ⚠️ **Important note:** the file read/save paths in the current notebook are hard-coded local Windows paths, e.g.:
> ```python
> df = pd.read_csv(r"D:\new\OneDrive\Desktop\New folder (2)\weatherAUS.csv")
> ```
> These need to be changed to relative paths (e.g. `data/weatherAUS.csv`) before the project is uploaded or run by anyone else.

---

## 🔍 Analysis Steps

### 1. Preprocessing
- Convert the `Date` column to `datetime` and extract `year`, `month`, `day`, and `day_of_week`.
- Filter the data down to **July** only (`month == 7`) into a separate DataFrame (`df2`).

### 2. Missing Values Inspection
- Visualize the count of missing values per column using a bar plot and the `missingno` library.

### 3. Missing Value Imputation – Multi-level Fallback Strategy
A progressively refined imputation strategy was used, from most specific to most general:
1. **Numerical columns** (`MinTemp`, `Humidity`, `Pressure`, ...): filled with the **median** per `Location` first, then the global median for any remaining NaNs.
2. **Categorical columns** (`WindGustDir`, `WindDir9am/3pm`, `RainToday`, `RainTomorrow`): filled with the **mode** per `Location`.
3. **`Cloud9am` / `Cloud3pm`**: bucketed via `qcut` based on `Humidity9am` and `Humidity3pm`, then filled using the median per `Location` + these buckets, with fallback to the `Location` median, then the global median.
4. **`Sunshine`**: same idea, relying on `Humidity` and `Cloud` buckets.
5. **`Evaporation`**: the most advanced imputation, relying on buckets from `MaxTemp`, `Temp3pm`, `Humidity3pm`, `Sunshine`, `WindSpeed3pm`, `WindSpeed9am` combined with `Location`.

### 4. Correlation Analysis
- Compute the correlation matrix between numerical variables.
- Display it as a heatmap to understand relationships, especially those involving `Cloud9am/3pm`, `Sunshine`, and `Evaporation`.

### 5. Additional Visualizations
- Distribution plots for `Cloud9am`, `Cloud3pm`, `Evaporation`, and `Sunshine` before and after imputation.
- Distribution of `RainTomorrow` (count plot).
- Average `Evaporation` across years for July (line plot).

### 6. Saving Cleaned Data
- Save the cleaned data to `weatherAUS_cleaned.csv`.
- Extract the full 2008 data and save it to `weather_2008.csv`.

### 7. Interactive Dashboard (Dash App)
- A simple Dash app displaying:
  - An interactive table (`DataTable`) showing the first 100 rows of the July data.
  - A histogram of `RainToday` distribution.
  - A chart of rainfall patterns across years (this part is cut off/incomplete in the current code).

---

## 🛠️ Libraries Used

```
pandas
numpy
matplotlib
seaborn
missingno
dash
dash-bootstrap-components
plotly
```

### Installing requirements
```bash
pip install pandas numpy matplotlib seaborn missingno dash dash-bootstrap-components plotly
```

Or create a `requirements.txt`:
```txt
pandas
numpy
matplotlib
seaborn
missingno
dash
dash-bootstrap-components
plotly
```
Then:
```bash
pip install -r requirements.txt
```

---

## ▶️ How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/weatherAUS-analysis.git
   cd weatherAUS-analysis
   ```

2. Place the `weatherAUS.csv` file inside a `data/` folder (and update the path in the notebook if it still points to a local path).

3. Install the requirements:
   ```bash
   pip install -r requirements.txt
   ```

4. Run the Jupyter Notebook:
   ```bash
   jupyter notebook weatherAUS.ipynb
   ```

5. To run only the dashboard, extract the Dash app code into a separate `.py` file and run it with:
   ```bash
   python app.py
   ```
   Then open your browser at `http://127.0.0.1:8050/`

---

## 📁 Project Structure (suggested)

```
weatherAUS-analysis/
│
├── data/
│   ├── weatherAUS.csv
│   ├── weatherAUS_cleaned.csv
│   └── weather_2008.csv
│
├── weatherAUS.ipynb
├── requirements.txt
└── README.md
```

---

## 📈 Results & Notes

- The multi-level imputation strategy (Location → Groups → Global Median/Mode) preserves the natural distribution of the data better than a naive random fill or a single global mean.
- There's a clear correlation between `Cloud9am/3pm`, `Humidity`, and `Sunshine`, which is the basis for the advanced imputation logic.
- Focusing on July specifically is useful because it represents the Australian winter season, giving a clearer picture of rainfall patterns during that period.

---

## 🚀 Future Improvements

- [ ] Fix file paths to be relative instead of hard-coded local paths.
- [ ] Separate the dashboard code into a standalone `app.py` file, apart from the notebook.
- [ ] Complete the missing part of the Dash app (the "Rain Pattern by Year" chart is currently cut off in the code).
- [ ] Add a predictive model to forecast `RainTomorrow` using the cleaned features.
- [ ] Add tests or data validation checks.

---

## 📄 License

This project is available under the MIT License (adjust as needed).
