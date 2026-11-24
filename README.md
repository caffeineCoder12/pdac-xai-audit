# pdac-xai-audit

Code for the paper **"Auditing Post-Hoc Explanations of Multimodal Pancreatic Cancer Survival Models: Analytical Choices Matter More than Explainer Choice"** (B Madhuvanthi, Raunit Raj, J Jayashree, Vijayashree J; School of Computer Science and Engineering, Vellore Institute of Technology), submitted to *BMC Medical Informatics and Decision Making*.

A gated clinical + transcriptomic fusion Cox network is trained on TCGA-PAAD (n = 177) and eight post-hoc explainers are audited (KernelSHAP at 200 and 4,096 coalitions, LIME, Integrated Gradients, GradientSHAP, DeepLIFT, permutation importance, partial dependence): faithfulness in-sample, out-of-fold and by retraining, equivalence tests, sanity checks, planted and analytic ground truth, a 36-pipeline preprocessing grid, exact modality Shapley values, two further model types, a histology-confirmed subset, four GEO cohorts, decision curves and Hallmark pathway agreement.

## Run order

Run the notebooks in Google Colab in this order. Each one reads the checkpoints of the previous ones from Google Drive and writes its own outputs to a new subfolder, so nothing earlier is overwritten. Every slow step checkpoints, so after a disconnect just *Run all* again.

| # | Notebook | Writes to | What it does |
|---|---|---|---|
| 1 | `notebooks/01_main_harness.ipynb` | `pancreatic_paper_checkpoints_log2_loggenes/` | Builds the dataset (`PREPROCESSING = 'log2_loggenes'`), trains the gated fusion network and baselines, runs the explainers, predictive metrics |
| 2 | `notebooks/02_partD_robustness.ipynb` | `.../partD/` | Histology and purity subsets, 36-pipeline preprocessing grid, disagreement decomposition, survival-specific explainers, survival metrics, GEO cohorts |
| 3 | `notebooks/03_partE_equivalence.ipynb` | `.../partE/` | Converged KernelSHAP, GradientSHAP, DeepLIFT; faithfulness under three removal schemes; out-of-fold faithfulness; ROAR; sanity, robustness and ground truth |
| 4 | `notebooks/04_partF_architectures_pdac.ipynb` | `.../partF/` | Equivalence tests, Moffitt subtypes, decomposition with interactions, concatenation network and random survival forest, PDAC-only cohort, extra clinical variables |
| 5 | `notebooks/05_partG_decision_curves_pathways.ipynb` | `.../partG/` | Decision curve analysis at 12 and 24 months; Hallmark pathway-level agreement with a random-gene-set control |

Parts E and F have a `QUICK_RUN` flag; `True` gives a short smoke test whose numbers are not the paper's.

## Data

No data are stored here; all sources are public and downloaded by the notebooks.

- TCGA-PAAD PanCancer Atlas: cBioPortal study `paad_tcga_pan_can_atlas_2018`; histology, purity and extra clinical fields from the GDC.
- GEO: GSE62452, GSE28735, GSE71729, GSE57495.
- Hallmark gene sets: MSigDB (via gseapy / Enrichr, with a direct GMT fallback).

## Requirements

Python 3.10 or later. `pip install -r requirements.txt` (Colab has most packages preinstalled). Versions are the minimums the code was tested with.

## Citation

See `CITATION.cff`.

## License

MIT
