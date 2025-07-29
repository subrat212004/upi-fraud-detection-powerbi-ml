#  UPI Transaction Fraud Detection – End-to-End Project

### Tools Used:
**Python (Pandas, scikit-learn) | Power BI | (SQL Skipped due to volume)**

---

### Project Overview

This project focuses on detecting fraudulent UPI transactions using real-world financial data. With over **2 million+ transactions**, we perform deep analysis, feature engineering, and machine learning modeling to accurately detect frauds. A professional Power BI dashboard was built to visualize key fraud insights interactively.

---

##  Data Cleaning & Preprocessing (Pandas)

### Observations:
- `isFlaggedFraud` column flags only 16 cases out of 8213 frauds.  Not useful; dropped.
- Fraud occurs only in `TRANSFER` and `CASH_OUT`.  Filtered only these.
- `nameOrig` and `nameDest` are just IDs; removed.
- Verified `oldbalanceOrg`, `newbalanceOrig`, `oldbalanceDest`, `newbalanceDest` for integrity.
- Multiple rows with zero balances need caution but preserved after logical filtering.

---

##  Feature Engineering

Created custom fraud indicators:
- `sender_balance_diff`: Difference before and after sender balances
- `receiver_balance_diff`: Difference before and after receiver balances
- `sender_mismatch`: Whether sender's balance update is inconsistent
- `receiver_mismatch`: Whether receiver's balance update is inconsistent
- `suspicious_receiver`: Flag if receiver is in top 10 most fraud-associated IDs
- `sender_zero_after`: Sender left with zero balance
- `receiver_zero_before`: Receiver started with zero

---

##  Feature Selection

Final selected features:
```python
features = [
  'amount',
  'sender_balance_diff',
  'receiver_balance_diff',
  'sender_mismatch',
  'receiver_mismatch',
  'suspicious_receiver',
  'sender_zero_after',
  'receiver_zero_before'
]
```
---

## Applied Train_test_split
Standardized scaling.

🧪 Logistic Regression
F1 Score: 0.961

Accuracy: 99.98%
Classification Report:
               precision    recall  f1-score   support

           0       1.00      1.00      1.00   2224042
           1       0.93      1.00      0.96      2875

    accuracy                           1.00   2226917
   macro avg       0.96      1.00      0.98   2226917
weighted avg       1.00      1.00      1.00   2226917

##  Power BI Dashboard

 designed an interactive and insight-rich dashboard using **Power BI**

###  Key KPIs Displayed:

- **Total Transactions**
- **Total Fraud Count**
- **Total Transaction Amount**
- **Total Fraud Amount**

Each KPI card is styled with bold, white font on a transparent background for contrast 

---
##  Final Conclusion & Observations

Based on extensive analysis and interactive Power BI visualizations, here are the most impactful findings:

###  Key Observations:

- **Fraud is highly concentrated in specific transaction types**:  
  > Only `TRANSFER` and `CASH_OUT` transaction types have fraud cases.  
  ➤ Other types like `PAYMENT`, `DEBIT`, and `CASH_IN` showed **0% fraud**.

- **Time Step Anomalies**:  
  > Fraudulent activity **spikes heavily during specific steps**.  
  - **Top 10 steps** revealed concentrated fraud counts.  
   Suggests fraud tends to cluster in particular transaction batches or windows.

- **Balance Mismatches are Key Indicators**:  
  - **Sender Mismatch** (where sender’s balance does not update correctly) is present in **over 90% of fraud cases**.  
  - **Receiver Mismatch** also shows strong correlation, though slightly less frequent.  
   These are critical red flags for real-time fraud detection.

- **Receiver Suspiciousness**:  
  > Certain destination IDs appear repeatedly across multiple fraud transactions.  
   Flagging these as “suspicious receivers” can proactively stop future fraud.

- **Fraud Amount is a Tiny Fraction of Total**:  
  > While the **number of frauds** is significant, the **total fraud amount is relatively low** compared to the full transaction volume.  
   This makes **volume-based fraud detection** less useful than **pattern-based** methods.

- **Fraud occurs fast**:  
  > Fraudulent transactions often leave **sender balance as zero** immediately after execution.  
   A strong sign of “drain-and-run” behavior.












