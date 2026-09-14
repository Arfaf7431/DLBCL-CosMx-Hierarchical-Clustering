# DLBCL-CosMx-Hierarchical-Clustering
MSc thesis project evaluating hierarchical clustering and cell-type annotation on CosMx spatial transcriptomics data in DLBCL.
# Clustering and Annotation of Cell Types in DLBCL via Spatial Transcriptomics

## Overview
This repository contains the MSc thesis research project investigating the cellular architecture and microenvironment of Diffuse Large B-Cell Lymphoma (DLBCL) using single-cell spatial transcriptomics data.

- **Author:** Arfa Fathima
- **Supervisor:** Dr. Hamidreza Arjmandi
- **Institution:** University of Birmingham
- **Degree:** Master of Science in Bioinformatics (August 2025)
- **Dataset:** CosMx Spatial Molecular Imager (11,382 cells × 989 targeted genes across 4 Fields of View)

---

## Abstract
Diffuse large B-cell lymphoma (DLBCL) is characterized by significant biological and clinical heterogeneity. Using CosMx Spatial Molecular Imaging at single-cell resolution, four fields of view comprising 11,382 cells and 989 genes were profiled from a diagnostic DLBCL specimen. 

By applying distance- and linkage-based agglomerative hierarchical clustering ($k=6$) in a 12-dimensional PCA space, five biologically coherent tissue compartments were resolved:
1. **Activated B cells**
2. **B cell / Plasmablast / Plasma continuum**
3. **Fibroblasts (Stromal ECM)**
4. **Macrophages / Monocytes**
5. **Secretory epithelial / Glandular cells**

Cluster 1 localized to tissue margins and low-capture corridors; post-hoc spatial audit confirmed it as a low-quality / edge-capture artifact, and it was excluded from biological interpretation.

---

## Computational & Methodological Pipeline

[Raw CosMx Seurat RDS (sample_1_Anne_FOV103_106.rds)]
│
▼
[R Export to Plain-Text CSV]
│
▼
[QC & Filtering (Python / Scanpy AnnData)]
├── Total counts > 1,000 (12 cells removed)
├── Detected genes per cell > 350 (10 cells removed)
└── Mitochondrial content < 15% (0 cells removed)
│
▼
[Library-Size Normalization & Log Transformation]
├── sc.pp.normalize_total + log1p transformation
└── Unscaled log1p matrix retained for fold-change estimation
│
▼
[Feature Selection & Dimensionality Reduction]
├── Percentile HVGs (Top 25% mean expression & dispersion)
└── PCA on scaled matrix -> 12 Principal Components retained
│
▼
[Agglomerative Hierarchical Clustering]
├── Euclidean distance with Ward's minimum variance linkage
└── Cut tree at k = 6 based on CH, DB, and Silhouette indices
│
▼
[Triangulated Differential Expression Testing]
├── Wilcoxon Rank-Sum Test (BH-FDR adjusted)
├── Random Forest Classifier (5-fold CV, MDI feature importance)
└── MAST Hurdle Model (Two-part likelihood ratio test)
│
▼
[Cell-Type Annotation & Spatial Audit]
├── Cross-validation against ACT, CellMarker 2.0, CellKb & CZ CellxGene
└── Coordinate overlay across FOVs 103–106 to validate edge artifacts
Here is the corrected and fully updated `README.md` template reflecting both changes:

1. **Cluster 1** is correctly marked as the low-quality / edge-artifact cluster.


2. The thesis link updated to `MSc_Thesis_Spatial_Transcriptomics_DLBCL.pdf`.



---

### Instructions

1. Go to your GitHub repository and click on **`README.md`**.
2. Click the **pencil icon** to edit the file.
3. Replace the entire content with the template below.
4. Click **Commit changes**.

---

### Updated `README.md` Template

```markdown
# Clustering and Annotation of Cell Types in DLBCL via Spatial Transcriptomics

## Overview
This repository contains the MSc thesis research project investigating the cellular architecture and microenvironment of Diffuse Large B-Cell Lymphoma (DLBCL) using single-cell spatial transcriptomics data.

- **Author:** Arfa Fathima
- **Supervisor:** Dr. Hamidreza Arjmandi
- **Institution:** University of Birmingham
- **Degree:** Master of Science in Bioinformatics (August 2025)
- **Dataset:** CosMx Spatial Molecular Imager (11,382 cells × 989 targeted genes across 4 Fields of View)

---

## Abstract
Diffuse large B-cell lymphoma (DLBCL) is characterized by significant biological and clinical heterogeneity. Using CosMx Spatial Molecular Imaging at single-cell resolution, four fields of view comprising 11,382 cells and 989 genes were profiled from a diagnostic DLBCL specimen. 

By applying distance- and linkage-based agglomerative hierarchical clustering ($k=6$) in a 12-dimensional PCA space, five biologically coherent tissue compartments were resolved:
1. **Activated B cells**
2. **B cell / Plasmablast / Plasma continuum**
3. **Fibroblasts (Stromal ECM)**
4. **Macrophages / Monocytes**
5. **Secretory epithelial / Glandular cells**

Cluster 1 localized to tissue margins and low-capture corridors; post-hoc spatial audit confirmed it as a low-quality / edge-capture artifact, and it was excluded from biological interpretation.

---

## Computational & Methodological Pipeline


```

