# 🛠️ Spare Parts Inventory Forecasting

## 📌 Project Overview

This project focuses on forecasting **spare-parts demand** for service centres using historical service transaction data.

The objective is to help service centres maintain sufficient spare-parts availability while controlling unnecessary inventory costs and supporting **Just-in-Time (JIT)** inventory planning.

The project evaluates multiple forecasting approaches and develops a **hybrid forecasting strategy** where the forecasting method is selected according to the historical demand behavior of each spare part.

---

## 🎯 Business Problem

Service centres need to maintain the right quantity of spare parts to support customer service requirements.

* **Overstocking** increases inventory and storage costs.
* **Understocking** can result in shortages and service delays.
* Highly variable or intermittent spare-parts demand makes inventory planning more difficult.

This project uses historical service data to estimate future monthly demand for selected spare parts.

---

## 🎯 Project Objectives

* Analyze historical spare-parts consumption.
* Clean and preprocess service transaction data.
* Identify demand patterns and variability.
* Aggregate spare-parts demand at monthly level.
* Build time-series forecasting models.
* Develop a machine-learning forecasting model.
* Compare model performance using MAE and RMSE.
* Select the most suitable forecasting approach for each spare part.
* Generate future spare-parts demand forecasts.
* Provide business insights for inventory planning.

---

## 📊 Dataset

The dataset contains:

* **28,482 records**
* **7 columns**
* Date range: **30-May-2017 to 06-Jan-2019**

### Dataset Features

| Feature                 | Description                           |
| ----------------------- | ------------------------------------- |
| `invoice_date`          | Invoice transaction date              |
| `job_card_date`         | Job card date                         |
| `business_partner_name` | Business partner/customer information |
| `vehicle_no`            | Vehicle identification number         |
| `vehicle_model`         | Vehicle model                         |
| `current_km_reading`    | Current vehicle kilometer reading     |
| `invoice_line_text`     | Spare-part/service item description   |

---

## 🧹 Data Preprocessing

The following preprocessing steps were performed:

* Converted date columns to datetime format.
* Examined missing values.
* Identified and analyzed duplicate records.
* Analyzed unique spare-part/service items.
* Aggregated transaction-level data into monthly demand.
* Identified partial months.
* Excluded partial months from the primary forecasting evaluation.
* Selected 18 frequently occurring actual spare parts.
* Created time-series features for machine learning.

### Complete Modeling Period

The primary forecasting analysis used:

**June 2017 – December 2018**

This represents **19 complete months**.

---

## 🔩 Selected Spare Parts

The forecasting analysis included 18 spare parts:

1. AIR FILTER
2. BRAKE SHOE
3. OIL FILTER
4. DISC PAD
5. WHEEL RUBBER
6. SPARK PLUG
7. CHAIN SPROCKET
8. SPROCKET RUBBER
9. SPROCKET BEARING
10. CLUTCH CABLE
11. CLUTCH ASSEMBLY
12. CLUTCH COVER GASKET
13. TANK COVER
14. SEAT COVER
15. INDICATOR
16. DISC PUMP KIT
17. DRUM BOLT
18. TAIL LAMP BULB

---

## 📈 Exploratory Data Analysis

The EDA examined:

* Monthly demand trends
* Spare-part frequency
* Demand distribution
* Mean and median demand
* Standard deviation
* Minimum and maximum demand
* Zero-demand months
* Coefficient of variation
* Demand variability by spare part

### Demand Pattern Categories

#### Relatively Stable

* BRAKE SHOE
* OIL FILTER
* DISC PAD
* CHAIN SPROCKET

#### Moderately Variable

Several parts including:

* AIR FILTER
* SPARK PLUG
* WHEEL RUBBER
* CLUTCH CABLE
* CLUTCH ASSEMBLY
* TANK COVER
* SEAT COVER
* INDICATOR

#### Intermittent / Highly Variable

* SPROCKET RUBBER
* SPROCKET BEARING

