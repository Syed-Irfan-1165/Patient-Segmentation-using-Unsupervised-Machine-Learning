# 🩺 Patient Segmentation using Unsupervised Machine Learning (NHANES)

Segmenting patients into clinically meaningful health groups from **demographic, dietary, and
laboratory** data using clustering, and translating the segments into actionable healthcare
insights. Built on the **National Health and Nutrition Examination Survey (NHANES)** dataset from
the NCHS / CDC.

> **Master's-level Machine Learning Mini Project** — fully reproducible (`random_state = 42`).

---

## 📁 Project Structure

```
Patient_segmentation/
│
│
├── notebooks/            # Jupyter notebook for analysis and experimentation
│   ├── NHANES_Patient_Segmentation_PROGRESS.ipynb   
│
│
├── README.md             
└── requirements.txt      # Python dependencies
```

---

## 🎯 Objective

NHANES is an **observational survey with no "patient type" label** — so the segmentation task is
inherently **unsupervised**. We discover the segments from the data, profile them into clinical
archetypes, and derive interventions. The project's primary metric is the **Silhouette Score**, and
any dimensionality reduction must retain **≥ 95 %** of variance.

---

## 🧾 Dataset

- **Source:** <https://wwwn.cdc.gov/Nchs/Nhanes/>
- **Cycle:** 2013–2014 (`DEMO_H`, `DR1TOT_H`, and the `*_H` laboratory files)
- **Raw size:** 9,813 participants × 662 variables across Demographics, Dietary, and Laboratory
  components.
- **Note:** this file contains no physical-exam data (no blood pressure / BMI), so the segmentation
  is built on **cardiometabolic laboratory markers + dietary intake + demographics**.

---

## ⚙️ Methodology (pipeline)

1. **Data understanding & quality audit** — shape, dtypes, missingness, duplicates, outliers.
2. **Cleaning & feature curation** — restrict to adults (≥18); curate ~21 clinical features; drop
   fasting-subsample labs (~68 % missing); median imputation; de-correlation.
3. **EDA** — 25+ visualizations (distributions, group comparisons, correlation heatmap, pair/joint
   plots, radar charts).
4. **Feature engineering** — HbA1c/glucose/cholesterol categories, atherogenic index (TC/HDL),
   diet-quality score, metabolic-risk score.
5. **Feature selection & scaling** — variance threshold, correlation filtering, mutual information;
   **MinMaxScaler** (justified).
6. **Optimal K** — Elbow, Silhouette, Calinski–Harabasz, Davies–Bouldin, Gap statistic.
7. **K-Means clustering** + clinical profiling & rule-based naming.
8. **PCA** (retain ≥95 % variance) + re-clustering; **DBSCAN**; **Agglomerative / GMM / Spectral**.
9. **Validation** — consolidated internal-metric comparison.
10. **Healthcare insights, conclusion, appendix.**

---

## 📊 Key Results

| Item | Value |
|---|---|
| Cohort analysed | **5,686 adults** (age ≥ 18) |
| Clustering features | **23** |
| PCA components for ≥95 % variance | **12** |
| Optimal K | **3** |
| Silhouette (K-Means) | **0.243** |
| Best model (silhouette) | K-Means on PCA (**0.254**) |

**Discovered segments:** `Healthy Younger Adults` · `Moderate Metabolic-Risk (Mixed-age)` ·
`High Cardiometabolic-Risk (Older)`. DBSCAN reveals the population is a smooth **risk continuum**
(one dense mass + outliers) rather than naturally separated islands — which is why a partitional
method (K-Means) is the appropriate segmentation tool here.

---

## 🚀 How to Run

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Launch the notebook (run cells top-to-bottom)
cd notebooks
jupyter notebook NHANES_Patient_Segmentation.ipynb

# --- OR execute headless and regenerate all artefacts ---
cd notebooks
python3 -m nbconvert --to notebook --execute --inplace \
        --ExecutePreprocessor.timeout=600 NHANES_Patient_Segmentation.ipynb
```