# 📋 Startup Success Prediction — Weekly Progress Report

---

## Week 5: Project Foundation & Data Collection

### What is done, studied and implemented:

**Studied:**
- Studied various machine learning algorithms suitable for binary classification — Random Forest, Gradient Boosting, Neural Networks, and their trade-offs.
- Researched Crunchbase dataset structure — understanding startup lifecycle data including funding rounds, investor profiles, company status (acquired, IPO, closed, operating).
- Studied the concept of ETL (Extract, Transform, Load) pipelines for structured data ingestion.

**Implemented:**
- Collected and organized **3 Crunchbase datasets** (`crunchbase-companies.csv`, `crunchbase-rounds.csv`, `crunchbase-investments.csv`) and an additional `big_startup_secsees_dataset.csv` containing **66,000+ startups**.
- Set up the project structure with Python virtual environment and `requirements.txt`.
- Created `setup_db.py` to initialize the SQLite relational database.
- Defined the database schema with 3 core tables: `startups`, `funding_rounds`, and `founders`.

---

## Week 6: Data Engineering & ETL Pipeline

### What is done, studied and implemented:

**Studied:**
- Studied relational database design principles — normalization, foreign key relationships between startups, funding rounds, and investors.
- Studied Pandas DataFrame operations for data cleaning, merging, and transformation.
- Researched encoding techniques (Label Encoding) for categorical variables.

**Implemented:**
- Built `etl_pipeline.py` — a complete ETL pipeline that:
  - **Extracts** raw CSV data from multiple sources.
  - **Transforms** column names, data types, and handles encoding.
  - **Loads** cleaned data into SQLite tables with proper schema.
- Successfully loaded **66,368 startups**, **114,984 funding rounds**, and **229,968 investor records** into the database.
- Implemented automatic detection of the larger dataset with fallback to original Crunchbase files.
- Added error handling and row-count reporting for each table load.

---

## Week 7: Feature Engineering & Random Forest Model Training

### What is done, studied and implemented:

**Studied:**
- Studied feature engineering techniques — extracting meaningful predictors from raw data.
- Studied Random Forest algorithm — ensemble of decision trees, bagging, feature importance.
- Studied SMOTE (Synthetic Minority Over-sampling Technique) for handling class imbalance in datasets.

**Implemented:**
- Built `train_advanced_model.py` with an advanced ML pipeline:
  - **6 engineered features**: `total_funding_rounds`, `total_raised_usd`, `total_investors`, `startup_age`, `category_encoded` (industry type), `country_encoded` (geography).
  - Applied **SMOTE** to balance Success vs Failure classes (66K → 120K balanced samples).
  - Trained a **Random Forest Classifier** with 150 estimators and balanced class weights.
  - Achieved **86.86% accuracy** with balanced precision and recall.
- Implemented Label Encoders for `category_code` and `country_code` — saved as `.pkl` files for consistent encoding during prediction.
- Added model versioning — timestamped backups saved to `model/versions/` before each retrain.

---

## Week 8: Deep Learning (Neural Network) & Ensemble Model

### What is done, studied and implemented:

**Studied:**
- Studied Multi-Layer Perceptron (MLP) architecture — hidden layers, activation functions, backpropagation.
- Studied feature scaling (StandardScaler) — why neural networks require normalized input.
- Studied ensemble learning — combining multiple models for better predictions.

**Implemented:**
- Built `train_nn_model.py` — a Deep Neural Network training pipeline:
  - **3-layer MLP** architecture (128 → 64 → 32 neurons) with early stopping.
  - Applied StandardScaler for feature normalization.
  - Applied SMOTE for class balance consistency with RF model.
  - Achieved **79.05% accuracy**.
- Designed the **Ensemble Prediction System**: Final prediction = Average of Random Forest (86.86%) + Neural Network (79.05%) confidence scores.
- Saved trained models as serialized `.pkl` files: `startup_success_rf_model.pkl`, `startup_success_nn_model.pkl`, `nn_scaler.pkl`.

---

## Week 9: REST API Development & Backend Architecture

### What is implemented:

- Built `Main.py` — a production-grade **Flask REST API** backend with the following endpoints:

| Endpoint | Method | Purpose |
|---|---|---|
| `/api/health` | GET | Backend health check, model status, accuracy metrics |
| `/api/predict` | POST | Single startup success prediction |
| `/api/predict/batch` | POST | Batch prediction for multiple startups |
| `/api/history` | GET | Retrieve past prediction logs |
| `/api/retrain` | POST | Trigger background model retraining |
| `/api/model/versions` | GET | List all saved model version files |

