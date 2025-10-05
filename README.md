# 🛍️ Product Price Prediction Model

This project aims to predict product prices using the dataset from the Kaggle Academy2025 competition. The machine learning models developed in this project predict prices based on various features such as product, date, and market information.

## 📁 Contents

- [Dataset](#-dataset)
- [Setup](#-setup)
- [Exploratory Data Analysis and Preprocessing](#-exploratory-data-analysis-and-preprocessing)
- [Feature Engineering](#-feature-engineering)
- [Modeling and Evaluation](#-modeling-and-evaluation)
- [Results](#-results)
- [Future Work](#-future-work)
- [License & Kaggle Data Usage](#-license--kaggle-data-usage)

## 📊 Dataset

- **Source**: [Kaggle Academy2025 Competition](https://www.kaggle.com/competitions/academy2025/data)
- **Files**:
  - `train.csv`: Training dataset (227,520 rows, 8 columns)
  - `testFeatures.csv`: Test dataset (45,504 rows, 8 columns)

- **Columns**:
  - `date`: Date when the product price was recorded
  - `product`: Product name
  - `product_nutritional_value`: Nutritional content of the product
  - `product_category`: Product category
  - `product_price`: Target variable (price)
  - `product_origin`: Place of production
  - `market`: Market where the product was sold
  - `city`: City where the product was sold
  - `id`: (only in test set)

- **Missing Values**: None

## ⚙️ Setup

Install the required Python libraries:

```bash
pip install pandas numpy seaborn matplotlib scikit-learn xgboost
```

## 🔍 Exploratory Data Analysis and Preprocessing

* The `tarih` column was renamed to `date` and time-based features such as `year`, `month`, `day`, `week`, and `season` were extracted.
* Outliers were removed using the Z-score method (applied to `product_price` and `product_nutritional_value`).
* Relationships between categorical variables and the target variable were visualized using boxplots.

---

## 💠 Feature Engineering

* **Mean Encoding:** Applied to `product`, `market`, `city`, `product_category`, and `product_origin`.
* **New Features:**
  * `weekend` (day-based)
  * `holiday_season` (month-based)
* **One-Hot Encoding:** Added for mean-encoded columns.
* **Correlation Matrix:** Relationships between numerical features were analyzed using a heatmap.
* **RFE (Recursive Feature Elimination):** The top 10 features were selected.

---

## 🤖 Modeling and Evaluation

Different models were evaluated using RMSE, MAE, and R² metrics:

### 📈 Linear Regression

* **RMSE:** 5.40
* **MAE:** 3.62
* **R²:** 0.8591

### 🌲 Random Forest Regressor

* **RMSE:** 1.39
* **MAE:** 0.77
* **R²:** 0.9906

### 🚀 XGBoost Regressor

* **Initial Model:**
  * RMSE: 1.10, MAE: 0.65, R²: 0.9942
* **With Early Stopping and Regularization:**
  * RMSE: 1.46, MAE: 0.89, R²: 0.9897
* **Optimized with RandomizedSearchCV:**
  * Best parameters:
    ```python
    {
      'subsample': 1.0,
      'reg_lambda': 1,
      'reg_alpha': 0.5,
      'min_child_weight': 3,
      'max_depth': 6,
      'colsample_bytree': 0.9
    }
    ```
  * RMSE: 1.16, MAE: 0.68, R²: 0.9935
* **Final RFE Model (10 features):**
  * RMSE: 1.35, MAE: 0.76, R²: 0.9912
* **Final Optimized Model (Regularization + EarlyStopping):**
  * RMSE: 1.58, MAE: 0.95, R²: 0.9879

---

## 📌 Results

* **XGBoost Regressor** outperformed linear regression and random forest models.
* `product_encoded` and `year` were the most important features in price prediction.
* **Hyperparameter optimization** significantly improved model performance.
* Reducing complexity sometimes slightly decreased performance but helped reduce overfitting.

---

## 🔮 Future Work

* More detailed hyperparameter searches using GridSearchCV
* Explore alternative regression models (CatBoost, LightGBM)

## 🛡️ License & Kaggle Data Usage

* This project code is released under the MIT License.

* Kaggle Dataset Usage Rules:

* The dataset belongs to the Academy2025 Kaggle Competition and is used for educational purposes and model development only.

* Submissions and work derived from the dataset must comply with Kaggle competition rules.

* The dataset cannot be redistributed outside of Kaggle.

* Notebooks or code sharing is allowed publicly if the dataset itself is not directly shared, and proper citation is given.
* Add new time series-based features
* Data augmentation and model ensembles
