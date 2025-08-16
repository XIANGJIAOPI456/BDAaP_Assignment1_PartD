# Nutrient Load Prediction in the Murray–Darling Basin

This repository provides a Jupyter Notebook (`three model.ipynb`) that applies **machine learning techniques** to predict nutrient export loads in the Murray–Darling Basin (2014–2023).  
The study addresses the critical question: *How does daily river flow influence nutrient export, and which nutrient is most sensitive to flow variation?*  

By combining hydrological datasets with temporal features and advanced predictive models, this project demonstrates how **data-driven forecasting** can support **water quality management** in river ecosystems.

---

##  Background
The Murray–Darling Basin is one of Australia’s most important water systems, sustaining agriculture, ecosystems, and communities. Nutrient export loads (e.g., nitrogen, phosphorus, organic matter) directly impact **water quality, biodiversity, and algal bloom risk**.  
Understanding and forecasting these loads is essential for **evidence-based water management**.

Traditional hydrological models capture some aspects of nutrient transport but often fail to represent **non-linear, time-dependent dynamics**. This project instead uses **machine learning** to capture these complex interactions, offering both **accuracy** and **interpretability**.

---

##  Objectives
- Investigate how **daily water flow** drives nutrient export in the Murray River.  
- Build predictive models for **seven nutrient-related variables**.  
- Compare the performance of **Random Forest, Support Vector Regression, and LSTM**.  
- Evaluate feature importance to identify key drivers of nutrient dynamics.  
- Provide actionable insights for water managers and researchers.

---

##  Dataset
- **Source**: Commonwealth Environmental Water Holder (CEWH) – Flow-MER program.  
- **Period**: January 2014 – June 2023.  
- **Data Size**: ~9.5 years of daily records from multiple stations.  

**Features used**:
- `Flow` – daily river discharge (primary hydrological driver).  
- Temporal indicators: `Year`, `Month`, `Day`, `Weekday`.  

**Prediction targets (7 variables)**:
1. Salinity load  
2. Phosphate load  
3. Particulate Organic Phosphorus (POP)  
4. Ammonium load  
5. Particulate Organic Nitrogen (PON)  
6. Dissolved Silica load  
7. Chlorophyll-a load  

---

##  Methodology
1. **Data Preprocessing**  
   - Missing values handled via mean imputation.  
   - Outliers removed using IQR/Z-score methods.  
   - Min-Max scaling applied for SVR/LSTM to normalize ranges.  

2. **Feature Engineering**  
   - Extracted temporal features (year, month, weekday).  
   - Created lagged flow features to capture delayed hydrological effects.  
   - Categorized flow into discrete bins (low, medium, high).  

3. **Models Implemented**  
   -  **Random Forest (RF)**: ensemble of decision trees, interpretable via feature importance.  
   -  **Support Vector Regression (SVM, RBF kernel)**: captures non-linear interactions in high-dimensional space.  
   -  **LSTM**: neural network for sequential dependencies, designed for time-series forecasting.  

4. **Hyperparameter Tuning**  
   - RF & SVM → `GridSearchCV` (cross-validation).  
   - LSTM → `Keras Tuner` with early stopping.  

5. **Evaluation Metrics**  
   - R² (variance explained)  
   - Adjusted R² (penalised for irrelevant features)  
   - Mean Squared Error (MSE)  
   - Root Mean Squared Error (RMSE)  

---

##  Results

### Model Comparison
| Model          | R²      | Adjusted R² | MSE     | RMSE   |
|----------------|---------|-------------|---------|--------|
| **SVM**        | 0.96239 | 0.95235     | 321.83  | 17.94  |
| Random Forest  | 0.91361 | 0.92456     | 739.32  | 27.19  |
| LSTM           | 0.58018 | 0.58263     | 3292.81 | 57.38  |

- **SVM** consistently achieved the **highest accuracy** across all nutrients.  
- **RF** performed well but was less precise during extreme events.  
- **LSTM** underperformed due to dataset size and noise (≈9.5 years is limited for deep learning).  

### Key Findings
- **River Flow** = dominant predictor for all nutrients.  
- **Seasonality (month, year)** = secondary influence, especially for chlorophyll-a.  
- **Daily/weekly indicators** = minimal impact.  
- **Prediction errors** = lowest for phosphate and ammonium, highest for PON and dissolved silica during flood-driven surges.  

### Visualisations
The notebook produces:
- **True vs Predicted time-series plots** (nutrient-by-nutrient).  
- **Residual error histograms**.  
- **Feature importance bar charts**.  
- **Decision boundary visualisations** (SVM).  

---

##  Implications
- For **researchers**: Kernel-based ML (SVM) is highly effective for **medium-scale hydrological datasets**, outperforming deep learning in this case.  
- For **practitioners**: SVM models can be integrated into **real-time monitoring systems**, supporting proactive nutrient management (e.g., reservoir releases, runoff control).  
- For **policy makers**: The dominance of flow highlights the importance of **flow regulation** in reducing nutrient pollution risk.  

---

##  Limitations
- Limited environmental covariates (no precipitation, turbidity, or land-use data).  
- Event-driven peaks (flood surges) not fully captured.  
- Trained on one basin only; generalisation to other catchments untested.  
- Data quality remains a dependency (sensor errors, missing values).  

---

##  Future Work
- Expand feature set with **precipitation, turbidity, water temperature, and land use**.  
- Develop **hybrid models** combining physical process models with ML.  
- Train event-specific submodels for **flood/drought nutrient spikes**.  
- Test **cross-basin transferability** to other Australian or global rivers.  
- Deploy into **real-time dashboards** for water managers.  

---

##  Requirements
Install dependencies before running the notebook:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn tensorflow keras-tuner