- Implemented **input validation** — rejects negative values, unrealistic inputs (age > 200, rounds > 100).
- Implemented **prediction history logging** — every prediction is stored in SQLite `prediction_history` table with timestamp, inputs, and results.
- Integrated **NLP Sentiment Analysis** (VADER) on founder bio text — adjusts confidence score by ±10%.
- Integrated **SHAP (Explainable AI)** — returns feature importance values as JSON for frontend charting.
- Enabled **CORS** for frontend-backend communication.
- Secured API keys using `.env` file with `python-dotenv`.

---

## Week 10: React Frontend Dashboard Development

### What is implemented:

- Built a modern **React + Vite** interactive dashboard with 3 tabs:

**🔮 Predict Tab:**
  - Input form with 6 fields: Funding Rounds, Total Raised (USD), Investors, Startup Age, Industry Category, Country Code, and Founder Bio.
  - Real-time prediction display with animated **SVG Confidence Gauge** (circular speedometer).
  - Progress bars showing individual Random Forest and Neural Network confidence scores.
  - NLP Sentiment badge (Positive / Neutral / Negative) with impact explanation.
  - **SHAP Feature Importance Bar Chart** — visual horizontal bars showing which features influenced the prediction most.

**📋 History Tab:**
  - Displays last 15 predictions with color-coded Success/Failure badges.
  - Shows funding amount, rounds, investors, and timestamp for each prediction.

**⚙️ Model Tab:**
  - Displays model metadata: accuracy, trained date, feature count, SHAP availability.
  - "Trigger Model Retrain" button — starts background retraining via API.
  - Feature chips showing all features used in the current model.

- Added **Health Badge** in header — auto-polls backend every 10 seconds, shows Online/Offline status.
- Premium dark theme UI with gradient backgrounds, glassmorphism panels, Inter font, and micro-animations.

---

## Week 11: MLOps Pipeline & Continuous Learning

### What is implemented:

- Built `mlops_pipeline.py` — an automated **MLOps Continuous Learning Engine** that:
  1. **Clearbit API** — Fetches company industry and location data dynamically.
  2. **NewsAPI** — Scans global news for recent startup funding announcements.
  3. **Yahoo Finance API** — Checks live stock market for IPO status.
  4. Logs all fetched data into `mlops_log` table in SQLite.
  5. Versions the current model (timestamped backup).
  6. Retrains both RF and NN models with updated data.

- Runs on a **12-hour automated schedule** using the `schedule` library.
- API keys securely loaded from `.env` file with status display on startup.
- Created `.env.example` with instructions for obtaining free API keys.
- Created `.gitignore` to protect sensitive files (`.env`, model files, database).

---

## Week 12: Integration Testing & Working Status

### Working status of the project:

**✅ Fully Functional End-to-End System:**

| Component | Status | Details |
|---|---|---|
| ETL Pipeline | ✅ Working | Loads 66K+ startups from CSV into SQLite |
| Random Forest Model | ✅ Working | 86.86% accuracy, SHAP explainability |
| Neural Network Model | ✅ Working | 79.05% accuracy, MLP with early stopping |
| Ensemble Prediction | ✅ Working | Combines RF + NN + NLP for final score |
| Flask REST API | ✅ Working | 6 endpoints, input validation, history logging |
| React Dashboard | ✅ Working | 3-tab UI with charts, gauges, and animations |
| MLOps Pipeline | ✅ Working | 3 API integrations, auto-retrain, versioning |
| NLP Analysis | ✅ Working | VADER sentiment on founder bio text |
| SHAP XAI | ✅ Working | Feature importance explanations for every prediction |

**Testing Performed:**
- Tested predictions with various startup profiles (high-funded vs bootstrapped).
- Validated input rejection for negative/extreme values.
- Tested batch prediction API with arrays of multiple startups.
- Verified model versioning creates backups before retraining.
- Tested frontend-backend connectivity and health check polling.
- Verified prediction history is logged and retrievable.

---

## Week 13: Team Roles & Project Status

### Roles of each member:

| Member | Role | Responsibilities |
|---|---|---|
| **Member 1** | ML Engineer & Backend Developer | Random Forest & Neural Network model training, feature engineering, SMOTE balancing, SHAP integration, Flask API development |
| **Member 2** | Data Engineer & MLOps | ETL pipeline development, SQLite database design, MLOps continuous learning pipeline, API integrations (NewsAPI, Clearbit, Yahoo Finance) |
| **Member 3** | Frontend Developer & UI/UX | React dashboard development, SVG confidence gauge, SHAP chart visualization, prediction history UI, responsive design |
| **Member 4** | NLP & Integration Lead | VADER sentiment analysis integration, ensemble model architecture, input validation, end-to-end testing, deployment configuration |

