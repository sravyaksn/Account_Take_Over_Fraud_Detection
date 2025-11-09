# 🕵️‍♀️ Detecting Account Takeover (ATO) Fraud Using AI/ML
👩‍💻 Author: Sravya Kakarlapudi
# 📘 Overview

Account Takeover (ATO) fraud occurs when fraudsters gain unauthorized access to a user’s account — often through stolen credentials — and quickly perform sensitive actions like password resets or fund transfers.

In this project, we simulate real-world ATO detection using synthetic online banking transaction data.
The focus is on behavioral pattern recognition through machine learning and feature engineering.

# 🚀 Project Workflow

Data Exploration – Understand transaction-level user events

Feature Engineering – Create behavioral indicators of suspicious activity

Model Training – Build a supervised learning model (Random Forest)

Evaluation – Compare model performance vs. rule-based system

Reflection – Assess how ML and rule systems can work together in production

# 📂 Dataset: ato_transactions.csv

Each row represents a user event in an online banking system.
The dataset simulates login activity, password resets, and fund transfers, including fraud labels.

Column	Description
user_id	Unique identifier for the user account
timestamp	Time of the event
event_type	Type of action (login, reset_password, transfer_funds, etc.)
ip_address	IP address of the event
location	Geographic location
device_id	Device fingerprint used during the event
amount	Dollar value (if applicable)
is_fraud	Target variable — 1 if the event is fraudulent
💡 Behavioral Patterns of ATO Fraud

ATO events often show distinctive patterns such as:

Logging in from a new location or device

Performing sensitive actions (password reset, transfer) shortly after login

Activity during off-hours (e.g., 3 AM)

Rapid event bursts within short time windows

These behavioral anomalies inspired the engineered features below.

# 🧪 Feature Engineering
Key Engineered Features and Rationale
Feature	Description	Fraud Behavior Captured
time_since_last_activity_min	Minutes since user’s last event	Very short gaps → automation; very long gaps → dormant account
login_followed_by_sensitive_30m	Flag if login is followed by sensitive action within 30 mins	Typical ATO sequence
rapid_actions_count_1h	Count of actions in last 1 hour	Bot-like or panic activity
hour_of_day, day_of_week, is_off_hours	Temporal context	Fraud often occurs at odd hours
event_type_enc, event_type_freq	Encoded action types and their frequency	Certain actions are more fraud-prone
user_id_enc	Encoded user identity	Captures user-specific patterns
amount	Transaction value	Unusually high or low amounts are suspicious

These engineered features provide the ML model with richer behavioral context beyond raw data.

# 🧠 Model Building

Algorithm Used: Random Forest Classifier
Handling Class Imbalance: Class weights set to "balanced"
Train/Test Split: 75% / 25% (stratified)

🔍 Top 5 Important Features

event_type_freq

event_type_enc

amount

user_id_enc

time_since_last_activity_min

These confirm that both event patterns and transaction value drive predictive power.

# 📈 Model Evaluation
Metric	Value
ROC-AUC	0.798
Recall (Fraud Class)	0.856
Precision (Fraud Class)	0.030
Accuracy	0.67

⚠️ The model achieves high recall but low precision, which is expected in fraud detection — the goal is to catch as many fraudulent events as possible, even if that means some false positives.

🧩 Rule-Based System vs ML Model

A rule-based risk scoring system was implemented to mimic a traditional fraud detection setup.

System	Precision	Recall	F1
Rule-Based	0.025	0.084	0.039
ML Model (RF)	0.030	0.856	—
⚖️ Interpretation

ML Model → detects much more fraud (high recall)

Rule System → fewer false positives but misses many frauds

In production, hybrid systems combining both are most effective:

Rules for known fraud patterns and quick explainability

ML for adaptive, complex behavior detection

# 🔍 Hyperparameter Tuning

Used GridSearchCV with StratifiedKFold and roc_auc scoring.
Optimal parameters improved AUC and recall while maintaining interpretability.

# 🎯 Key Learnings

Behavioral features (e.g., time gaps, off-hours, sensitive sequences) are critical for ATO detection.

Imbalanced data significantly impacts precision; threshold tuning helps improve recall trade-offs.

Hybrid fraud systems that combine rules and ML are most practical for production.

# 🛠️ Tech Stack

Python, pandas, NumPy

scikit-learn (RandomForestClassifier, GridSearchCV)

Matplotlib / Seaborn for visualization

Precision-Recall analysis for threshold tuning

# 📊 Visualizations

Distribution of risk scores for fraud vs non-fraud

Precision-Recall curve highlighting tuned threshold

Feature importance chart from Random Forest
