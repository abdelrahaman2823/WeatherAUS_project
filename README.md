# Weather Australia Data Processing

## Project Overview

This project focuses on the preprocessing, cleaning, analysis, and visualization of the **WeatherAUS** dataset.

The original dataset, **`weatherAUS.csv`**, contains daily weather observations collected from different locations in Australia. The project applies a dedicated preprocessing and missing-value treatment workflow to the **July subset** of the dataset, followed by exploratory analysis and interactive visualization.

---

## Dataset Structure

### Main Dataset

**File:** `weatherAUS.csv`

The original WeatherAUS dataset is used as the primary data source.

The `Date` column is converted to a datetime format, and the following temporal features are extracted:

* `year`
* `month`
* `day`
* `dat_of_week`

The original `Date` column is then removed from the working dataset.

---

## July Cleaned Dataset

### File

**`weatherAUS_cleaned.csv`**

This dataset represents the **July-only subset** of the main WeatherAUS dataset.

The July records are selected using:

```python
df2 = df[df["month"] == 7].copy()
```

The cleaning process is performed specifically on this July dataset.

### Missing-Value Treatment

#### Numerical Features

Missing values in numerical weather variables are initially imputed using the **median value within each location**.

If values remain missing after location-based imputation, the overall median of the corresponding column is used.

The numerical variables include:

* `MinTemp`
* `MaxTemp`
* `Rainfall`
* `WindGustSpeed`
* `WindSpeed9am`
* `WindSpeed3pm`
* `Humidity9am`
* `Humidity3pm`
* `Pressure9am`
* `Pressure3pm`
* `Temp9am`
* `Temp3pm`

---

#### Categorical Features

Missing categorical values are handled using the **mode within each location**.

If a missing value remains, the overall mode of the corresponding column is used.

The categorical variables include:

* `WindGustDir`
* `WindDir9am`
* `WindDir3pm`
* `RainToday`
* `RainTomorrow`

---

### Cloud Coverage Imputation

`Cloud9am` and `Cloud3pm` are treated separately because their missing values are analyzed in relation to humidity and location.

The process uses:

1. Humidity-based quantile groups.
2. Location-based grouping.
3. Median imputation within the corresponding groups.
4. Location-level median as a fallback.
5. Overall median as the final fallback.

This approach preserves relationships between cloud coverage, humidity, and geographical location.

---

### Sunshine Imputation

Missing values in `Sunshine` are handled using a more detailed grouping strategy based on:

* `Location`
* `Humidity9am`
* `Humidity3pm`
* `Cloud9am`
* `Cloud3pm`

Quantile-based groups are created using `pd.qcut`, and group-level medians are used for imputation.

Location-level median and overall median are then used as fallback methods.

---

### Evaporation Imputation

Missing `Evaporation` values are imputed using multiple weather-related grouping variables:

* `Location`
* `MaxTemp`
* `Temp3pm`
* `Humidity3pm`
* `Sunshine`
* `WindSpeed3pm`
* `WindSpeed9am`

Quantile-based groups are created for the relevant numerical variables, followed by group-level median imputation.

Location-level median and overall median are used as fallback methods.

---

## Exploratory Data Analysis

The notebook includes several exploratory analysis steps to evaluate the quality and characteristics of the July dataset.

### Missing Values

Missing-value distributions are examined before and after the cleaning process using:

* Missing-value counts
* `missingno`
* Bar plots

This allows the effectiveness of the imputation process to be evaluated.

### Distribution Analysis

The distributions of several weather variables are visualized, including:

* Cloud coverage
* Sunshine
* Evaporation

### Correlation Analysis

Correlation matrices are generated to investigate relationships between numerical weather variables.

The analysis includes variables related to:

* Temperature
* Humidity
* Rainfall
* Wind speed
* Atmospheric pressure
* Cloud coverage
* Sunshine
* Evaporation

### Rainfall Analysis

The July dataset is also analyzed in relation to:

* `RainToday`
* `RainTomorrow`
* Yearly rainfall patterns
* Sunshine and rainfall
* Evaporation and rainfall
* Cloud coverage and rainfall
* Wind direction and rainfall

---

## Visualization Dashboard

An interactive dashboard is implemented using **Dash**, **Plotly**, and **Dash Bootstrap Components**.

The dashboard provides:

* July weather data table
* Rain Today distribution
* July rain patterns by year
* Sunshine vs. Rain Today
* Evaporation vs. Rain Today
* Cloud 3pm vs. Rain Today
* Wind Direction vs. Rain Today

The dashboard is designed specifically for exploring the cleaned July dataset.

---

## Additional Dataset

### `weather_2008.csv`

The notebook also exports a separate dataset containing observations from **2008 only**.

This dataset is created by filtering the main processed dataframe using:

```python
df_2008 = df[df['year'] == 2008]
```

It is important to distinguish this dataset from `weatherAUS_cleaned.csv`:

| Dataset                  | Scope                   | Purpose                                              |
| ------------------------ | ----------------------- | ---------------------------------------------------- |
| `weatherAUS.csv`         | Full WeatherAUS dataset | Main source dataset                                  |
| `weatherAUS_cleaned.csv` | **July only**           | Cleaned July dataset used for analysis and dashboard |
| `weather_2008.csv`       | **Year 2008**           | Separate yearly subset                               |

---

## Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Missingno
* Plotly
* Dash
* Dash Bootstrap Components

---

## Processing Workflow

```text
weatherAUS.csv
      │
      ▼
Date Conversion & Feature Extraction
      │
      ├── year
      ├── month
      ├── day
      └── day of week
      │
      ▼
July Filtering
(month == 7)
      │
      ▼
Missing-Value Analysis
      │
      ▼
Numerical Imputation
(Location Median → Overall Median)
      │
      ▼
Categorical Imputation
(Location Mode → Overall Mode)
      │
      ▼
Cloud Coverage Imputation
      │
      ▼
Sunshine Imputation
      │
      ▼
Evaporation Imputation
      │
      ▼
Exploratory Data Analysis
      │
      ▼
weatherAUS_cleaned.csv
      │
      ▼
Interactive July Dashboard
```

---

## Output

The primary cleaned output of this workflow is:

**`weatherAUS_cleaned.csv`**

This file contains the cleaned **July weather observations** and is the main dataset used for the subsequent July analysis and dashboard.
