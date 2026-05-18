<div align="center">

# Genomics Spatial Methods

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776ab?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Scanpy](https://img.shields.io/badge/Scanpy-1.9%2B-1a7abf?style=for-the-badge)](https://scanpy.readthedocs.io/)
[![Squidpy](https://img.shields.io/badge/Squidpy-1.3%2B-5c4d8a?style=for-the-badge)](https://squidpy.readthedocs.io/)
[![Colab](https://img.shields.io/badge/Google_Colab-Ready-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![License](https://img.shields.io/badge/License-Educational-2e7d32?style=for-the-badge)](.)

**A structured, end-to-end tutorial series on spatial transcriptomics analysis.**

Covering spot-based and single-cell resolution platforms — from raw data loading through spatial statistics, image analysis, and interaction inference — using Scanpy, Squidpy, and SpatialData in Google Colab.

</div>

---

## Table of Contents

- [What Is This Repository](#what-is-this-repository)
- [Repository Structure](#repository-structure)
- [End-to-End Workflow](#end-to-end-workflow)
- [Who This Is For](#who-this-is-for)
- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Module 1 — Spatial Analysis with Scanpy](#module-1--spatial-analysis-with-scanpy)
- [Module 2 and 3 — Visium Analysis](#module-2-and-3--visium-analysis)
  - [2A — Visium Fluorescence](#2a--visium-fluorescence-data)
  - [2B — Visium H&E](#2b--visium-he-data)
  - [Visium Platform Comparison](#visium-platform-comparison)
- [Module 4 — Xenium In Situ Analysis](#module-4--xenium-in-situ-analysis)
- [Data and File Formats](#data-and-file-formats)
- [Software Stack](#software-stack)
- [Running in Google Colab](#running-in-google-colab)
- [Troubleshooting](#troubleshooting)
- [References](#references)

---

## What Is This Repository

Spatial transcriptomics captures gene expression at the location where it occurs within a tissue — preserving the physical context that bulk and single-cell RNA-seq discard. This repository provides a complete, annotated set of notebooks that teach you how to work with spatial data in Python, progressing from foundational concepts through platform-specific workflows.

The four modules are designed to be taken sequentially. Each builds on the data structures, vocabulary, and analytical logic introduced in the one before it. By the end of the series, you will have worked with both spot-based capture (Visium) and subcellular in situ (Xenium) data, understood the differences between imaging modalities, and run spatial statistics including autocorrelation, neighborhood enrichment, co-occurrence analysis, and ligand-receptor interaction inference.

---

## Repository Structure

```
genomics-spatial-methods/
│
├── notebooks/
│   ├── 01_scanpy_spatial_basics.ipynb        # Module 1  — Scanpy fundamentals
│   ├── 02a_visium_fluorescence.ipynb         # Module 2A — Visium fluorescence
│   ├── 02b_visium_hne.ipynb                  # Module 2B — Visium H&E
│   └── 03_xenium_analysis.ipynb              # Module 3  — Xenium in situ
│
├── data/
│   ├── visium_fluo/                          # Fluorescence Visium sample data
│   │   ├── filtered_feature_bc_matrix/
│   │   └── spatial/
│   ├── visium_hne/                           # H&E Visium sample data
│   │   ├── filtered_feature_bc_matrix/
│   │   └── spatial/
│   └── xenium/                               # Xenium in situ sample data
│       ├── cell_feature_matrix/
│       ├── transcripts.csv.gz
│       ├── cells.csv.gz
│       ├── nucleus_boundaries.csv.gz
│       └── morphology_mip.ome.tif
│
├── outputs/
│   ├── figures/                              # All saved plots and spatial maps
│   ├── adata/                                # Processed AnnData .h5ad files
│   └── tables/                               # Exported results (CSVs, gene lists)
│
├── utils/
│   └── helpers.py                            # Shared utility functions
│
├── environment.yml                           # Conda environment specification
├── requirements.txt                          # pip dependency list
└── README.md
```

> **Note on data:** Built-in dataset loaders in Scanpy and Squidpy download sample data automatically when the notebooks are run. You do not need to populate the `data/` directory manually. For your own datasets, follow the structure above.

---

## End-to-End Workflow

```
                    RAW PLATFORM OUTPUT
                           |
          .────────────────┴────────────────.
          │                                 │
     VISIUM OUTPUT                   XENIUM OUTPUT
  (Space Ranger .h5 +              (Transcript tables +
   tissue image)                    cell boundaries +
          │                          OME-TIFF image)
          ▼                                 ▼
    ┌───────────┐                    ┌─────────────┐
    │  AnnData  │                    │ SpatialData │
    │  (.h5ad)  │                    │   (sdata)   │
    └─────┬─────┘                    └──────┬──────┘
          │                                 │
          ▼                                 ▼
   QUALITY CONTROL ──────────────── QUALITY CONTROL
   NORMALIZATION ─────────────────── NORMALIZATION
   FEATURE SELECTION ───────────── FEATURE SELECTION
   PCA → UMAP → LEIDEN ────── PCA → UMAP → LEIDEN
          │                                 │
          └──────────────┬──────────────────┘
                         ▼
              SPATIAL GRAPH CONSTRUCTION
                         │
           .─────────────┼──────────────────.
           ▼             ▼                  ▼
      MORAN'S I     NEIGHBORHOOD       CO-OCCURRENCE
    Autocorrelation  ENRICHMENT         ANALYSIS
    (Modules 2A, 4)  (Modules 2B, 4)   (Module 2B)
                         │
                         ▼
              LIGAND-RECEPTOR INFERENCE
              (Module 2B — H&E only)
                         │
                         ▼
              IMAGE FEATURE EXTRACTION
              (Modules 2A, 2B)
                         │
                         ▼
                  BIOLOGICAL INTERPRETATION
```

---

## Who This Is For

- Graduate students and researchers entering spatial transcriptomics for the first time
- Bioinformaticians familiar with scRNA-seq who want to extend into spatial analysis
- Computational biologists comfortable with Python but new to Scanpy, Squidpy, or SpatialData
- Anyone working with 10x Genomics Visium or Xenium datasets seeking an annotated, step-by-step reference

A working knowledge of Python is assumed. Familiarity with single-cell concepts such as count matrices, normalization, and dimensionality reduction is helpful but not required.

---

## Prerequisites

| Area | What You Need |
|---|---|
| Programming | Python — loops, functions, lists, dictionaries |
| Data handling | NumPy and Pandas at a basic level |
| Biology | A general understanding of gene expression and RNA sequencing |
| Tools | Ability to open and run a Jupyter or Google Colab notebook |

---

## Environment Setup

All notebooks are designed to run on **Google Colab** with no local installation required.

### Option A — pip

```bash
python -m venv spatial-env
source spatial-env/bin/activate        # Windows: spatial-env\Scripts\activate

pip install scanpy squidpy anndata matplotlib seaborn
pip install spatialdata spatialdata-io  # Module 4 (Xenium) only
```

### Option B — Conda

```bash
conda env create -f environment.yml
conda activate spatial-env
```

### Google Colab (Recommended)

Run the first cell in each notebook before anything else:

```python
# Modules 1, 2A, 2B
!pip install scanpy squidpy anndata -q

# Module 4 — Xenium
!pip install spatialdata spatialdata-io -q
```

---

## Module 1 — Spatial Analysis with Scanpy

[![Difficulty](https://img.shields.io/badge/Difficulty-Beginner-2e7d32?style=flat-square)](.)
[![Ref](https://img.shields.io/badge/Reference-Scanpy_Tutorials-1a7abf?style=flat-square)](https://scanpy-tutorials.readthedocs.io/en/latest/spatial/basic-analysis.html)

**Reference:** [Basic Spatial Analysis with Scanpy](https://scanpy-tutorials.readthedocs.io/en/latest/spatial/basic-analysis.html)

This is the entry point for the entire series. It introduces the `AnnData` data structure, establishes the standard analysis pipeline that all subsequent modules build upon, and demonstrates how gene expression data and tissue images coexist within a single unified object.

### What This Module Teaches

- How spatial transcriptomics data is structured in Python using `AnnData`
- The complete standard pipeline: QC → normalization → feature selection → PCA → UMAP → clustering
- How to overlay gene expression and cluster labels on a tissue image
- How to identify marker genes and spatially variable genes per cluster

### Input

| Item | Format | Description |
|---|---|---|
| Space Ranger output directory | Directory | Root folder of the Space Ranger pipeline output |
| Filtered feature-barcode matrix | `.h5` or `.mtx.gz` | Gene expression counts per spatial spot |
| Tissue positions | `tissue_positions_list.csv` | x/y pixel coordinates of each barcode on tissue |
| Tissue image | `.png` (hires + lowres) | Brightfield or fluorescence scan of the section |
| Scale factors | `scalefactors_json.json` | Pixel-to-micron conversion for each image resolution |

### Output

| Item | Format | Description |
|---|---|---|
| Processed AnnData object | `.h5ad` | QC-filtered, normalized, clustered dataset |
| UMAP plot | `.png` | Low-dimensional embedding colored by cluster |
| Spatial cluster map | `.png` | Cluster labels overlaid on tissue image |
| Highly variable gene flags | `adata.var['highly_variable']` | Boolean mask for top variable genes |
| Marker gene table | `adata.uns['rank_genes_groups']` | Top differentially expressed genes per cluster |

### Step-by-Step

**Step 1 — Load data**

```python
import scanpy as sc

adata = sc.read_visium("path/to/spaceranger/output/")
adata.var_names_make_unique()
sc.pl.spatial(adata, img_key="hires", color="total_counts")
```

**Step 2 — Quality control**

```python
sc.pp.calculate_qc_metrics(adata, inplace=True)
sc.pp.filter_cells(adata, min_counts=1000)
sc.pp.filter_genes(adata, min_cells=10)
```

Spots with very few counts are likely outside the tissue or low-quality capture areas. Genes expressed in fewer than 10 spots carry insufficient statistical power for downstream analysis.

**Step 3 — Normalization and log-transformation**

```python
sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)
```

**Step 4 — Highly variable gene selection**

```python
sc.pp.highly_variable_genes(adata, flavor="seurat", n_top_genes=2000)
adata = adata[:, adata.var.highly_variable]
```

**Step 5 — Dimensionality reduction**

```python
sc.pp.pca(adata)
sc.pp.neighbors(adata)
sc.tl.umap(adata)
```

**Step 6 — Clustering**

```python
sc.tl.leiden(adata, key_added="clusters", random_state=42)
sc.pl.umap(adata, color="clusters")
sc.pl.spatial(adata, color="clusters", size=1.5)
```

**Step 7 — Marker gene identification**

```python
sc.tl.rank_genes_groups(adata, "clusters", method="t-test")
sc.pl.rank_genes_groups_heatmap(adata, n_genes=5, groupby="clusters")
```

---

## Module 2 and 3 — Visium Analysis

[![Difficulty](https://img.shields.io/badge/Difficulty-Intermediate-e65100?style=flat-square)](.)
[![Ref](https://img.shields.io/badge/Reference-Squidpy_Tutorials-5c4d8a?style=flat-square)](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/)

Both Visium modules use **Squidpy**, which extends Scanpy with spatial graph construction, spatial statistics, and image analysis. They share the same platform — 10x Genomics Visium — but differ in staining method and the analytical steps each enables. Their upstream pipelines are identical to Module 1. The modules diverge at image processing and spatial statistics, which is why they are presented together.

---

### 2A — Visium Fluorescence Data

**Reference:** [Analyze Visium Fluorescence Data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_fluo.html)

Fluorescence imaging uses dyes or antibodies that emit light at specific wavelengths. The resulting image contains multiple distinct channels — each representing a different biological marker (e.g., DAPI for nuclei, a labeled antibody for a protein of interest). This module demonstrates how to work with multichannel fluorescence images alongside gene expression data and how to extract quantitative image features at the spot level.

#### What This Module Teaches

- Loading multichannel fluorescence images using Squidpy's `ImageContainer`
- Extracting spot-level image features: texture, histogram statistics, and segmentation-based features
- Building a spatial neighborhood graph based on physical tissue coordinates
- Measuring spatial autocorrelation with Moran's I to identify spatially structured genes
- Combining image features with gene expression for joint analysis

#### Input

| Item | Format | Description |
|---|---|---|
| Visium gene expression data | `.h5ad` or Space Ranger output | Spot-level count matrix |
| Fluorescence tissue image | `.tiff` (multichannel) | One imaging channel per biological marker |
| Spatial coordinates | `tissue_positions_list.csv` | Spot positions in pixel units |
| Scale factors | `scalefactors_json.json` | Pixel-to-micron conversion ratios |

#### Output

| Item | Format | Description |
|---|---|---|
| AnnData with image features | `.h5ad` | Expression augmented with per-spot image features |
| Moran's I scores | `adata.uns['moranI']` | Spatial autocorrelation values ranked per gene |
| Spatially variable gene list | `.csv` | Top genes ranked by Moran's I score |
| Image feature matrix | `adata.obsm['img_features']` | Texture, histogram, summary values per spot |
| Spatial scatter plots | `.png` | Gene expression and cluster maps on tissue |

#### Step-by-Step

**Step 1 — Load data and image**

```python
import squidpy as sq

adata = sq.datasets.visium_fluo_adata()
img   = sq.datasets.visium_fluo_image_crop()
```

**Step 2 — Extract image features**

```python
sq.im.calculate_image_features(
    adata,
    img,
    features=["summary", "texture", "histogram"],
    key_added="img_features"
)
```

Summary features capture mean and standard deviation of pixel intensities per channel. Texture features (GLCM-based) capture spatial relationships between pixel intensities. Histogram features capture the full distribution of pixel values across intensity bins.

**Step 3 — Build spatial neighborhood graph**

```python
sq.gr.spatial_neighbors(adata, coord_type="grid", n_neighs=6)
```

`coord_type="grid"` is appropriate for Visium because spots are arranged in a regular hexagonal grid. This graph is the foundation for all spatial statistics in Squidpy.

**Step 4 — Spatial autocorrelation — Moran's I**

```python
sq.gr.spatial_autocorr(adata, mode="moran")
adata.uns["moranI"].sort_values("I", ascending=False).head(10)
```

Moran's I ranges from -1 to +1. A score near +1 means the gene is expressed in spatially clustered patterns. A score near 0 indicates random spatial distribution. Negative scores indicate spatial dispersion.

**Step 5 — Visualize**

```python
sq.pl.spatial_scatter(adata, color=["leiden", "GENE_OF_INTEREST"])
```

---

### 2B — Visium H&E Data

**Reference:** [Analyze Visium H&E Data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_hne.html)

Hematoxylin and eosin staining is the gold standard in histopathology. Hematoxylin stains nuclei dark blue-purple; eosin stains cytoplasm and extracellular matrix pink. H&E images are single brightfield images — not multichannel — and analysis focuses on tissue morphology and cellular organization. This module introduces neighborhood enrichment, co-occurrence analysis, and ligand-receptor interaction inference — the most biologically interpretive analyses in the series.

#### What This Module Teaches

- H&E image handling and tissue segmentation using Squidpy
- Neighborhood enrichment scoring — which clusters are spatially adjacent more than expected by chance
- Co-occurrence analysis — how cluster proximity changes as a function of spatial distance
- Ligand-receptor interaction inference between neighboring cell-type clusters
- Interpreting enrichment matrices and interaction dot plots

#### Input

| Item | Format | Description |
|---|---|---|
| Visium gene expression data | `.h5ad` or Space Ranger output | Spot-level count matrix |
| H&E tissue image | `.png` or `.tiff` (single channel) | Brightfield histology image |
| Cluster annotations | `adata.obs['leiden']` | Leiden cluster labels from standard pipeline |
| Ligand-receptor database | Internal to Squidpy | Curated gene interaction pairs (CellChat / NicheNet) |

#### Output

| Item | Format | Description |
|---|---|---|
| Segmentation mask | `img['segmentation']` | Per-pixel cell segmentation from H&E image |
| Segmentation features | `adata.obsm['seg_features']` | Per-spot cell count and morphology metrics |
| Neighborhood enrichment matrix | `adata.uns['leiden_nhood_enrichment']` | Z-scores of pairwise cluster co-localization |
| Co-occurrence scores | `adata.uns['leiden_co_occurrence']` | Proximity probability curves across distances |
| Ligand-receptor results | `adata.uns['leiden_ligrec']` | p-values and mean expression per interaction pair |
| Enrichment heatmap | `.png` | Cluster-by-cluster spatial enrichment matrix |
| Interaction dot plot | `.png` | LR pairs colored by significance and strength |

#### Step-by-Step

**Step 1 — Load data and image**

```python
adata = sq.datasets.visium_hne_adata()
img   = sq.datasets.visium_hne_image_crop()
```

**Step 2 — Tissue segmentation**

```python
sq.im.segment(img, method="watershed", layer="image", channel=0)

sq.im.calculate_image_features(
    adata, img,
    features=["segmentation"],
    layer="image",
    key_added="seg_features"
)
```

**Step 3 — Spatial neighborhood graph**

```python
sq.gr.spatial_neighbors(adata, coord_type="grid")
```

**Step 4 — Neighborhood enrichment**

```python
sq.gr.nhood_enrichment(adata, cluster_key="leiden")
sq.pl.nhood_enrichment(adata, cluster_key="leiden", method="average", cmap="inferno")
```

A positive z-score between two clusters means they are found as neighbors more often than random chance predicts. A negative z-score indicates spatial avoidance.

**Step 5 — Co-occurrence analysis**

```python
sq.gr.co_occurrence(adata, cluster_key="leiden")
sq.pl.co_occurrence(adata, cluster_key="leiden", clusters="Cluster_1")
```

**Step 6 — Ligand-receptor interaction inference**

```python
sq.gr.ligrec(
    adata,
    n_perms=1000,
    cluster_key="leiden",
    copy=False,
    use_raw=False
)
sq.pl.ligrec(adata, cluster_key="leiden", source_groups="Cluster_1")
```

---

### Visium Platform Comparison

| Feature | Module 2A — Fluorescence | Module 2B — H&E |
|---|---|---|
| Staining type | Multichannel fluorescence | Brightfield hematoxylin and eosin |
| Image channels | Multiple (DAPI, marker channels) | Single |
| Image analysis focus | Intensity per channel, texture | Morphology, cell segmentation |
| Key image features | Summary, texture, histogram | Segmentation-based cell counts |
| Primary spatial statistic | Moran's I autocorrelation | Neighborhood enrichment, co-occurrence |
| Cell communication analysis | Not covered | Ligand-receptor inference (ligrec) |
| Primary biological output | Spatially structured gene programs | Cell-cell communication pathways |

Both modules share the same upstream pipeline (QC, normalization, clustering) and the same spatial graph construction step. Differences arise entirely from the imaging modality and the biological questions each enables.

---

## Module 4 — Xenium In Situ Analysis

[![Difficulty](https://img.shields.io/badge/Difficulty-Advanced-b71c1c?style=flat-square)](.)
[![Ref](https://img.shields.io/badge/Reference-Squidpy_Tutorials-5c4d8a?style=flat-square)](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_xenium.html)

**Reference:** [Analyze Xenium Data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_xenium.html)

10x Genomics Xenium is an in situ spatial transcriptomics platform that provides **subcellular resolution**. Unlike Visium, which captures gene expression at the level of spatial spots containing several cells, Xenium directly images RNA molecules inside tissue using fluorescence-labeled probes and assigns each transcript to a specific cell based on its physical coordinates. This is true single-cell spatial transcriptomics.

### Visium vs Xenium — Platform Comparison

| Feature | Visium (Modules 2 and 3) | Xenium (Module 4) |
|---|---|---|
| Resolution | Spot-level (~55 µm, ~10 cells/spot) | Single-cell and subcellular |
| Transcript assignment | Passively captured by spot position | Localized by fluorescence probe imaging |
| Gene panel | Whole transcriptome | Targeted panel (100–5000 genes) |
| Cells per capture unit | Multiple cells per spot | One cell per barcode |
| Output format | Feature-barcode matrix + tissue image | Transcript coords + cell boundaries + image |
| Primary data structure | `AnnData` | `SpatialData` |
| Image type | H&E or fluorescence scan | OME-TIFF z-stack (DAPI morphology) |
| Spatial resolution | ~55 µm | < 1 µm (subcellular) |

### What This Module Teaches

- Loading and exploring a `SpatialData` object produced from Xenium output
- Understanding the relationship between transcripts, cells, boundaries, and images in one coordinate system
- Applying a standard scRNA-seq pipeline to true single-cell Xenium data
- Visualizing individual transcript positions at subcellular resolution
- Performing spatial statistics at single-cell rather than spot-level resolution

### Input

| Item | Format | Description |
|---|---|---|
| Cell feature matrix | Directory (MEX format) | Gene-by-cell count matrix |
| Transcript coordinates | `transcripts.csv.gz` | x, y, z position of every detected RNA molecule |
| Cell metadata | `cells.csv.gz` | Per-cell summary: transcript count, nucleus area, centroid |
| Nucleus boundaries | `nucleus_boundaries.csv.gz` | Polygon vertex coordinates for each nucleus |
| Cell boundaries | `cell_boundaries.csv.gz` | Polygon vertex coordinates for each cell |
| Morphology image | `morphology_mip.ome.tif` | Maximum intensity projection of DAPI z-stack |

### Output

| Item | Format | Description |
|---|---|---|
| SpatialData object | `sdata` (in-memory) | All data layers in one unified coordinate system |
| Processed AnnData (cell-level) | `.h5ad` | QC-filtered, normalized, clustered single cells |
| UMAP embedding | `.png` | Cell-level UMAP colored by Leiden cluster |
| Spatial cell map | `.png` | Cluster labels overlaid on DAPI morphology image |
| Transcript scatter plot | `.png` | Individual RNA molecule positions across tissue |
| Marker gene table | `.csv` | Top differentially expressed genes per cluster |

### Step-by-Step

**Step 1 — Load Xenium output into SpatialData**

```python
import spatialdata_io as sdio

sdata = sdio.xenium("path/to/xenium/output/")
print(sdata)
```

| Element | Key | Content |
|---|---|---|
| Cell circles | `sdata["cell_circles"]` | Simplified circular cell representations with expression |
| Cell boundaries | `sdata["cell_boundaries"]` | Precise polygon outlines for each cell |
| Nucleus boundaries | `sdata["nucleus_boundaries"]` | Polygon outlines for each nucleus |
| Transcripts | `sdata["transcripts"]` | All detected RNA molecules with x, y, z coordinates |
| Morphology image | `sdata["morphology_mip"]` | DAPI maximum intensity projection |

**Step 2 — Extract cell-level AnnData**

```python
import scanpy as sc

adata = sdata["cell_circles"].values
```

This gives a standard `AnnData` object — one row per cell, one column per gene. All Scanpy functions apply from this point.

**Step 3 — Quality control**

```python
sc.pp.calculate_qc_metrics(adata, percent_top=None, log1p=False, inplace=True)
sc.pp.filter_cells(adata, min_counts=10)
sc.pp.filter_genes(adata, min_cells=5)
```

**Step 4 — Normalization**

```python
sc.pp.normalize_total(adata)
sc.pp.log1p(adata)
```

**Step 5 — Dimensionality reduction and clustering**

```python
sc.pp.pca(adata)
sc.pp.neighbors(adata)
sc.tl.umap(adata)
sc.tl.leiden(adata, random_state=42)
```

**Step 6 — Spatial visualization at single-cell resolution**

```python
import squidpy as sq

sq.pl.spatial_scatter(adata, color="leiden", shape="hex", size=20)
```

**Step 7 — Transcript-level visualization**

```python
sq.pl.spatial_scatter(adata, color="GENE_OF_INTEREST", shape=None, size=3)
```

This capability is unique to in situ platforms — individual RNA molecule positions plotted directly on tissue, revealing subcellular localization of any gene of interest.

---

## Data and File Formats

### AnnData Object Structure

| Slot | Content | Type |
|---|---|---|
| `adata.X` | Gene expression count matrix (spots or cells × genes) | Sparse matrix |
| `adata.obs` | Per-observation metadata: QC metrics, cluster labels | DataFrame |
| `adata.var` | Per-gene metadata: gene names, highly variable flags | DataFrame |
| `adata.obsm['spatial']` | 2D tissue coordinates for each spot or cell | NumPy array |
| `adata.uns['spatial']` | Tissue images and scale factors (Visium) | Dictionary |
| `adata.obsp['spatial_connectivities']` | Spatial neighborhood graph adjacency matrix | Sparse matrix |
| `adata.uns['moranI']` | Moran's I autocorrelation results per gene | DataFrame |
| `adata.uns['leiden_nhood_enrichment']` | Neighborhood enrichment z-scores (cluster × cluster) | Dictionary |
| `adata.uns['leiden_co_occurrence']` | Co-occurrence scores across distance bins | Dictionary |
| `adata.uns['leiden_ligrec']` | Ligand-receptor interaction p-values and means | Dictionary |

### Space Ranger Output (Visium)

```
spaceranger_output/
├── filtered_feature_bc_matrix/
│   ├── barcodes.tsv.gz          # One barcode per spot passing the tissue filter
│   ├── features.tsv.gz          # Ensembl gene IDs and HGNC gene names
│   └── matrix.mtx.gz            # Sparse count matrix in Matrix Market format
└── spatial/
    ├── tissue_positions_list.csv # Barcode-to-pixel coordinate mapping
    ├── scalefactors_json.json    # Image downscale factors per resolution
    ├── tissue_hires_image.png    # High-resolution tissue scan
    └── tissue_lowres_image.png   # Low-resolution tissue scan
```

### Xenium Output

```
xenium_output/
├── cell_feature_matrix/          # Gene-by-cell count matrix (MEX format)
│   ├── barcodes.tsv.gz
│   ├── features.tsv.gz
│   └── matrix.mtx.gz
├── transcripts.csv.gz            # Every transcript: x, y, z, gene name, cell_id
├── cells.csv.gz                  # Per-cell: centroid, transcript count, area
├── nucleus_boundaries.csv.gz     # Polygon vertices for each nucleus
├── cell_boundaries.csv.gz        # Polygon vertices for each cell
└── morphology_mip.ome.tif        # Maximum intensity projection DAPI (OME-TIFF)
```

---

## Software Stack

| Package | Version | Role in This Series |
|---|---|---|
| `Python` | >= 3.9 | Programming language |
| `scanpy` | >= 1.9 | QC, normalization, PCA, UMAP, clustering, marker genes — all modules |
| `squidpy` | >= 1.3 | Spatial graph, Moran's I, enrichment, co-occurrence, ligrec, image features — Modules 2–4 |
| `anndata` | >= 0.9 | Core data structure shared across all modules |
| `spatialdata` | >= 0.1 | Unified multi-element data structure for Xenium — Module 4 |
| `spatialdata-io` | >= 0.1 | Data loaders for Xenium and other in situ platforms — Module 4 |
| `matplotlib` | >= 3.6 | Base plotting library |
| `seaborn` | >= 0.12 | Statistical plots and heatmaps |
| `numpy` | >= 1.23 | Numerical operations |
| `pandas` | >= 1.5 | Tabular data manipulation |
| `scikit-learn` | >= 1.1 | PCA internals and machine learning utilities |
| `leidenalg` | >= 0.9 | Leiden clustering algorithm |

---

## Running in Google Colab

**Step 1 — Open the notebook**

Click the Colab link next to the module you want to run. The notebook opens in your browser with all code pre-loaded.

**Step 2 — Connect to a runtime**

Click **Connect** in the top-right corner. Select a CPU runtime. No GPU is required for any module in this series.

**Step 3 — Run the setup cell**

The first code cell installs all required packages. Run it before any other cell. This typically takes 2 to 4 minutes. Skipping this step will cause import errors throughout the notebook.

**Step 4 — Run cells sequentially**

Every cell depends on the variables created by the cells before it. Running cells out of order will produce `NameError`, `KeyError`, or `AttributeError` exceptions. Work strictly top to bottom.

**Step 5 — Save your progress**

Go to **File > Save a copy in Drive** to save the notebook including all outputs to your Google Drive. Colab does not automatically save outputs between sessions.

### Runtime Reference

| Consideration | Detail |
|---|---|
| Session timeout | Disconnects after ~90 minutes of inactivity |
| On reconnect | All in-memory variables are lost — re-run from the top |
| Dataset source | All tutorial datasets are fetched automatically — no upload needed |
| RAM limit | ~12 GB on standard Colab CPU runtime; sufficient for all four modules |
| Disk limit | ~107 GB on standard Colab runtime; sufficient for all sample datasets |

---

## Troubleshooting

**Import errors after the setup cell**

Restart the Colab runtime (Runtime > Restart runtime) and re-run all cells from the top. Version conflicts during installation are typically resolved on a clean restart.

**`ValueError: obsm['spatial'] not found`**

Ensure you used `sc.read_visium()` for Visium data and that the `spatial/` directory is present in the Space Ranger output folder passed to the reader.

**Memory error during image feature extraction**

Reduce the processing scale for large tissue sections:

```python
sq.im.calculate_image_features(adata, img, scale=0.5, key_added="img_features")
```

**Leiden clustering gives different results on each run**

Set an explicit seed to reproduce results:

```python
sc.tl.leiden(adata, random_state=42)
```

**Dataset download fails mid-notebook**

Check your internet connection and retry the cell. Large file downloads can occasionally time out on Colab's network connection.

**`KeyError` when accessing `adata.uns['moranI']`**

Confirm that `sq.gr.spatial_neighbors()` was run before `sq.gr.spatial_autocorr()`. The spatial graph is a prerequisite for autocorrelation computation.

**Xenium loading fails with `FileNotFoundError`**

Confirm the path passed to `sdio.xenium()` points to the root Xenium output directory — the folder directly containing `cells.csv.gz`, `transcripts.csv.gz`, and `cell_feature_matrix/`. Do not point to a subdirectory.

---

## References

### Tutorial Documentation

- Scanpy Spatial Tutorials — https://scanpy-tutorials.readthedocs.io/en/latest/spatial/
- Squidpy Documentation — https://squidpy.readthedocs.io/en/stable/
- SpatialData Documentation — https://spatialdata.scverse.org/en/latest/

### Key Publications

- Palla et al. (2022). Squidpy: a scalable framework for spatial omics analysis. *Nature Methods*, 19, 171–178.
- Wolf et al. (2018). SCANPY: large-scale single-cell gene expression data analysis. *Genome Biology*, 19, 15.
- Ståhl et al. (2016). Visualization and analysis of gene expression in tissue sections by spatial transcriptomics. *Science*, 353(6294), 78–82.
- Virshup et al. (2021). anndata: Annotated data. *bioRxiv*.

### 10x Genomics Platform Documentation

- Visium Spatial Gene Expression — https://www.10xgenomics.com/products/spatial-gene-expression
- Xenium In Situ — https://www.10xgenomics.com/products/xenium-in-situ
- Space Ranger Pipeline — https://support.10xgenomics.com/spatial-gene-expression/software/pipelines/latest/what-is-space-ranger

### Community Resources

- scverse ecosystem — https://scverse.org
- Bioinformatics Stack Exchange — https://bioinformatics.stackexchange.com

---

<div align="center">

[![Scanpy](https://img.shields.io/badge/-Scanpy-1a7abf?style=flat-square)](https://scanpy.readthedocs.io/)
[![Squidpy](https://img.shields.io/badge/-Squidpy-5c4d8a?style=flat-square)](https://squidpy.readthedocs.io/)
[![SpatialData](https://img.shields.io/badge/-SpatialData-b71c1c?style=flat-square)](https://spatialdata.scverse.org/)
[![AnnData](https://img.shields.io/badge/-AnnData-2e7d32?style=flat-square)](https://anndata.readthedocs.io/)

*This tutorial series is intended for educational use.*
*All datasets are publicly available and fetched automatically within each notebook.*

</div>
