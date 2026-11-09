Dataset link: https://www.kaggle.com/datasets/himadri07/ciciot2023

IoT Intrusion Detection System (IDS)
An end-to-end Machine Learning pipeline designed to classify network traffic, detect malicious cyber threats, and handle extreme class imbalance across large-scale IoT telemetry data (1.17+ million records).

Project Overview & What Was Built:
This project bridges the gap between theoretical machine learning and real-world data engineering by processing messy, high-dimensional network logs to secure modern IoT ecosystems against automated attacks.

Key Engineering Steps & Why I Did Them:
One-Hot Encoding & Feature Alignment (pd.get_dummies & .reindex):
Why: Network logs contain categorical attributes (like protocol types and flags). Aligning features strictly to the model's expected inputs prevents shape mismatch errors during evaluation.

Leakage-Free Feature Scaling (StandardScaler):
Why: To preserve evaluation integrity and prevent data leakage, the scaler was fitted exclusively on the training dataset and saved (scaler.pkl), then independently applied (.transform()) to validation and test partitions.

Class Imbalance Mitigation (Random Over-Sampler & Class Weights)
Why: Network security data is inherently skewed where normal traffic and massive DDoS floods vastly outnumber sophisticated attacks. ROS and class_weight='balanced' were introduced to prevent the model from completely ignoring rare traffic categories.

Decision Tree Model & Hyperparameter Tuning:
Why: A Decision Tree Classifier was tuned using impurity-based split criteria and balanced class weights to map complex decision boundaries across high-dimensional features.

Results Snapshot
Testing Accuracy: 97.94%
Weighted Average F1-Score: 0.98 (High aggregate prediction stability across dominant traffic classes).
Macro Average F1-Score: 0.66 (Proves that class balancing successfully protected moderate minority classes like reconnaissance scans and DNS spoofing from flatlining).

Engineering Insights: Navigating Data Scarcity
While dominant traffic and high-volume DDoS categories achieved near-perfect classification, ultra-rare attack classes (such as XSS and CommandInjection) recorded lower performance metrics.

Engineering Takeaway: Rather than a failure, this highlights a fundamental constraint of real-world data science. With fewer than 100 raw samples out of 1.1 million rows, these classes represent an extreme needle-in-a-haystack distribution. Algorithmic over-sampling can duplicate existing rows, but it cannot synthetically invent complex malicious patterns from a near-total absence of raw data.

Future Improvements Planned:
Advanced Synthetic Generation: Upgrading from basic random over-sampling to SMOTE (Synthetic Minority Over-sampling Technique) to generate realistic feature variations.
Ensemble Methods: Transitioning to gradient boosting models like XGBoost or Random Forests to reduce tree variance.
Anomaly Detection: Framing ultra-rare injections as anomalies using Isolation Forests rather than forcing a multi-class tree to classify invisible data.
