# scRNA-seq Analysis of FSHD Patient Data

End-to-end single-cell RNA-seq analysis of facioscapulohumeral muscular dystrophy (FSHD) using publicly available GEO dataset **GSE122873** (van den Heuvel et al. 2019).

---

## Overview

Facioscapulohumeral muscular dystrophy (FSHD) is caused by aberrant expression of the DUX4 transcription factor in a small subset of muscle cells. This project replicates and extends the analysis from the original paper using Python, independently validating the core finding that DUX4-affected cells are present exclusively in FSHD samples.

---

## What This Analysis Does

- Downloads and processes raw count matrices directly from GEO
- Performs quality control, filtering, normalisation, and feature selection
- Corrects donor batch effects using Harmony
- Identifies cell populations through Leiden clustering and manual marker-based annotation
- Runs differential expression (Wilcoxon) and pathway enrichment (GSEApy/Reactome)
- Applies ML-based cell annotation using CellTypist
- Detects DUX4-affected cells using a multi-biomarker scoring approach

---

## Key Findings

| Metric | This Analysis | van den Heuvel et al. 2019 |
|--------|--------------|---------------------------|
| Cells after QC | 6,950 | ~4,976 |
| Cell populations identified | 9 | 9 |
| Total DEGs (FSHD vs Control) | 1,436 | — |
| DUX4-affected cells | 28 | 23 |
| DUX4 cell ratio | 1 in 248 | 1 in 217 |
| DUX4 cells in Control | 0 | 0 |

All 28 DUX4-affected cells were found exclusively in FSHD samples — zero in controls — independently replicating the paper's central finding.

---

## Pipeline Summary

```
Raw GEO data (GSE122873)
        ↓
Data loading & ENSEMBL → gene symbol mapping (mygene)
        ↓
Quality control (MT%, UMI counts, gene counts)
        ↓
Normalisation (library size + log1p) + HVG selection
        ↓
PCA → Harmony batch correction (donor variable)
        ↓
UMAP + Leiden clustering (resolution = 0.5)
        ↓
Marker gene identification + manual cell type annotation
        ↓
Differential expression (Wilcoxon, FSHD vs Control)
        ↓
Pathway enrichment (GSEApy, Reactome 2022)
        ↓
ML annotation (CellTypist, Immune_All_Low model)
        ↓
DUX4 biomarker scoring + affected cell detection
```

---

## Cell Types Identified

| Cluster | Cell Type | Cells |
|---------|-----------|-------|
| 0 | Skeletal Muscle (Fast) | 1,540 |
| 1 | Skeletal Muscle (Slow) | 1,746 |
| 2 | Macrophages | 471 |
| 3 | Fibroblasts | 156 |
| 4 | Myoblasts | 1,365 |
| 5 | FAPs/Smooth Muscle | 724 |
| 6 | Slow Twitch Muscle | 760 |
| 7 | Proliferating Cells | 155 |
| 8 | Stem-like Cells | 33 |

---

## Tools & Libraries

| Category | Tools |
|----------|-------|
| Core analysis | Scanpy, AnnData |
| Batch correction | Harmonypy |
| Gene mapping | mygene |
| Differential expression | Scanpy (Wilcoxon) |
| Pathway enrichment | GSEApy (Reactome 2022) |
| ML annotation | CellTypist |
| Data handling | Pandas, NumPy |
| Visualisation | Matplotlib, Seaborn |
| Data download | GEOparse |

---

## Requirements

```bash
conda create -n scrna python=3.10
conda activate scrna
pip install scanpy harmonypy gseapy celltypist geoparse mygene \
            scikit-misc pandas numpy matplotlib seaborn jupyter
```

---

## How to Run

1. Clone this repository
```bash
git clone https://github.com/[your-username]/scrnaseq-fshd-analysis.git
cd scrnaseq-fshd-analysis
```

2. Create and activate the conda environment (see Requirements above)

3. Launch Jupyter and open the notebook
```bash
jupyter notebook fshd_analysis_final.ipynb
```

4. Run all cells — the notebook downloads the data automatically from GEO on first run. No manual data download required.

---

## Repository Structure

```
scrnaseq-fshd-analysis/
│
├── fshd_analysis_final.ipynb    # Main analysis notebook
├── README.md                    # This file
├── figures/                     # All generated plots (19 figures)
└── results/                     # Saved AnnData object (.h5ad)
```

---

## Data

- **Source:** NCBI Gene Expression Omnibus
- **Accession:** [GSE122873](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE122873)
- **Reference:** van den Heuvel A. et al. (2019). Single-cell RNA sequencing in facioscapulohumeral muscular dystrophy disease modeling. *Human Molecular Genetics*, 28(7), 1064–1075.

---

## Author

**Alaa Ahmad**
MSc Bioinformatics, University of Edinburgh (2025)
[LinkedIn](https://www.linkedin.com/in/alaa-ahmad-49464915b/)
