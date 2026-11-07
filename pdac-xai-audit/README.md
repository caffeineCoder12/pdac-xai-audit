# Auditing post-hoc explanations of a multimodal pancreatic cancer survival model

Code for the paper *"Analytic choices, not the choice of explainer, drive disagreement between post-hoc explanations of multimodal pancreatic cancer survival models"* (B Madhuvanthi, Raunit Raj, Vijayashree; VIT Vellore).

We train a gated clinical + genomic fusion Cox network on TCGA-PAAD (n = 177) and audit eight post-hoc explainers (KernelSHAP-200, KernelSHAP-4096, LIME, Integrated Gradients, GradientSHAP, DeepLIFT, permutation importance, PDP) across a 36-pipeline preprocessing grid, with exact modality Shapley values, equivalence tests (TOST), a PDAC-only subset and four GEO validation cohorts.

## Notebooks (run in order)

| Notebook | What it does |
|---|---|
| `notebooks/01_main_harness.ipynb` | Builds the log2 dataset, trains the fusion model and baselines, runs all eight explainers and the faithfulness metrics (`PREPROCESSING = 'log2_loggenes'`) |
| `notebooks/02_partD_robustness.ipynb` | Preprocessing grid, estimator budget, seeds, exact modality Shapley, external GEO cohorts |
| `notebooks/03_partE_equivalence.ipynb` | Equivalence tests against IG, out-of-fold metrics, molecular subtypes |
| `notebooks/04_partF_external_cohorts.ipynb` | PDAC-only cohort, extra clinical variables, final summary |

`archive/` holds earlier versions of the main harness (v1–v4), kept for history only.

## Running

1. Open each notebook in Google Colab (GPU optional) and mount Google Drive when asked.
2. Upload `tcga_paad_clean.csv` (clinical + outcome table exported from cBioPortal, see below) to the Colab session for notebook 01.
3. Run notebooks 01 → 04 with *Run all*. Checkpoints are written to `MyDrive/pancreatic_paper_checkpoints_log2_loggenes/{partD,partE,partF}`, so an interrupted run resumes where it stopped.
4. Parts E and F have a `QUICK_RUN` flag (default `False`). Setting it to `True` gives a ~15-minute smoke test whose numbers are not the paper's.

Full runtime is several hours on a Colab GPU.

## Data

No data are stored in this repository. All sources are public:

- TCGA-PAAD PanCancer Atlas: cBioPortal study `paad_tcga_pan_can_atlas_2018` (RNA file downloaded automatically from the cBioPortal datahub); extra clinical fields from the GDC.
- GEO: GSE62452, GSE28735, GSE71729, GSE57495 (downloaded automatically).

## Requirements

Python 3.10+, see `requirements.txt` (Colab has most of these preinstalled).

## Citation

See `CITATION.cff`. If you use this code, please cite the paper.

## License

MIT