These classifications were based on the observed historical demand characteristics.

---

## ⏱️ Forecasting Approach

Four forecasting approaches were evaluated.

### 1. Naive Forecast

The previous month's demand was used as the forecast for the following month.

### 2. Three-Month Moving Average

The average demand of the previous three months was used as the forecast.

### 3. Simple Exponential Smoothing

More recent observations were given greater influence when generating forecasts.

### 4. Random Forest Regression

A Random Forest regression model was developed using historical demand and time-based features.

---

## 🤖 Machine Learning Feature Engineering

The Random Forest model used:

* `lag_1`
* `lag_2`
* `lag_3`
* `rolling_mean_3`
* `rolling_std_3`
* `month`
* Spare-part identity

### Time-Based Split

The forecasting problem was treated as a time-series problem rather than using a random train-test split.

**Training period:**

September 2017 – September 2018

**Testing period:**

October 2018 – December 2018

The machine-learning dataset contained:

* **234 training observations**
* **54 testing observations**

---

## 📊 Model Evaluation

The primary evaluation metrics were:

* **MAE — Mean Absolute Error**
* **RMSE — Root Mean Squared Error**

MAE was used as the primary comparison metric because it represents the average forecasting error in demand units.

### Overall Model Comparison

| Model                        |        MAE |   RMSE |
| ---------------------------- | ---------: | -----: |
| Naive                        |  **8.704** | 12.442 |
| Simple Exponential Smoothing |  **9.535** |      — |
| 3-Month Moving Average       | **11.475** | 16.925 |
| Random Forest                | **11.692** | 16.854 |

The results show that simple forecasting methods performed competitively on the available dataset.

---

## 🔀 Hybrid Forecasting Strategy

Forecasting performance differed between spare parts.

Therefore, instead of applying one model to every spare part, the model with the lowest MAE was selected individually.

| Spare Part          | Selected Model |
| ------------------- | -------------- |
| AIR FILTER          | SES            |
| BRAKE SHOE          | Naive          |
| CHAIN SPROCKET      | Random Forest  |
| CLUTCH ASSEMBLY     | Random Forest  |
| CLUTCH CABLE        | Random Forest  |
| CLUTCH COVER GASKET | Moving Average |
| DISC PAD            | Moving Average |
| DISC PUMP KIT       | Moving Average |
| DRUM BOLT           | Naive          |
| INDICATOR           | Random Forest  |
| OIL FILTER          | Random Forest  |
| SEAT COVER          | Random Forest  |
| SPARK PLUG          | Naive          |
| SPROCKET BEARING    | SES            |
| SPROCKET RUBBER     | Naive          |
| TAIL LAMP BULB      | SES            |
| TANK COVER          | SES            |
| WHEEL RUBBER        | Naive          |

### Model Selection Summary

| Model          | Number of Spare Parts |
| -------------- | --------------------: |
| Random Forest  |                     6 |
| Naive          |                     5 |
| SES            |                     4 |
| Moving Average |                     3 |

This demonstrates that different spare parts require different forecasting approaches depending on their historical demand patterns.

---

## 🔮 January 2019 Forecast

Using information available through December 2018, the hybrid forecasting strategy generated the following January 2019 forecasts:

| Spare Part          | Forecast Demand |
| ------------------- | --------------: |
| AIR FILTER          |             144 |
| BRAKE SHOE          |              45 |
| CHAIN SPROCKET      |              32 |
| CLUTCH ASSEMBLY     |              20 |
| CLUTCH CABLE        |              27 |
| CLUTCH COVER GASKET |              18 |
| DISC PAD            |              32 |
| DISC PUMP KIT       |               9 |
| DRUM BOLT           |               7 |
| INDICATOR           |               5 |
| OIL FILTER          |              60 |
| SEAT COVER          |              12 |
| SPARK PLUG          |              27 |
| SPROCKET BEARING    |              90 |
| SPROCKET RUBBER     |             113 |
| TAIL LAMP BULB      |               5 |
| TANK COVER          |               7 |
| WHEEL RUBBER        |               6 |

