# League of Legends – Win Prediction Model

## 📌 Project Overview

This project builds a **machine learning model to predict the winning team in a League of Legends ranked match** using real match data.

Unlike many approaches that rely on final-game statistics (gold, total kills, total towers), this project focuses on a **realistic and explainable prediction**, emphasizing:
- early-game actions,
- team composition,
- champion scaling across game phases,
- and match duration effects.

The goal is not only prediction accuracy, but also **understanding which strategic factors influence victory**.

---

## 📊 Dataset

The data comes from the Kaggle dataset:

**League of Legends Ranked Games – by Paololol**

Several CSV files were combined and processed into a single final dataset:

- `matches.csv` – general match information (duration, version, etc.)
- `participants.csv` – player-level champion data
- `teamstats.csv` – aggregated team statistics
- additional objective-related datasets

All data preprocessing results in:

📁 **`lol_final.csv`**  
(one row per match, one target variable: `winner`)

---

## 🧠 Feature Engineering

### Team Composition (Target Encoding)
Champion strength is encoded using **target encoding**, summarizing historical win rates instead of one-hot encoding champions.

### Game Phase Modeling
Matches are divided into:
- Early game (0–15 min)
- Mid game (15–30 min)
- Late game (>30 min)

Champion performance is estimated **per phase** using **Out-Of-Fold target encoding** to avoid data leakage.

### Late Game Weighting
A **sigmoid-based multiplier** increases the impact of late-game compositions as match duration grows, modeling champion scaling realistically.

Interaction features between composition and duration are also included.

---

## 🧹 Data Preparation

- Removal of identifier and non-informative variables
- Elimination of data leakage (final-game statistics)
- Removal of remake matches (duration < 300 seconds)
- Controlled handling of missing values
- Final feature selection focused on early actions and composition

---

## 🤖 Models

The following classifiers were evaluated:
- Logistic Regression
- Random Forest
- Gradient Boosting
- **XGBoost** (selected model)

### Evaluation Metrics
- Accuracy
- F1-score
- **ROC-AUC** (primary metric)

---

## ⚙️ Hyperparameter Optimization

XGBoost was optimized using:
- **RandomizedSearchCV**
- Cross-validation (CV = 3)
- Optimization metric: ROC-AUC

Randomized Search was chosen for efficiency given the size of the hyperparameter space.

---

## 📈 Results

Final model performance on the test set:
- Accuracy ≈ **0.82**
- F1-score ≈ **0.83**
- ROC-AUC ≈ **0.91**

The model generalizes well and avoids overfitting.

---

## ❌ Error Analysis

Most prediction errors occur in:
- very long and evenly matched games,
- scenarios where early advantage conflicts with late-game scaling.

These cases reflect the intrinsic uncertainty of competitive matches.

---

## 📁 Repository Structure

├── data/
│ ├── matches.csv
│ ├── participants.csv
│ ├── ...
│ └── lol_final.csv
├── LoL_WinPrediction.ipynb
└── README.md

---

## ✅ Key Takeaways

- The model avoids trivial predictors and focuses on strategic factors.
- Feature engineering plays a central role in performance.
- Cross-validation is applied both in feature creation and model optimization.
- The approach is realistic, interpretable, and robust.

---

## 👤 Author
Daniel García Nilo
Project developed for academic purposes in Machine Learning / Data Science.
