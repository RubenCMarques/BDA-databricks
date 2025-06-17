# S&P 500 Direction Prediction Using Big Data Analytics

## 1. Introduction & Background

### 1.1 Problem Statement
Financial markets are complex, dynamic systems influenced by factors such as investor behavior, macroeconomic indicators, and global events. Accurately predicting short-term price movements in these markets is a significant challenge for both researchers and practitioners.  
This project focuses on forecasting the daily direction of the S&P 500 index using historical price data and U.S. inflation rates. Leveraging Big Data technologies, the goal is to build a scalable machine learning pipeline capable of handling and modeling large volumes of financial time series data.

### 1.2 Research Motivation
The motivation behind this study is twofold:
- **Economic impact:** Financial forecasting can guide investment decisions and inform policy analysis.
- **Technical demonstration:** The project demonstrates the power and limitations of Big Data Analytics using scalable, distributed processing (Apache Spark) in a real-world context.  
By combining lagged market features with inflation data, we explore whether short-term price direction can be effectively predicted using scalable algorithms.

---

## 2. Data Collection & Preprocessing

### 2.1 Data Sources
- **Market Data:** Sourced from Investing.com, including daily price, open, high, low, and forward prices.
- **Macroeconomic Data:** Monthly U.S. inflation rates from US Inflation Calculator (Consumer Price Index, Bureau of Labor Statistics).

### 2.2 Data Characteristics
- Several hundred daily entries covering ~25 years.
- Each entry: trading date, numerical variables (prices, indicators), forward-looking estimates.
- Monthly inflation aligned with daily entries using forward-filling.

### 2.3 Data Cleaning & Preprocessing Steps
- Checked for missing values; dropped `Volume` due to 100% missingness.
- Forward-filled missing inflation data.
- Engineered lagged features: one-day lag for price, open, high, low, percent change, forward open, forward price, inflation.
- Created binary label: 1 if price increased, 0 if decreased.
- Removed rows with missing values from lag operations.
- Assembled features into vector column (PySpark ML format); split data 80/20 for train/test.

---

## 3. Methodology & Tools

### 3.1 Machine Learning Techniques
- **Logistic Regression** and **Random Forest** models implemented with PySpark MLlib.
- Binary classification target: up (1) or down (0) daily price direction.
- Random Forest optimized via cross-validation and hyperparameter tuning.
- **Evaluation metric:** ROC AUC (higher = better class distinction).

| Model                   | ROC AUC |
|-------------------------|---------|
| Logistic Regression     | 0.5140  |
| Random Forest Untuned   | 0.5206  |
| Random Forest Tuned     | 0.5206  |

### 3.2 Querying & Storage
- All data manipulation and feature engineering conducted with PySpark (Spark DataFrames).
- Used PySpark SQL-style API for efficient processing, lag features, and aggregations.

### 3.3 Visualization
- Bar plots and grouped statistics to assess model prediction confidence.
- Probability scores binned (e.g., 0.4–0.5, 0.5–0.6) and compared to true outcomes.
- Most predictions were near 0.5, showing low model confidence.

### 3.4 Streaming Simulation
- Simulated a real-time scenario by processing data one day at a time.
- Demonstrated pipeline suitability for streaming/near real-time data.

---

## 4. Conclusion

This project demonstrates the application of Big Data Analytics (Apache Spark MLlib) to stock market prediction with large-scale financial data. We designed a scalable pipeline to process historical S&P 500 and futures prices, alongside inflation rates.  
By engineering lagged features, the pipeline attempts to capture short-term dependencies, with futures prices offering valuable predictive signals for next-day direction.  
Despite using several models (Logistic Regression, Random Forest, Tuned Random Forest), ROC AUC scores were only slightly better than random guessing, confirming the inherent unpredictability of short-term market movements based on historical prices alone.

**Key takeaways:**
- Big Data tools are valuable for efficiently handling and modeling financial time series.
- Predictive results are challenging to achieve with price and basic economic data due to market unpredictability.
- The pipeline is easily extensible to include advanced macroeconomic features, sentiment analysis, or technical signals, and can support streaming data for near real-time prediction.

---

## 5. Changes After the Defense

Following project defense, we addressed feedback by integrating two new data sources:
- **U.S. Inflation rates** (for broader macroeconomic context)
- **S&P 500 Futures prices** (forward-looking market perspective)

Engineered lagged features from these variables and incorporated them into the modeling pipeline, resulting in a more comprehensive analysis and aligning with real-world forecasting practices.

---

