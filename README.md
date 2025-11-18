# Credit Rating Classification (36106 AT2)

Predict customer credit rating (**Good / Standard / Poor**) for banking use cases:

- **Good** → pre-approvals / cross-sell  
- **Standard** → limit management  
- **Poor** → early warning

This repo includes **EDA**, **data preparation**, a **baseline**, and **four experiments** using **scikit-learn**.  
**Runs locally**.

---

## 1) Project Overview

- **Data**: assignment-provided, 12,500 rows × 48 columns (tabular).  
- **Target**: `credit_rating ∈ {Good, Standard, Poor}`.  
- **Goal**: an **accurate**, **stable**, and **deployable** model (no leakage, no sensitive features).

**Models tested & role**

| Model | Role | Notes |
|---|---|---|
| Decision Tree (CART) | Baseline | Simple, interpretable non-linear start point |
| Random Forest (**RF-SAFE**) | Primary | Balanced performance; lower variance on tabular data |
| Histogram Gradient Boosting (**HGB-SAFE**) | Companion | Better probability calibration for threshold policies |
| SVM (RBF) | Contrast | Shows trade-offs vs. tree models |

---

## 2) Why these models?

- **CART**: clear, explainable baseline.  
- **RF-SAFE**: strong on tabular data; bagging reduces variance; balanced across classes.  
- **HGB-SAFE**: often **better calibrated probabilities** (useful when business rules use thresholds).  
- **SVM (RBF)**: margin-based contrast model to test non-tree behaviour.

> In production we keep **RF-SAFE** as default.  
> Use **HGB-SAFE** when **probability quality (calibration)** matters most.

---

## 3) Why these metrics?

Class sizes are uneven (Standard > Good/Poor). Plain accuracy can mislead.

- **Macro-F1** → averages F1 across classes; treats classes **equally**.  
- **Balanced Accuracy (bAcc)** → averages per-class recall; checks each class is found.  
- *(Optional)* OvR ROC-AUC → additional ranking check.

We report **validation** and **test** results, plus **3-seed mean ± std** (RF-SAFE / HGB-SAFE) in the report.

---

## 4) Repository Structure

/  
├─ 36106/assignment/AT2/data/ # put datasets here  
├─ 36106-25SP-AT2-25948860-EDA.ipynb # EDA  
├─ 36106-25SP-AT2-25948860-EDA.html  
├─ 36106_25SP_AT2_25948860_Preparation.ipynb # Data Preparation  
├─ 36106_25SP_AT2_25948860_Preparation.html  
├─ 36106_25SP_AT2_25948860_Baseline.ipynb # Baseline (CART)  
├─ 36106_25SP_AT2_25948860_Baseline.html  
├─ 36106-25SP-AT2-25948860-experiment-1.ipynb # Exp1 (CART tuned)  
├─ 36106-25SP-AT2-25948860-experiment-1.html  
├─ 36106-25SP-AT2-25948860-experiment-2.ipynb # Exp2 (RF, SAFE vs LEAK)  
├─ 36106-25SP-AT2-25948860-experiment-2.html  
├─ 36106-25SP-AT2-25948860-experiment-3.ipynb # Exp3 (HGB, ablation)  
├─ 36106-25SP-AT2-25948860-experiment-3.html  
├─ 36106-25SP-AT2-25948860-experiment-4.ipynb # Exp4 (SVM)  
├─ 36106-25SP-AT2-25948860-experiment-4.html  
└─ 36106-25SP-AT2-25948860-project_report.docx # Final report  

---

## 5) How to Run

**Python 3.9+** recommended.

Put datasets under: 36106/assignment/AT2/data/

Open and run notebooks in this order:

**EDA**

**Preparation**

**Baseline**

**Experiment 1–4**

Run all cells top → bottom.

See the **project_report.docx** for final numbers, figures, and decisions.

## 6) Notebook Summaries
**EDA**
- Class balance (Good/Standard/Poor)

- Key features and simple plots

**Preparation**
- Remove PII (IDs, names, emails, phones, addresses)

- Fix invalid values (e.g., negative delinquency → 0)

- Build `DTI` and `EMI-to-income`

- Log-transform long-tailed amounts

- **Split 70/15/15** and **fit preprocessors on train only** (avoid leakage)

**Baseline**
- Decision Tree baseline (simple, interpretable)

**Experiments**
- **Exp1 (CART)** — tuned tree baseline

- **Exp2 (RF)** — RF-SAFE (production) and SAFE vs LEAK check

- **Exp3 (HGB)** — HGB-SAFE and policy-variable ablation

- **Exp4 (SVM)** — contrast model; fairness ablation

## 7) Key Results
- **RF-SAFE:** most balanced (macro-F1 & bAcc); stable across seeds

- **HGB-SAFE:** similar accuracy; better calibration (good for thresholds)

- **LEAK features** (e.g., `credit_mix`, `cc_interest_rate`, `credit_limit_change`) inflate metrics → not for deployment

- **Sensitive feature** (`gender`) excluded for fairness/compliance

Full results, reliability plots, and SAFE→LEAK bars are in the **project_report.docx**.

## 8) Ethics & Fairness
- No PII used in modelling

- No sensitive or policy-derived features in production

- Group checks (TPR/FPR). If gaps > 0.10, apply small **group thresholds** (±0.02) with minimal macro-F1 impact

- Indigenous impacts considered (near-threshold human review; clear recourse)

## 9) Reproducibility
- Fixed random seeds

- Train-only fit for all preprocessors and models (prevent leakage)

- **3-seed** mean ± std for RF-SAFE and HGB-SAFE (see report)

- Feature policy documented (SAFE vs LEAK)

## 10) License & Contact
For coursework use

Questions: open an issue in this repo

## TL;DR
Put data in `36106/assignment/AT2/data/` → run **Preparation** → **Baseline** → **Experiments** → read the **report**.
