# XGBoost: Finding the Higgs Boson

NTI Machine Learning Activity Day: our group's study of **XGBoost (eXtreme Gradient Boosting)**, with a presentation, an interactive demo and a full notebook that applies XGBoost to real CERN data.

**Team:** Marwan Mohammed Mostafa · Ammar Ibrahim Ibrahim · Belal Salah

## What's in this repo

| File | What it is |
|------|------------|
| `Higgs_Boson.ipynb` | The full notebook: EDA → preprocessing → tuning → evaluation → SHAP → model comparison |
| `XGBoost_Presentation_v5.pptx` | The presentation (3 parts: XGBoost explained, the math, the code) |
| `xgboost_interactive_demo.html` | Interactive boosting demo: add trees one by one and change λ and η (open in any browser) |
| `xgb_higgs_model.json` | The final trained XGBoost model |
| `presentation_images/` | All plots and images used in the slides |
| `requirements.txt` | Exact package versions |

## The dataset

ATLAS Higgs Boson Machine Learning Challenge 2014 from **CERN Open Data**: <https://opendata.cern.ch/record/328>

Each row is one simulated proton collision; the task is to classify it as **signal** (a Higgs boson was produced) or **background**. We used the 250,000 original training events (30 features).

> Download `atlas-higgs-challenge-2014-v2.csv.gz` from the link above and put it in this folder before running the notebook (no need to unzip it).

## Key results

| Model | Test ROC-AUC | Training time |
|-------|-------------:|--------------:|
| **XGBoost (tuned)** | **0.911** | 5.3 s |
| LightGBM | 0.910 | 0.7 s |
| sklearn GradientBoosting | 0.909 | 355 s |
| Random Forest | 0.905 | 14.3 s |
| Logistic Regression | 0.812 | 0.6 s |

- Tuned XGBoost: **84.3% accuracy**, early stopping at 778 trees, train/test AUC gap of only 0.027
- About **400× faster** than scikit-learn's classic gradient boosting with the same settings
- SHAP showed the model learned the **Higgs mass peak at ~125 GeV** on its own
- We verified XGBoost's leaf-value formula **w = −G/(H+λ)** and its gain formula by hand: they match exactly
- Feature engineering and extra data raised the AUC to **0.914**; a regression version predicts the particle mass with **R² = 0.92**

## How to run

```bash
pip install -r requirements.txt
```

Then open `Higgs_Boson.ipynb` and run all cells (about 11 minutes on a laptop).

## References

- Chen & Guestrin (2016), *XGBoost: A Scalable Tree Boosting System*, KDD: <https://arxiv.org/abs/1603.02754>
- XGBoost documentation: <https://xgboost.readthedocs.io>
- ATLAS collaboration (2014), Higgs Boson ML Challenge dataset, CERN Open Data
