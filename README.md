# NOKIA_Internship
🔧 Nokia Repair Component Failure Prediction System
This repository contains the implementation of a machine learning-based predictive system to determine which components in TRX or PSU units are likely to fail based on historical repair logs and test error codes. It aims to assist engineers at RAP/OAT/LBTS stations in streamlining the repair process with confidence-backed recommendations.

Project Goal
Build a predictive pipeline that:

Identifies unit type (TRX / PSU)
Analyzes test error codes from Incoming & Module stages
Predicts faulty components likely needing replacement
Provides dashboard visualization and a feedback loop
Stage 1: Model ErrorCombo → CompCode
Stage 2: Map CompCode → OUTMaterialCode
Stage 3: Build rule-based ErrorCombo + CompCode → Debug Suggestion
📊 Dataset Overview
We use a combination of TRX and PSU unit repair records:

Field	Description
Unit ID	Unique identifier of each board
Unit Type	TRX or PSU
Incoming Test Result	Pass/Fail + Error Code
Module Test Result	Pass/Fail + Error Code
Final Replaced Components	Actual replaced components (Target)
Visual Inspection Result	(Optional)
🧹 Data Processing Pipeline
Data Cleaning

Normalize text fields
Remove incomplete rows
One-hot encode or categorize error codes
Create combo identifier: combo_code = IncomingCode_ModuleCode
Feature Engineering

Unit Type (0 = TRX, 1 = PSU)
Incoming & Module Test Error Codes (E10, M210, etc.)
Combined Error Pattern (E10_M210)
Probability from historical combos (if available)
Target

Final replaced components (single or multi-label)
🧠 Model Pipeline
Model Selection

RandomForest → for single label prediction
MultiOutputClassifier(RandomForest) → for multi-component replacement
XGBoost → for optimized accuracy
Rule-Based Fallback → Hard-coded probabilities from known code combos
Training & Evaluation

Split: 80/20 Train/Test
Stratified Sampling (if needed)
GridSearchCV or manual tuning
Metrics:
Accuracy, Precision, Recall, F1-Score
Top-K Accuracy
Confusion Matrix
SHAP-based Feature Importance
Prediction Flow
[Engineer scans board]

↓

[Incoming + Module Test Codes Retrieved]

↓

[Model Input: TRX, E10, M210]

↓

[Output: Component Z + Confidence Level]

🔁 Feedback Loop
Store actual replacement + test outcome (Pass/Fail)
Append to training data
Periodically retrain the model
📊 Dashboard (Optional)
A web interface built using Streamlit or Flask to:

Upload test logs
Display predictions
Visualize logs / model insights
Download reports
Can be used in RAP/OAT/LBTS stations.

Tech Stack
Python (Pandas, Scikit-learn, XGBoost)
Streamlit / Flask (Dashboard)
SHAP / Matplotlib (Explainability)
Future Enhancements
Add anomaly detection for unseen error codes
Cloud-based API endpoint for real-time prediction
Integration with repair station scanners & serial loggers
🙌 Contributors
Data Science: ML model development, SHAP analysis
Firmware/Hardware: Repair data, failure patterns
Backend/Frontend: Dashboard & visualization tools
📄 License
This project is proprietary and currently used for internal purposes at Nokia. License to be defined.
