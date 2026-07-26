# neuroblastoma-outcome-prediction
Machine learning analysis comparing RNA-Seq and Microarray platforms for clinical outcome prediction in neuroblastoma, with lean gene signature optimisation and clinical validation

# Evaluating the Clinical Utility of RNA-Seq vs. Microarray Data for Outcome Prediction in Neuroblastoma

**Module:** LM Data Analytics & Statistical Machine Learning (37237 & 37911)  
**Institution:** University of Birmingham Dubai  
**Group:** 3

## Overview

This project investigates whether RNA-Seq's greater transcriptomic resolution improves clinical outcome prediction over Microarray in neuroblastoma, and identifies the minimum gene set needed for accurate classification. Using the SEQC/MAQC-III cohort (498 patients), we trained and evaluated machine learning models across six clinical endpoints on both platforms, optimised lean gene signatures, and assessed clinical independence from established markers (MYCN, INSS staging).

## Key Findings

- Both platforms achieved strong predictive performance (AUC > 0.90 for death and high-risk endpoints)
- RNA-Seq reaches peak performance with fewer genes — AUC 0.920 at 20 genes vs. Microarray's 0.912 at 50
- At the 10-gene level, RNA-Seq (AUC 0.914) outperforms Microarray (AUC 0.883) with zero gene overlap between platforms
- Signatures are fully independent of MYCN amplification and age
- The 10-gene model correctly identifies 15 of 28 patients (53.6%) misclassified as Favourable by INSS staging
- Cross-platform MI analysis shows 44–99% signal loss when top genes are assessed on the alternate platform

## Repository Structure

```
├── UPDATED-Neuroblastoma_Clinical_Endpoint_Prediction_.ipynb   # Full analysis notebook
├── docs/
│   └── Group_3_Group_Work_with_Individual_Essay_Submission.pdf  # Group essay (2000 words)
├── .gitignore
└── README.md
```

> **Note:** The group presentation (`Machine_Learning_Group_3.pptx`, 322 MB) exceeds GitHub's 100 MB file limit. To include it, use [Git LFS](https://git-lfs.com/) or host it separately (e.g. Google Drive).

## Dataset

**Source:** SEQC/MAQC-III Consortium ([Zhang et al., Genome Biology, 2015](https://doi.org/10.1186/s13059-015-0694-1))

- **Patients:** 498 neuroblastoma samples (249 training / 249 test)
- **RNA-Seq:** 23,146 genes (log2 FPKM) → 13,735 after filtering
- **Microarray:** 44,708 Agilent probes → 5,903 genes after probe-to-gene collapse
- **Endpoints:** Death from disease, progression, high risk (binary); INSS stage (5-class); sex (binary); age at diagnosis (continuous)

> **Note:** Raw data files are not included in this repository due to size. The dataset is publicly available from the [SEQC project on GEO](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE49711).

## Analysis Pipeline

The analysis follows an eight-stage pipeline, with all preprocessing thresholds and feature selection derived exclusively from the 249 training patients to prevent data leakage.

| Stage | Description |
|-------|-------------|
| 1. Data Acquisition | Load RNA-Seq + Microarray expression matrices and patient metadata |
| 2. Preprocessing & QC | ID correction (330 Illumina lane suffixes), variance/sparsity/expression filtering, probe-to-gene collapse |
| 3. Exploratory Analysis | Class imbalance assessment, endpoint correlations, PCA visualisation |
| 4. Unsupervised Learning | K-Means (k=2), hierarchical clustering, silhouette scoring, chi-square enrichment |
| 5. Supervised Classification | 6 models × 2 platforms × 6 endpoints, MI feature selection (top 100), nested CV (3×5 fold) |
| 6. Signature Optimisation | Feature count curve (k=1–100), 10-gene lean signature, platform-specific selection |
| 7. Clinical Validation | MYCN independence test, INSS staging comparison, platform verdict |
| 8. Outcome Prediction | 249 test-set predictions, probability scores, calibration assessment |

## Models Evaluated

- Logistic Regression (L1 / L2 regularisation)
- Random Forest
- Gradient Boosting
- XGBoost
- SVM (RBF kernel)

All models used `class_weight='balanced'` and were tuned via GridSearchCV (3-fold inner CV) nested within 5-fold stratified outer CV.

## Requirements

```
Python 3.12
scikit-learn==1.6.1
xgboost==3.2.0
pandas==2.2.2
numpy==2.0.2
matplotlib==3.10.0
seaborn==0.13.2
scipy
```

Install with:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost scipy
```

## Usage

1. Download the SEQC dataset files (`log2FPKM.tsv`, `allProbIntensities.tsv`, `patientInfo.tsv`) and place them in a `data/` directory
2. Open `UPDATED-Neuroblastoma_Clinical_Endpoint_Prediction_.ipynb` in Jupyter or VS Code
3. Update the `data_dir` path in Section 1.1 to point to your data directory
4. Run all cells sequentially

## References

1. Zhang, W. et al. Comparison of RNA-seq and microarray-based models for clinical endpoint prediction. *Genome Biol.* **16**, 133 (2015).
2. Gupta, M. et al. Development and validation of a 21-gene prognostic signature in neuroblastoma. *Sci. Rep.* **13**, 12526 (2023).
3. Xia, Y. et al. Development and validation of a novel stemness-related prognostic model for neuroblastoma. *Transl. Pediatr.* **13**, 91–109 (2024).
4. Jahangiri, L. Predicting neuroblastoma patient risk groups, outcomes, and treatment response using machine learning methods: a review. *Med. Sci.* **12**, 5 (2024).

## License

This project was completed as part of assessed coursework at the University of Birmingham. The code and analysis are shared for educational purposes.
