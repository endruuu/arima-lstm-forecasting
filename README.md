# 🛒 Grocery Sales Forecasting with ARIMA and LSTM

<p align="center">
  <img src="https://img.shields.io/badge/Project-Sales%20Forecasting-blue?style=for-the-badge" alt="Project Badge">
  <img src="https://img.shields.io/badge/Methods-ARIMA%20%7C%20LSTM-success?style=for-the-badge" alt="Methods Badge">
</p>

<p align="center">
  This project compares two forecasting approaches, <b>ARIMA</b> and <b>LSTM</b>,
  to predict grocery sales for the next <b>30 days</b> and support better stock preparation.
</p>

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Background](#-background)
- [Project Objectives](#-project-objectives)
- [Dataset](#-dataset)
- [Methodology](#-methodology)
- [ARIMA Experiment](#-arima-experiment)
- [LSTM Experiment](#-lstm-experiment)
- [Results and Evaluation](#-results-and-evaluation)
- [Conclusion](#-conclusion)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [How to Run](#-how-to-run)
- [Technologies Used](#-technologies-used)

---

## 🎯 Project Overview

This project develops a sales forecasting system for a grocery store using two
different approaches:

- **ARIMA** for statistical time series forecasting
- **LSTM** for deep learning-based sequence modeling

The main goal is to generate **30-day sales forecasts** so the store can
estimate future demand and prepare inventory more effectively.

## 🧩 Background

Daily sales data often contains seasonality, trends, and fluctuations that are
not always captured equally well by a single modeling approach. Because of that,
this project compares a classical statistical model and a deep learning model on
the same dataset to identify which approach is more suitable for grocery sales
forecasting.

## 🎯 Project Objectives

- Understand the characteristics of the sales dataset
- Perform time series preprocessing for forecasting
- Evaluate the performance of **ARIMA** and **LSTM**
- Compare the results of both approaches
- Identify the best model for this forecasting case

---

## 📊 Dataset

The dataset used in this project contains grocery sales data from a store in
**Ecuador, South America**.

🔗 **Dataset source**
https://drive.google.com/file/d/1gVorz1UQg3C_WrY0-IoSsj1PK4q-TReX/view

### Dataset summary

- File used in the experiment: **`store5.csv`**
- Analysis focus: **Store 5**
- Initial data size: **55,572 rows × 7 columns**
- Number of product families: **33**
- Time range starts from **2013-01-01**

### Main columns

| Column        | Description                    |
| ------------- | ------------------------------ |
| `id`          | Unique identifier for each row |
| `date`        | Transaction date               |
| `store_nbr`   | Store number                   |
| `family`      | Product category               |
| `sales`       | Sales value                    |
| `onpromotion` | Number of items on promotion   |
| `dcoilwtico`  | Crude oil price                |

### Derived data for modeling

For the experiments, the raw data is transformed into daily time series data:

- In the **ARIMA** workflow, the data is aggregated into **daily total sales**
  and enriched with an exogenous feature: **`is_holiday`**
- In **LSTM**, the data is aggregated into two main features:
  - **`total_sales`**
  - **`total_onpromotion`**

---

## 🔬 Methodology

The project workflow consists of five main stages:

1. **Problem Definition** Define the forecasting target, which is sales for the
   next 30 days.

2. **Gathering Information** Perform initial exploration to understand sales
   patterns, missing values, and data structure.

3. **Preliminary Exploratory Analysis** Analyze the time series characteristics,
   including seasonality, outliers, and stationarity.

4. **Choosing and Fitting Models** Train and compare **ARIMA-based modeling**
   and several deep learning architectures.

5. **Using and Evaluating Models** Generate predictions on the test set,
   evaluate performance metrics, visualize results, and forecast future sales.

---

## 📈 ARIMA Experiment

The ARIMA-based approach in this project is implemented using **SARIMAX**, which
is a seasonal ARIMA model with exogenous variables. So, while the project title
uses **ARIMA** for simplicity, the actual implementation in the notebook uses
**SARIMAX**.

### Main steps

- Load and preprocess the data
- Convert `date` to datetime format
- Filter the analysis period
- Aggregate daily sales
- Create the `is_holiday` feature
- Perform stationarity testing using the **ADF Test**
- Analyze **ACF** and **PACF**
- Run grid search for optimal parameters
- Split data into train and test sets
- Fit the best model
- Predict on the test set
- Forecast the next **30 days**

### Key findings

- The sales series is **stationary** based on the ADF Test
- Seasonal differencing with **lag 7** is also stationary
- Best parameters from grid search:
  - **Order**: `(1, 1, 1)`
  - **Seasonal order**: `(0, 1, 1, 7)`

### Data split

- **Training**: 1350 days
- **Testing**: 338 days

---

## 🤖 LSTM Experiment

The LSTM approach is used to capture non-linear patterns and sequential
dependencies in the sales data.

### Main steps

- Load the dataset
- Inspect data structure and missing values
- Convert `date` into a time index
- Resample and aggregate data daily
- Perform exploratory data analysis
- Remove closed-store days (`total_sales = 0`) to improve modeling stability
- Split data into **train, validation, and test**
- Normalize the data
- Create sequences / windows
- Train several deep learning architectures
- Predict on the test set
- Forecast the next **30 days**

### Architectures tested

- **Single LSTM**
- **Stacked LSTM**
- **Bidirectional LSTM**
- **GRU**
- **LSTM + BatchNormalization + L1/L2 Regularization**

### Data split

- **Train**: 1175 rows
- **Validation**: 167 rows
- **Test**: 337 rows

---

## 🏆 Results and Evaluation

The models are evaluated using three metrics:

- **MAE** (_Mean Absolute Error_)
- **RMSE** (_Root Mean Squared Error_)
- **MAPE** (_Mean Absolute Percentage Error_)

### ARIMA-based results

| Model   |    MAE |    RMSE |                     MAPE |
| ------- | -----: | ------: | -----------------------: |
| SARIMAX | 906.34 | 1411.17 | 26809767917521125376.00% |

### Deep learning results

| Model                    |     MAE |    RMSE |   MAPE |
| ------------------------ | ------: | ------: | -----: |
| Single LSTM              | 1102.44 | 1456.10 | 10.68% |
| Stacked LSTM             | 1261.90 | 1729.71 | 11.89% |
| GRU                      | 1405.18 | 1937.15 | 12.73% |
| Bidirectional LSTM       | 1434.34 | 1975.66 | 12.95% |
| LSTM + BatchNorm + L1/L2 | 3097.74 | 3595.80 | 28.43% |

### Comparison summary

| Aspect                              | ARIMA                                       | LSTM                                      |
| ----------------------------------- | ------------------------------------------- | ----------------------------------------- |
| Approach type                       | Statistical time series                     | Deep learning                             |
| Main strength                       | More interpretable and lightweight          | Better at capturing complex patterns      |
| Best-performing model in experiment | SARIMAX                                     | **Single LSTM**                           |
| Evaluation note                     | MAE and RMSE are good, but MAPE is unstable | Results are more consistent and realistic |

### Result interpretation

Although **SARIMAX** produces slightly lower **MAE** and **RMSE**, its **MAPE**
is extremely large and unrealistic. This suggests that MAPE is not a reliable
indicator for this model on the dataset used in this project.

In contrast, **Single LSTM** provides more stable and practical forecasting
performance, with:

- **MAE:** 1102.44
- **RMSE:** 1456.10
- **MAPE:** 10.68%

---

## ✅ Conclusion

Based on the experiments, **Single LSTM** is selected as the best model for this
project.

This model provides more stable and realistic results for the grocery sales
forecasting case, making it a stronger choice for predicting sales over the next
30 days.

Overall, the findings suggest that the sales patterns in this dataset likely
contain non-linear components and more complex dynamics, making deep learning
more suitable than a classical statistical model for this task.

---

## 📁 Project Structure

The repository structure used in this project is:

```bash
SalesForecasting/
│
├── dataset/
│   └── store5.csv
│
├── notebook/
│   ├── forecast_arima.ipynb
│   └── forecast_lstm.ipynb
│
├── documentation/
│   └── Project 1 - Time Series_Iron Man.pdf
│
└── README.md
```

---

## 🚀 Installation

### Prerequisites

- Python 3.10 or a compatible version
- Jupyter Notebook or JupyterLab
- `pip`

### Install the main libraries

```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy statsmodels tensorflow keras pmdarima
```

### Optional virtual environment

```bash
python -m venv venv
```

**Windows**

```bash
venv\Scripts\activate
```

**Linux / Mac**

```bash
source venv/bin/activate
```

---

## 💻 How to Run

### 1. Prepare the dataset

Make sure `store5.csv` is available inside the `dataset/` folder or in the
location expected by the notebooks.

### 2. Run the ARIMA notebook

Open and run:

```bash
notebook/forecast_arima.ipynb
```

This notebook includes:

- data preprocessing
- ADF test
- ACF & PACF analysis
- parameter grid search
- model evaluation
- 30-day forecasting

### 3. Run the LSTM notebook

Open and run:

```bash
notebook/forecast_lstm.ipynb
```

This notebook includes:

- daily data aggregation
- data normalization
- sequence creation
- training multiple architectures
- model evaluation
- 30-day forecasting

---

## 🧰 Technologies Used

- **Python**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Seaborn**
- **scikit-learn**
- **statsmodels**
- **TensorFlow / Keras**
- **pmdarima**
- **Jupyter Notebook**
