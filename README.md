## 📁 Project Structure – *TravelTide*

### 🔹 Root

- [README.md](./README.md) – Project overview and goals  
- [requirements.txt](./requirements.txt) – Python dependencies (pip)  
- [.gitignore](./.gitignore) – Files/folders to exclude from Git  

---

### 📄 01_docs/ – Business and project documentation

- [00_project_structure.md](./01_docs/00_project_structure.md) – Overview of folders, file purposes, and project organization  
- [01_business_use_case.md](./01_docs/01_business_use_case.md) – Business goals, KPIs, context  
- [02_data_requirements.md](./01_docs/02_data_requirements.md) – Required data sources and fields  
- [03_preparing_data.md](./01_docs/03_preparing_data.md) – Data understanding, cleansing steps  
- [04_data_observations.md](./01_docs/04_data_observations.md) – Observations made during EDA  
- [05_cohort_definition.md](./01_docs/05_cohort_definition.md) – Definition of the agreed cohort segmentation  
- [06_feature_decisions.md](./01_docs/06_feature_decisions.md) – Rationale behind feature selection  
- [07_model_evaluation.md](./01_docs/07_model_evaluation.md) – Evaluation metrics and validation  
- [08_bias_injection_log.md](./01_docs/08_bias_injection_log.md) – Log of where and how bias was introduced  
- [09_deployment_next_steps.md](./01_docs/09_deployment_next_steps.md) – Recommendations or deployment steps  
- [10_glossary.md](./01_docs/10_glossary.md) – Domain terms and definitions  

---

### 📊 02_data/ – Organized data storage

- [01_raw/](./02_data/01_raw/) – Original unmodified data  
- [02_external/](./02_data/02_external/) – Third-party or open data  
- [03_interim/](./02_data/03_interim/) – Intermediate cleaned or transformed data  
- [04_processed/](./02_data/04_processed/) – Final datasets ready for analysis/modeling  

---

### ⚙️ 03_config/ – Centralized configuration

- [01_config.yaml](./03_config/01_config.yaml) – File paths, model parameters, etc.  

---

### 🧠 04_src/ – Source code for data handling and modeling

#### SQL
- [01_clean_data.sql](./04_src/01_sql/01_clean_data.sql) – Data cleaning, transformation  
- [02_join_sources.sql](./04_src/01_sql/02_join_sources.sql)  
- [03_feature_engineering.sql](./04_src/01_sql/03_feature_engineering.sql)  

#### Python
- [01_data_preparation.py](./04_src/02_py/01_data_preparation.py) – Data cleaning, transformation  
- [02_modeling.py](./04_src/02_py/02_modeling.py) – Training, scoring, segmentation  
- [03_evaluation.py](./04_src/02_py/03_evaluation.py) – Model evaluation, metric computation  
- [04_utils.py](./04_src/02_py/04_utils.py) – Utility functions (e.g., plots, loaders)  

---

### 📓 05_notebooks/ – Jupyter notebooks in pipeline order

- [01_exploration.ipynb](./05_notebooks/01_exploration.ipynb) – Initial data exploration and visualization  
- [02_cleansing.ipynb](./05_notebooks/02_cleansing.ipynb) – Data cleansing  
- [03_preprocessing.ipynb](./05_notebooks/03_preprocessing.ipynb) – Preprocessing, feature engineering  
- [04_modeling.ipynb](./05_notebooks/04_modeling.ipynb) – Modeling (e.g. clustering, scoring)  
- [05_evaluation.ipynb](./05_notebooks/05_evaluation.ipynb) – Evaluation and interpretation of results  

---

### 📤 06_outputs/ – Outputs generated during analysis

- [01_visuals/](./06_outputs/01_visuals/) – Plots, charts, heatmaps, etc.  
- [02_tables/](./06_outputs/02_tables/) – Exported results (CSV, Excel)  

---

### 📢 07_reports/ – Final reports and project outcomes

- [01_summary.md](./07_reports/01_summary.md) – Summary of results and conclusions  
- [02_presentation.pdf](./07_reports/02_presentation.pdf) – Presentation slides for stakeholders  
- [03_storytelling.md](./07_reports/03_storytelling.md) – Visuals and text modules for narrative presentation of results  
