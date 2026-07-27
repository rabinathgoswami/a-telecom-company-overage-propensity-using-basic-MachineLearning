# Company A — Proactive Plan Right-Sizing (Overage Propensity) Model

**Role:** Associate, IT Consulting — Proof-of-Concept engagement for Company A (anonymous telecom operator)

## Project Summary

Company A, a telecom operator, provided two customer-level datasets. This project explores the data,
develops a business hypothesis, and builds a machine learning model to solve it.

**Important note on the target variable:** `churn` is **not** used as the final prediction target. It is
used only as an exploratory lens during EDA — a convenient, pre-labeled signal of "something went wrong"
for a customer — to help identify which business problem is worth solving. The final ML task predicts a
different, business-actionable target: **`is_overage_prone`**, defined as whether a customer regularly
exceeds their plan and incurs overage charges.

### Business Hypothesis
Customers can be identified, from their historical usage-behavior fingerprint alone, as being
"plan-mismatched" — i.e., structurally prone to exceeding their current plan and incurring overage
charges — before that mismatch shows up on a bill. Proactively recommending a right-sized (upgraded) plan
to these customers converts unpredictable, dissatisfaction-linked overage revenue into stable subscription
revenue, while reducing customer-care cost and bill-shock-driven churn risk.

### Final Model
A tuned XGBoost classifier trained on leakage-safe, pre-overage usage-behavior features.

| Model | Accuracy | ROC-AUC | F1 |
|---|---|---|---|
| Logistic Regression (baseline) | 0.7917 | 0.8648 | 0.8080 |
| XGBoost (default) | 0.8820 | 0.9549 | 0.8966 |
| **XGBoost (tuned) — final** | **0.8899** | **0.9582** | **0.9036** |

## Repository Structure

```
.
├── README.md                              # This file
├── Final-notebook.ipynb                   # Main analysis & modeling notebook
├── Company A Dataset Overview.docx        # Column-level data dictionary
├── telecom/
│   ├── Client.csv                         # 1 row per customer: demographics, device, household, plan-usage summary
│   └── Record.csv                         # 1 row per customer: monthly billing & network-usage behavior + churn flag
└── test/
    └── Test_notebook.ipynb                                 # Rough/exploratory notebooks used during earlier experimentation
```

## Notebook Structure (`Final-notebook.ipynb`)

1. Environment Setup
2. Data Ingestion
3. Data Cleaning
4. Broad EDA (Rigorous)
5. Hypothesis Formulation
6. Targeted EDA
7. Bottleneck Identification
8. Business Proposal
9. Preprocessing
10. Model Building
11. Validation & Finalization

## How to Run This Project (Google Colab + Google Drive)

This notebook is built to run in **Google Colab** and reads the data directly from **Google Drive**.
Follow these steps exactly:

### 1. Set up the folder in Google Drive
1. Go to [Google Drive](https://drive.google.com) and create a new folder for this project
   (e.g. `Final_Assignment`), anywhere you like in `My Drive`.
2. Inside that folder, upload:
   - the `telecom` folder (containing `Client.csv` and `Record.csv`)
   - the `Final-notebook.ipynb` file

Your Drive folder should look like:
```
MyDrive/
└── <your-project-folder>/
    ├── Final-notebook.ipynb
    └── telecom/
        ├── Client.csv
        └── Record.csv
```

### 2. Open the notebook in Colab
1. In Google Drive, double-click `Final-notebook.ipynb` (or right-click → **Open with → Google
   Colaboratory**). If you don't see that option, install the "Google Colaboratory" app from
   Google Workspace Marketplace first.

### 3. Point the notebook to your folder
1. In the notebook, find the cell containing this line:
   ```python
   PROJECT_DIR = Path("/content/drive/MyDrive/gciFA/Final_Assignment")  # <-- EDIT ME IF NEEDED
   ```
2. Replace the path inside `Path("...")` with the actual path to the folder you created in Step 1
   (e.g. `/content/drive/MyDrive/<your-project-folder>`).

### 4. Run the notebook
1. Click **Runtime → Run all** (or run cells one by one from the top).
2. The first code cell will attempt to mount Google Drive — a popup will ask you to **allow
   access to your Google Drive**. Click **Allow** and sign in with the account where you saved
   the project folder.
3. Once Drive is mounted, the rest of the notebook will run end-to-end: it loads `Client.csv` and
   `Record.csv`, cleans the data, runs EDA, builds features, trains and tunes the models, and
   outputs the final evaluation metrics and plots.

### Notes
- `RANDOM_STATE = 42` is fixed throughout for reproducibility.
- Additional libraries (`sklearn`, `xgboost`, `scipy.stats`, etc.) are imported later in the
  notebook, right at the point they are first used, rather than all at the top.
- Refer to `Company A Dataset Overview.docx` for full column-level definitions of both source files.

## Data Files

| File | Grain | Content |
|---|---|---|
| `telecom/Client.csv` | 1 row / customer | Demographics, device, household, plan-usage summary stats |
| `telecom/Record.csv` | 1 row / customer | Monthly billing & network-usage behavior + `churn` flag |

Both files are 1:1 on `Customer_ID` and are merged via an inner join.

## Disclaimer

This is a proof-of-concept analysis prepared for an anonymized telecom operator ("Company A") as
part of a consulting engagement / assignment. Business assumptions (e.g. pricing, campaign
acceptance rates) used in the ROI simulation in Section 11 are clearly labeled estimates, not
Company A's actual figures.
