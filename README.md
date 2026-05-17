<div align="center">

<img src="https://img.shields.io/badge/Stock%20Price-Predictor-16a34a?style=for-the-badge&logoColor=white" alt="Stock Price Predictor" />

# 📈 Stock Price Predictor

### **Machine Learning-Powered Stock Market Analysis using KNN**

*Predict stock price direction and closing values using K-Nearest Neighbors — trained on real TATAGLOBAL market data.*

<br/>

[![Python](https://img.shields.io/badge/Python-3.9-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data-150458?style=flat-square&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-Array-013243?style=flat-square&logo=numpy&logoColor=white)](https://numpy.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-Viz-11557C?style=flat-square&logo=matplotlib&logoColor=white)](https://matplotlib.org/)

<br/>

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![Dataset](https://img.shields.io/badge/Dataset-TATAGLOBAL-blue?style=flat-square)](https://github.com/Shaheer-zahid/stock-price-predictor)
[![Notebooks](https://img.shields.io/badge/Notebooks-2-orange?style=flat-square&logo=jupyter)](https://github.com/Shaheer-zahid/stock-price-predictor)

</div>

---

## 🧠 Overview

**Stock Price Predictor** is a machine learning project that applies the **K-Nearest Neighbors (KNN)** algorithm to historical stock market data to tackle two key prediction problems:

| Problem | Type | Goal |
|---|---|---|
| 📊 **Classification** | Binary | Will the stock close **higher or lower** than it opened? |
| 💰 **Regression** | Continuous | What will the **exact closing price** be? |

The dataset covers **1,235 trading days** of [TATAGLOBAL](https://www.nseindia.com/) (NSE India) stock data spanning **2013 to 2018**, providing a solid foundation for both model types.

---

## 📁 Repository Structure

```
stock-price-predictor/
├── 📓 Stock_classification.ipynb   # KNN classifier — predict price direction (Up/Down)
├── 📓 Stock_regression.ipynb       # KNN regressor  — predict exact closing price
├── 📊 Stock_dataset.csv            # TATAGLOBAL historical stock data (1,235 rows)
└── 📄 README.md
```

---

## 📊 Dataset

**File:** `Stock_dataset.csv`

Historical daily trading data for **TATAGLOBAL** (NSE: TATAGLOBAL), covering October 2013 – October 2018.

| Column | Description |
|---|---|
| `Date` | Trading date |
| `Open` | Opening price of the day |
| `High` | Highest price of the day |
| `Low` | Lowest price of the day |
| `Last` | Last traded price |
| `Close` | Closing price of the day |
| `Total Trade Quantity` | Total shares traded |
| `Turnover (Lacs)` | Total turnover in Indian Lacs |

**Shape:** 1,235 rows × 8 columns

---

## 🔬 Notebooks

### 📓 1. Stock Classification — `Stock_classification.ipynb`

Predicts whether the stock will **close higher or lower** than its opening price (a binary up/down signal).

**Pipeline:**

```
Raw Data
  └─► Feature Engineering  (Open-Close diff, High-Low diff)
      └─► Binary Label Creation  (1 = price went up, 0 = price went down)
          └─► Train / Test Split  (75% / 25%, random_state=44)
              └─► GridSearchCV  (k = 2 to 11, 5-fold CV)
                  └─► KNeighborsClassifier  (best k selected automatically)
                      └─► StandardScaler  (feature normalization)
                          └─► Accuracy Evaluation
```

**Features used:**

| Feature | Formula | Intuition |
|---|---|---|
| `Open - Close` | Opening price − Closing price | Daily price momentum |
| `High - Low` | Day's high − Day's low | Daily volatility range |

**Target:** `1` if next close > today's close, else `0`

**Hyperparameter Tuning:** `GridSearchCV` over `n_neighbors ∈ {2, 3, …, 11}` with 5-fold cross-validation to find the optimal `k`.

---

### 📓 2. Stock Regression — `Stock_regression.ipynb`

Predicts the **exact closing price** (continuous value) rather than just the direction.

**Pipeline:**

```
Raw Data
  └─► Feature Engineering  (Open-Close diff, High-Low diff)
      └─► Target: Closing Price  (continuous)
          └─► Train / Test Split  (75% / 25%, random_state=44)
              └─► GridSearchCV  (k = 2 to 11, 5-fold CV)
                  └─► KNeighborsRegressor  (best k selected automatically)
                      └─► Prediction & RMSE Evaluation
                          └─► Actual vs Predicted DataFrame
```

**Evaluation Metric:** Root Mean Squared Error (RMSE)

```python
rms = np.sqrt(np.mean(np.power((actual - predicted), 2)))
# Result: ~39.20
```

**Sample Predictions:**

| Actual Close | Predicted Close |
|---|---|
| 161.60 | 206.84 |
| 193.85 | 230.08 |
| 143.90 | 153.26 |
| 278.20 | 199.20 |

---

## 🛠️ Tech Stack

| Library | Version | Usage |
|---|---|---|
| `Python` | 3.9 | Core language |
| `NumPy` | — | Numerical operations & RMSE calculation |
| `Pandas` | — | Data loading, feature engineering, result tables |
| `Matplotlib` | — | Data visualization |
| `scikit-learn` | — | KNN models, GridSearchCV, StandardScaler, train/test split |
| `Jupyter Notebook` | — | Interactive development environment |

---

## 🚀 Getting Started

### Prerequisites

Make sure you have Python 3.7+ and Jupyter installed.

### 1. Clone the Repository

```bash
git clone https://github.com/Shaheer-zahid/stock-price-predictor.git
cd stock-price-predictor
```

### 2. Install Dependencies

```bash
pip install numpy pandas matplotlib scikit-learn jupyter
```

Or with conda:

```bash
conda install numpy pandas matplotlib scikit-learn jupyter
```

### 3. Launch Jupyter Notebook

```bash
jupyter notebook
```

Then open either notebook:
- `Stock_classification.ipynb` — for direction prediction
- `Stock_regression.ipynb` — for price prediction

---

## ⚙️ How It Works

### Feature Engineering

Both notebooks derive two key features from the raw OHLC data:

```python
data['Open - Close'] = data['Open'] - data['Close']  # Daily momentum
data['High - Low']   = data['High']  - data['Low']   # Daily volatility
```

These two features capture the essential price movement signals without requiring complex technical indicators.

### Model Selection via Grid Search

```python
params = {'n_neighbors': [2, 3, 4, 5, 6, 7, 8, 9, 10, 11]}
model = GridSearchCV(KNeighborsClassifier(), params, cv=5)
model.fit(X_train, y_train)
```

`GridSearchCV` automatically selects the `k` value that generalises best across 5 cross-validation folds — no manual tuning needed.

### Feature Scaling (Classification)

```python
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled  = scaler.transform(X_test)
```

Scaling is applied to improve KNN's distance-based predictions, since the two features operate on different numeric ranges.

---

## 📈 Results

| Notebook | Model | Metric | Result |
|---|---|---|---|
| Classification | KNeighborsClassifier + GridSearchCV | Accuracy (scaled) | See notebook output |
| Regression | KNeighborsRegressor + GridSearchCV | RMSE | ~39.20 |

---

## 🤝 Contributing

Contributions and improvements are welcome!

1. **Fork** the repository
2. **Create** your feature branch
3. **Commit** your changes with a clear message
4. **Push** to your branch
5. **Open** a Pull Request

Ideas for improvement:
- Add LSTM / time-series models for sequential pattern learning
- Include additional technical indicators (RSI, MACD, Bollinger Bands)
- Expand to multiple stock tickers
- Add a Streamlit or Flask web interface for live predictions

---

## 📄 License

Distributed under the **MIT License**. See `LICENSE` for more information.

---

<div align="center">

Built with ❤️ by [Shaheer Zahid](https://github.com/Shaheer-zahid)

⭐ **Star this repo if you found it useful!**

</div>
