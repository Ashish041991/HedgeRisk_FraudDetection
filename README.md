# 🛡️ Enterprise Financial Crimes & Transactional Anomaly Detection Pipeline

An end-to-end, high-performance spatial-temporal machine learning pipeline engineered to ingest, compress, feature-engineer, and model massive e-commerce credit card transaction streams to detect fraudulent anomalies while minimizing ledger-degrading customer false alarms.

🚀 ** Kaggle Late Submission Verified Score:** **0.915191 Public ROC-AUC** (Top 10-15% Global Performance)

---

## 📊 End-to-End Pipeline Architecture & Production Benchmarks

This system models transactional anomaly records by leveraging a relational dataset of over 590,540 rows and 434 highly dense features. To handle high-volume data streams on standard computing infrastructure, the pipeline utilizes compressed chunk streaming ingestion alongside tree boosting algorithms.

### Model Evaluation Leaderboard

| Model Architecture | Data Split Strategy | Global ROC-AUC | PR-AUC (Core Metric) | Status |
| :--- | :--- | :---: | :---: | :---: |
| **LightGBM Classifier** | **Calibrated 1:10 Matrix** | **0.9041** | **0.5086** | 🏆 **Production Champion** |
| LightGBM Classifier | Raw Unbalanced Set | 0.8937 | 0.5089 | Parity Baseline |
| XGBoost Classifier | Calibrated 1:10 Matrix | 0.8964 | 0.4836 | Rejected (Lagging Split Fit) |
| LightGBM Classifier | Synthetic 50/50 Split | 0.8919 | 0.4780 | Rejected (Overfitting Distortion) |

---

## 🛠️ Advanced Risk Engineering Checklist

*   **RAM-Optimized Chunk Streaming:** Replaced monolithic dataset loading with a custom 50k streaming chunk framework. By indexing transaction profiles via key lookups and downcasting 64-bit attributes to 16/32-bit types, memory usage dropped by over 85%, preventing systemic memory crashes.
*   **Progressive Time-Series Partitioning:** Rejected random train-test splitting in favor of a strict chronological timeline split via `TransactionDT`. This safeguards the pipeline against future-data leakage and reflects real-world banking deployment scenarios.
*   **Transactional Network Interaction Engineering:** Built high-utility domain features mapping immediate transaction size deviation averages against card groups (`Amt_Deviation_From_Card_Avg`) and transit reliance frequencies (`Amt_to_Dist_Ratio`).
*   **Card-to-Email Network Proxies:** Engineered an identity network proxy cross-matching individual card IDs with localized purchaser email domains (`Card_Email_Network_Proxy`), capturing malicious merchant network exploitation patterns.
*   **Hyphen Schema Alignment:** Deployed an automated dynamic string realignment wrapper to resolve naming layout discrepancies between the training data (`id_12`) and test data schemas (`id-12`) to eliminate runtime evaluation failures.

---

## 🔬 Operational Ledger Cost-Minimization Matrix

In financial crime engineering, evaluating a model purely on traditional mathematical metrics (like a default `0.50` probability cutoff) degrades bank capital. To resolve this, a custom financial loss allocation function was integrated to calculate exact operational ledger performance based on real-world constraints:
*   **Missed Fraud (False Negative) Cost:** Average chargeback reimbursement loss of **€150** per instance.
*   **False Alarm (False Positive) Cost:** Customer friction processing and automated verification fee of **€10** per hold.

### The Operational Cost Curve Discovery
By conducting a full search across the continuous probability spectrum, the pipeline uncovered that an arbitrary target shift (like chasing an 85% interception recall threshold at `0.3363`) inadvertently triggered a massive wave of false positives that cost the bank an extra **-€49,510** in overhead. 

Instead, the optimization engine mapped the absolute financial sweet spot at **`0.5222`**. This minimized combined system loss down to **€271,850**, unlocking **€1,510 in net savings** for the bank ledger over standard parameters while defusing over 1,120 customer false alarms.

---

## 💻 Local Replication & Verification

To run this model inference engine locally, clone the workspace repository and trigger the application script:

```bash
# Clone the repository
git clone https://github.com
cd YOUR_REPO_NAME

# Install production-pined package wheels
pip install -r requirements.txt

# Run testing validation scripts
python run_inference.py
```

### Core Repository Files Layout
*   `app.py`: Production inference interface engine script.
*   `housing_pipeline.pkl`: Serialized model weight dictionary asset file.
*   `lgb_fraud_submission.csv`: Kaggle verified submission prediction output matrix.
*   `requirements.txt`: Software package dependency tracking.