### Current Project Status:
- All core modules are **fully developed and integrated**.
- End-to-end flow works: Data Ingestion → Model Training → API Serving → Dashboard Visualization.
- System supports model versioning, prediction logging, and automated retraining.
- Code is organized into modular script files with clear single-responsibility design.

---

## Week 14: Societal Impact & Advanced Implementations

### What is implemented:

**Advanced Feature Additions:**
- Added industry (`category_code`) and geography (`country_code`) as encoded features — model now considers sector and location.
- Implemented class imbalance handling via SMOTE — ensuring fair predictions for minority classes.
- Added model metadata tracking (`model_metadata.json`) with accuracy and training timestamps.

### Progress towards society:

**Societal Impact:**
1. **For Entrepreneurs:** Provides data-driven insight into whether their startup has key success indicators — helping founders make informed decisions early.
2. **For Investors/VCs:** Acts as a preliminary screening tool to evaluate startup viability before committing capital — reducing investment risk.
3. **For Incubators & Accelerators:** Helps identify which startups in their portfolio need additional support based on predictive risk scores.
4. **For Policy Makers:** Aggregate predictions can reveal which industries and geographies have higher startup failure rates — informing economic policy and support programs.
5. **Transparency via XAI:** SHAP explanations ensure predictions are not "black box" — users understand *why* a prediction was made, promoting trust and accountability in AI.
6. **NLP-Driven Insight:** Analyzing founder vision statements adds a qualitative, human-centered dimension to an otherwise purely quantitative model.

---

## Week 15: Plan for Review 3

### Review 3 Plan:

**1. Live Demo (15 min):**
- Start the full system using `RunProject.bat` (one-click launcher).
- Walk through the React dashboard — enter startup metrics, run prediction, show confidence gauge and SHAP chart.
- Demonstrate the History tab showing logged predictions.
- Show the Model tab with retrain button — trigger a live retrain.
- Show API health check and model versioning.

**2. Technical Deep Dive (10 min):**
- Explain the Ensemble Architecture: RF (86.86%) + NN (79.05%) + NLP Sentiment.
- Show the SHAP explainability output — how and why each feature impacts the prediction.
- Walk through the MLOps pipeline — how it connects to 3 live APIs and auto-retrains.
- Discuss SMOTE and how it solved class imbalance in training data.

**3. Architecture Overview (5 min):**
- Present the system architecture diagram: ETL → SQLite → ML Models → Flask API → React Dashboard.
- Explain the data flow from raw CSV to stored prediction history.

**4. Q&A Preparation:**
- Why Random Forest + Neural Network ensemble?
- How does SHAP explain predictions?
- What does the NLP module add to accuracy?
- How does the system handle new, unseen data?
- What makes this production-ready vs a basic Jupyter notebook?

---

## Week 16: PPT Status Before Review 3

### PPT Status:

**PPT is structured and ready with the following slides:**

| Slide # | Title | Content |
|---|---|---|
| 1 | Title Slide | Project title, team members, guide name, date |
| 2 | Problem Statement | Why predicting startup success matters — failure rates, investor losses |
| 3 | Objectives | Build an AI-powered prediction system with explainability and automation |
| 4 | Literature Survey | Existing approaches vs our multi-model ensemble approach |
| 5 | System Architecture | End-to-end flow diagram: Data → ETL → DB → ML → API → Dashboard |
| 6 | Technology Stack | Python, Flask, React, SQLite, Scikit-learn, SHAP, VADER, Vite |
| 7 | Data Engineering | ETL pipeline, 66K+ startups, 3 normalized tables |
| 8 | Feature Engineering | 6 features: funding, investors, age, industry, country + NLP |
| 9 | ML Models | Random Forest (86.86%) + Neural Network (79.05%) + Ensemble |
| 10 | SMOTE & Class Balancing | Before vs After distribution chart |
| 11 | Explainable AI (SHAP) | Feature importance bar chart, sample explanations |
| 12 | NLP Sentiment Analysis | VADER on founder bio — positive/negative/neutral impact |
| 13 | MLOps Pipeline | 3 live APIs, auto-retrain, model versioning |
| 14 | Dashboard Screenshots | Predict tab, History tab, Model tab screenshots |
| 15 | Results & Accuracy | Classification report, confusion matrix, ensemble comparison |
| 16 | Societal Impact | Entrepreneurs, investors, incubators, policy makers |
| 17 | Future Scope | Docker, XGBoost, MLflow, Kubernetes, real-time WebSockets |
| 18 | Conclusion | Summary of achievements and contributions |
| 19 | References | Datasets, libraries, research papers |

**Status:** All slides drafted. Screenshots of working dashboard captured. Architecture diagrams prepared. Ready for final review and formatting.