---

## ⚠️ January 2019 Validation Note

January 2019 was a **partial month** in the original dataset.

The available January actuals were therefore not treated as the primary model evaluation period.

The partial-month validation produced:

* MAE: **30.111**
* RMSE: **44.000**

These values should **not** be interpreted as the project's final model accuracy.

The main model evaluation is based on the complete-month **October–December 2018** test period.

---

## 💡 Key Business Insights

### High-Demand Spare Parts

AIR FILTER, BRAKE SHOE, and OIL FILTER showed relatively high demand among the selected parts.

These parts require close inventory monitoring.

### Stable-Demand Parts

BRAKE SHOE, OIL FILTER, DISC PAD, and CHAIN SPROCKET showed comparatively stable demand patterns.

### Intermittent Demand

SPROCKET RUBBER and SPROCKET BEARING showed highly variable and intermittent demand.

These parts may require different inventory planning strategies from regularly consumed parts.

### Model Performance Varies by Part

No single forecasting model performed best for every spare part.

A hybrid strategy can therefore provide part-specific forecasts.

---

## 📌 Business Recommendations

### 1. Update Forecasts Regularly

Forecasts should be recalculated periodically using the latest service transactions.

### 2. Monitor High-Demand Parts

Frequently consumed parts should receive closer inventory monitoring.

### 3. Handle Intermittent Demand Separately

Highly variable parts should not necessarily be managed using the same forecasting strategy as stable-demand parts.

### 4. Use Part-Specific Forecasting

Select forecasting methods according to historical performance for each spare part.

### 5. Track Forecast Error

Compare forecast demand with actual demand regularly to identify deterioration in model performance.

### 6. Add Inventory Information

Future versions can include:

* Current stock level
* Supplier lead time
* Safety stock
* Reorder point
* Stockout history
* Purchase orders
* Supplier information

This would allow the project to move beyond demand forecasting toward inventory optimization.

---

## ⚠️ Project Limitations

* Only 19 complete months were available for the primary analysis.
* Only 18 selected spare parts were modeled.
* The primary test period contained only three complete months.
* January 2019 was a partial month.
* Inventory-specific variables were not available.
* External demand drivers were not included.
* Some spare parts have intermittent demand.
* The dataset represents service transactions rather than complete inventory movement.

---

## 🏁 Conclusion

This project developed a spare-parts demand forecasting framework using historical service transaction data.

The project included:

* Data cleaning
* Exploratory data analysis
* Monthly demand aggregation
* Demand variability analysis
* Time-series forecasting
* Machine-learning forecasting
* Model comparison
* Part-specific model selection
* Future demand forecasting
* Business recommendations

The evaluation showed that simple forecasting methods performed strongly on the available data, while model performance varied by spare part.

A **hybrid forecasting strategy** was therefore developed to select an appropriate forecasting approach for each spare part.

The solution can support inventory planning by providing estimated future spare-parts demand. Additional inventory variables such as stock levels, lead times, safety stock, reorder points, and stockout history could further improve the solution.

---

## 🛠️ Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Jupyter Notebook
* Time-Series Forecasting
* Random Forest Regression
* Exploratory Data Analysis
* Feature Engineering

---

## 📁 Project Structure

```text
Forecasting-Spare-Parts-Inventory/
│
├── data/
│   └── service_data.csv
|
├── reports/
│   └── Spare Parts Inventory Forecasting-Final_Project_Report.docx
|
├──Inventory_Forecasting.ipynb
│
└── README.md
```

---

## 👨‍💻 Author

**Devam Jasani**

Data Science & AI/ML Engineer

---


### 👨‍💻Contributing 



* Contributions are welcome! If you have suggestions or improvements, please fork the repository and submit a pull request.

---

## ⭐ If you found this project useful, don't forget to star this repository!