[Raw CosMx Seurat RDS (sample_1_Anne_FOV103_106.rds)]
│
▼
[R Export to Plain-Text CSV]
│
▼
[QC & Filtering (Python / Scanpy AnnData)]
├── Total counts > 1,000 (12 cells removed)
├── Detected genes per cell > 350 (10 cells removed)
└── Mitochondrial content < 15% (0 cells removed)
│
▼
[Library-Size Normalization & Log Transformation]
├── sc.pp.normalize_total + log1p transformation
└── Unscaled log1p matrix retained for fold-change estimation
│
▼
[Feature Selection & Dimensionality Reduction]
├── Percentile HVGs (Top 25% mean expression & dispersion)
└── PCA on scaled matrix -> 12 Principal Components retained
│
▼
[Agglomerative Hierarchical Clustering]
├── Euclidean distance with Ward's minimum variance linkage
└── Cut tree at k = 6 based on CH, DB, and Silhouette indices
│
▼
[Triangulated Differential Expression Testing]
├── Wilcoxon Rank-Sum Test (BH-FDR adjusted)
├── Random Forest Classifier (5-fold CV, MDI feature importance)
└── MAST Hurdle Model (Two-part likelihood ratio test)
│
▼
[Cell-Type Annotation & Spatial Audit]
├── Cross-validation against ACT, CellMarker 2.0, CellKb & CZ CellxGene
└── Coordinate overlay across FOVs 103–106 to validate edge artifacts

```

---

## Key Methodological Details

### 1. Data Quality Control
- **Input Specimen:** Single diagnostic DLBCL specimen from CCLG Vivo Biobank processed by Dr. Matthew Pugh and imaged at Birmingham Tissue Analytics.
- **Initial Matrix:** 11,404 cells × 989 targeted genes (`sample_1_Anne_FOV103_106.rds`).
- **Post-QC Yield:** **11,382 high-quality cells** and **989 genes**.

### 2. Hierarchical Clustering & Parameter Rationale
- **Embedding Space:** 12 Principal Components derived from scaling HVGs.
- **Linkage Criterion:** **Ward’s method** on Euclidean distance matrix to minimize within-cluster variance.
- **Cluster Selection ($k=6$):**
  - **Calinski-Harabasz Index:** Peak value of 2,384.69 (maximizing between-cluster separation relative to within-cluster dispersion).
  - **Davies-Bouldin Index:** Minimized score of 1.654 (indicating compact, separated partitions).
  - **Silhouette Coefficient:** Average score of 0.163, reflecting expected continuous transitions across immune differentiation states in lymphoid tissues.

### 3. Differential Expression & Marker Discovery
To prevent method-specific bias, markers were triangulated using three distinct statistical paradigms in a one-versus-rest design (requiring $>25\%$ within-cluster detection prevalence):
- **Wilcoxon Rank-Sum:** Non-parametric ranking robust to zero-inflated distributions.
- **Random Forest:** Stratified 5-fold cross-validation extracting class-specific Mean Decrease in Impurity (MDI).
- **MAST Hurdle Model:** Combines a discrete logistic regression component (gene detection rate) with a continuous Gaussian component (abundance level among positive cells).

---

## Annotated Cell Clusters

| Cluster ID | Final Annotation | Key Diagnostic Markers | Biological Justification & Spatial Notes |
| :---: | :--- | :--- | :--- |
| **Cluster 1** | Low-quality / Edge Artifact | Housekeeping-biased genes | Lacks 3-way DE consensus; concentrates along tissue margins/lumen voids in spatial overlays. Excluded from biological interpretation. |
| **Cluster 2** | Activated B cell | *TPT1, IFITM1, IGKC, CD74* | Mixed immune activation signature across B, T, and antigen-presenting cells. |
| **Cluster 3** | B cell / Plasmablast continuum | *CD19, MS4A1, CD79A, IRF4, MZB1, JCHAIN* | Canonical B-cell lineage markers co-expressed with plasma cell differentiation drivers. |
| **Cluster 4** | Fibroblasts (Stromal ECM) | *COL1A1, COL3A1, FN1, LUM, DCN* | Rich expression of extracellular matrix and structural genes. |
| **Cluster 5** | Macrophages / Monocytes | *LYZ, CD68, TYROBP, APOE, C1QA/B/C* | Classic myeloid markers and antigen-presentation programs. |
| **Cluster 6** | Secretory Epithelial cells | *KRT19, KRT8, MMP7, SERPINA1, CLU, VEGFA* | Cytokeratins combined with glandular secretory/tissue injury markers. |

---

## Thesis Document
The full Master's dissertation document is available in this repository:
- [`MSc_Thesis_Spatial_Transcriptomics_DLBCL.pdf`](./MSc_Thesis_Spatial_Transcriptomics_DLBCL.pdf)

## Software Stack & Tools
- **Languages:** R, Python
- **Libraries:** `Scanpy`, `Pandas`, `NumPy`, `Scikit-Learn`, `SciPy`, `MAST`, `Seurat`, `Matplotlib`, `Seaborn`
- **Reference Catalogues:** ACT, CellMarker 2.0, CellKb, CZ CELLxGENE Discover

```
