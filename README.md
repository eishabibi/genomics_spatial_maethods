<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Genomics Spatial Methods</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
    font-size: 15px;
    line-height: 1.75;
    color: #24292e;
    background: #ffffff;
    max-width: 980px;
    margin: 0 auto;
    padding: 40px 32px 80px;
  }

  /* ── HEADER ── */
  .page-header {
    text-align: center;
    padding: 48px 0 36px;
    border-bottom: 2px solid #e1e4e8;
    margin-bottom: 40px;
  }
  .page-header h1 {
    font-size: 2.4rem;
    font-weight: 700;
    color: #0d1117;
    letter-spacing: -0.5px;
    margin-bottom: 14px;
  }
  .page-header p {
    font-size: 1.05rem;
    color: #586069;
    max-width: 680px;
    margin: 0 auto;
  }

  /* ── TABLE OF CONTENTS ── */
  .toc {
    background: #f6f8fa;
    border: 1px solid #e1e4e8;
    border-radius: 8px;
    padding: 24px 28px;
    margin-bottom: 40px;
  }
  .toc h2 {
    font-size: 1rem;
    font-weight: 600;
    color: #24292e;
    margin-bottom: 12px;
    text-transform: uppercase;
    letter-spacing: 0.6px;
    border: none;
    padding: 0;
    background: none;
  }
  .toc ol { padding-left: 20px; }
  .toc li { margin: 5px 0; }
  .toc a { color: #0366d6; text-decoration: none; }
  .toc a:hover { text-decoration: underline; }
  .toc .sub { padding-left: 18px; list-style: disc; }

  /* ── SECTION HEADINGS ── */
  h2 {
    font-size: 1.35rem;
    font-weight: 700;
    padding: 10px 16px;
    border-radius: 6px;
    margin: 44px 0 18px;
  }
  h3 {
    font-size: 1.1rem;
    font-weight: 600;
    margin: 28px 0 12px;
    color: #24292e;
  }
  h4 {
    font-size: 0.95rem;
    font-weight: 600;
    margin: 22px 0 10px;
    color: #24292e;
    text-transform: uppercase;
    letter-spacing: 0.4px;
  }

  /* Module color themes */
  .h-general  { color: #0550ae; background: #dbeafe; border-left: 5px solid #1a7abf; }
  .h-module1  { color: #065f46; background: #d1fae5; border-left: 5px solid #10b981; }
  .h-visium   { color: #4c1d95; background: #ede9fe; border-left: 5px solid #7c3aed; }
  .h-xenium   { color: #7f1d1d; background: #fee2e2; border-left: 5px solid #dc2626; }

  .module-badge {
    display: inline-block;
    font-size: 0.75rem;
    font-weight: 600;
    padding: 2px 10px;
    border-radius: 20px;
    margin-right: 8px;
    vertical-align: middle;
  }
  .badge-beginner    { background: #d1fae5; color: #065f46; }
  .badge-intermediate{ background: #fef3c7; color: #92400e; }
  .badge-advanced    { background: #fee2e2; color: #7f1d1d; }

  /* ── PROSE ── */
  p { margin: 12px 0; color: #24292e; }
  ul, ol { padding-left: 22px; margin: 10px 0; }
  li { margin: 5px 0; }
  a { color: #0366d6; text-decoration: none; }
  a:hover { text-decoration: underline; }
  strong { font-weight: 600; }
  em { font-style: italic; }

  hr {
    border: none;
    border-top: 1px solid #e1e4e8;
    margin: 36px 0;
  }

  blockquote {
    border-left: 4px solid #1a7abf;
    background: #f1f8ff;
    padding: 12px 16px;
    border-radius: 0 6px 6px 0;
    margin: 16px 0;
    color: #24292e;
  }

  /* ── CODE ── */
  code {
    font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, monospace;
    font-size: 0.85em;
    background: #f3f4f6;
    border: 1px solid #e1e4e8;
    border-radius: 4px;
    padding: 1px 5px;
    color: #c7254e;
  }

  pre {
    background: #0d1117;
    border-radius: 8px;
    padding: 20px 22px;
    overflow-x: auto;
    margin: 16px 0;
    border: 1px solid #30363d;
  }
  pre code {
    font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, monospace;
    font-size: 0.875rem;
    background: none;
    border: none;
    padding: 0;
    color: #c9d1d9;
    line-height: 1.6;
  }

  /* ── TABLES ── */
  table {
    width: 100%;
    border-collapse: collapse;
    margin: 18px 0;
    font-size: 0.9rem;
  }
  th {
    background: #f6f8fa;
    border: 1px solid #d0d7de;
    padding: 9px 14px;
    text-align: left;
    font-weight: 600;
    color: #24292e;
  }
  td {
    border: 1px solid #d0d7de;
    padding: 8px 14px;
    vertical-align: top;
  }
  tr:nth-child(even) td { background: #f6f8fa; }

  /* Module-colored table headers */
  .table-module1 th { background: #d1fae5; color: #065f46; border-color: #6ee7b7; }
  .table-visium  th { background: #ede9fe; color: #4c1d95; border-color: #c4b5fd; }
  .table-xenium  th { background: #fee2e2; color: #7f1d1d; border-color: #fca5a5; }
  .table-general th { background: #dbeafe; color: #1e3a8a; border-color: #93c5fd; }

  /* ── STEP BOXES ── */
  .step {
    border-left: 3px solid #e1e4e8;
    padding: 4px 0 4px 16px;
    margin: 20px 0;
  }
  .step-title {
    font-weight: 700;
    font-size: 0.92rem;
    color: #24292e;
    margin-bottom: 8px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .step-num {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    font-size: 0.75rem;
    font-weight: 700;
    flex-shrink: 0;
  }
  .num-m1 { background: #10b981; color: white; }
  .num-vis { background: #7c3aed; color: white; }
  .num-xen { background: #dc2626; color: white; }
  .num-gen { background: #1a7abf; color: white; }

  /* ── IO CARDS ── */
  .io-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin: 16px 0;
  }
  .io-card {
    border-radius: 8px;
    overflow: hidden;
  }
  .io-card-header {
    padding: 8px 14px;
    font-size: 0.8rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.6px;
  }
  .io-input  .io-card-header { background: #1a7abf; color: white; }
  .io-output .io-card-header { background: #10b981; color: white; }
  .io-card table { margin: 0; font-size: 0.84rem; }
  .io-card th { background: #f6f8fa; }
  .io-card td, .io-card th { border-color: #e1e4e8; }

  /* ── COMPARISON BANNER ── */
  .compare-banner {
    background: linear-gradient(135deg, #ede9fe 0%, #dbeafe 100%);
    border: 1px solid #c4b5fd;
    border-radius: 8px;
    padding: 16px 20px;
    margin: 20px 0 10px;
    font-size: 0.9rem;
    color: #4c1d95;
  }

  /* ── TREE ── */
  .tree pre {
    background: #f6f8fa;
    border: 1px solid #e1e4e8;
    color: #24292e;
  }
  .tree pre code { color: #24292e; }

  /* ── WORKFLOW ── */
  .workflow pre {
    background: #0d1117;
    color: #c9d1d9;
  }

  /* ── FOOTER ── */
  .page-footer {
    text-align: center;
    margin-top: 60px;
    padding-top: 24px;
    border-top: 1px solid #e1e4e8;
    color: #8b949e;
    font-size: 0.85rem;
  }
  .footer-pills { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; margin-top: 12px; }
  .pill {
    padding: 4px 14px;
    border-radius: 20px;
    font-size: 0.78rem;
    font-weight: 600;
  }
  .pill-blue   { background: #dbeafe; color: #1e40af; }
  .pill-purple { background: #ede9fe; color: #5b21b6; }
  .pill-red    { background: #fee2e2; color: #991b1b; }
  .pill-green  { background: #d1fae5; color: #065f46; }
</style>
</head>
<body>

<!-- ═══════════════════════════════════════════════════════ HEADER -->
<div class="page-header">
  <h1>Genomics Spatial Methods</h1>
  <p>A structured, end-to-end tutorial series on spatial transcriptomics analysis. Covering spot-based and single-cell resolution platforms — from raw data loading through spatial statistics, image analysis, and interaction inference — using Scanpy, Squidpy, and SpatialData in Google Colab.</p>
</div>

<!-- ═══════════════════════════════════════════════════════ TOC -->
<div class="toc">
  <h2>Table of Contents</h2>
  <ol>
    <li><a href="#overview">What Is This Repository</a></li>
    <li><a href="#structure">Repository Structure</a></li>
    <li><a href="#workflow">End-to-End Workflow</a></li>
    <li><a href="#audience">Who This Is For</a></li>
    <li><a href="#prerequisites">Prerequisites</a></li>
    <li><a href="#setup">Environment Setup</a></li>
    <li><a href="#module1">Module 1 — Spatial Analysis with Scanpy</a></li>
    <li>
      <a href="#visium">Module 2 &amp; 3 — Visium Analysis</a>
      <ul class="sub">
        <li><a href="#visium-fluo">2A — Visium Fluorescence</a></li>
        <li><a href="#visium-hne">2B — Visium H&amp;E</a></li>
        <li><a href="#visium-compare">Visium Platform Comparison</a></li>
      </ul>
    </li>
    <li><a href="#xenium">Module 4 — Xenium In Situ Analysis</a></li>
    <li><a href="#formats">Data and File Formats</a></li>
    <li><a href="#stack">Software Stack</a></li>
    <li><a href="#colab">Running in Google Colab</a></li>
    <li><a href="#troubleshooting">Troubleshooting</a></li>
    <li><a href="#references">References</a></li>
  </ol>
</div>

<!-- ═══════════════════════════════════════════════════════ OVERVIEW -->
<h2 class="h-general" id="overview">What Is This Repository</h2>

<p>Spatial transcriptomics captures gene expression at the location where it occurs within a tissue — preserving the physical context that bulk and single-cell RNA-seq discard. This repository provides a complete, annotated set of notebooks that teach you how to work with spatial data in Python, progressing from foundational concepts through platform-specific workflows.</p>

<p>The four modules are designed to be taken sequentially. Each builds on the data structures, vocabulary, and analytical logic introduced in the one before it. By the end of the series, you will have worked with both spot-based capture (Visium) and subcellular in situ (Xenium) data, understood the differences between imaging modalities, and run spatial statistics including autocorrelation, neighborhood enrichment, co-occurrence analysis, and ligand-receptor interaction inference.</p>

<hr>

<!-- ═══════════════════════════════════════════════════════ STRUCTURE -->
<h2 class="h-general" id="structure">Repository Structure</h2>

<div class="tree">
<pre><code>genomics-spatial-methods/
│
├── notebooks/
│   ├── 01_scanpy_spatial_basics.ipynb        # Module 1 — Scanpy fundamentals
│   ├── 02a_visium_fluorescence.ipynb         # Module 2A — Visium fluorescence
│   ├── 02b_visium_hne.ipynb                  # Module 2B — Visium H&E
│   └── 03_xenium_analysis.ipynb              # Module 3 — Xenium in situ
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
└── README.html</code></pre>
</div>

<blockquote><strong>Note on data:</strong> Built-in dataset loaders in Scanpy and Squidpy download sample data automatically when the notebooks are run. You do not need to populate the <code>data/</code> directory manually for tutorial purposes. For your own datasets, place them in the appropriate subdirectory following the structure above.</blockquote>

<hr>

<!-- ═══════════════════════════════════════════════════════ WORKFLOW -->
<h2 class="h-general" id="workflow">End-to-End Workflow</h2>

<p>The diagram below shows how data moves through the full tutorial series — from raw platform output to biological interpretation.</p>

<div class="workflow">
<pre><code>                    RAW PLATFORM OUTPUT
                           |
          .────────────────┴────────────────.
          │                                 │
     VISIUM OUTPUT                   XENIUM OUTPUT
  (Space Ranger .h5 +              (Transcript tables +
   tissue image)                    cell boundaries +
          │                          OME-TIFF image)
          │                                 │
          ▼                                 ▼
    ┌───────────┐                    ┌─────────────┐
    │  AnnData  │                    │ SpatialData │
    │  (.h5ad)  │                    │   (sdata)   │
    └─────┬─────┘                    └──────┬──────┘
          │                                 │
          ▼                                 ▼
   QUALITY CONTROL ──────────────── QUALITY CONTROL
   (filter spots,                   (filter cells,
    genes, counts)                   transcript counts)
          │                                 │
          ▼                                 ▼
   NORMALIZATION ─────────────────── NORMALIZATION
   (total counts, log1p)             (total counts, log1p)
          │                                 │
          ▼                                 ▼
  FEATURE SELECTION ───────────── FEATURE SELECTION
  (highly variable genes)          (targeted panel)
          │                                 │
          ▼                                 ▼
   PCA → UMAP → LEIDEN ────── PCA → UMAP → LEIDEN
          │                                 │
          └──────────────┬──────────────────┘
                         │
                         ▼
              SPATIAL GRAPH CONSTRUCTION
              (physical coordinate-based neighbors)
                         │
           .─────────────┼──────────────────.
           │             │                  │
           ▼             ▼                  ▼
      MORAN'S I     NEIGHBORHOOD       CO-OCCURRENCE
      Spatial        ENRICHMENT         ANALYSIS
    Autocorrelation  Cluster            Distance-based
    (Modules 2A, 4)  Co-localization    Proximity
                     (Modules 2B, 4)    (Module 2B)
                         │
                         ▼
              LIGAND-RECEPTOR INFERENCE
              (Module 2B — H&E only)
                         │
                         ▼
              IMAGE FEATURE EXTRACTION
              (texture, histogram, segmentation)
              (Modules 2A, 2B)
                         │
                         ▼
                  BIOLOGICAL INTERPRETATION
                  Marker genes · Cluster annotation
                  Spatial maps · Cell-cell communication</code></pre>
</div>

<hr>

<!-- ═══════════════════════════════════════════════════════ AUDIENCE -->
<h2 class="h-general" id="audience">Who This Is For</h2>

<ul>
  <li>Graduate students and researchers entering spatial transcriptomics for the first time</li>
  <li>Bioinformaticians familiar with scRNA-seq who want to extend into spatial analysis</li>
  <li>Computational biologists comfortable with Python but new to Scanpy, Squidpy, or SpatialData</li>
  <li>Anyone working with 10x Genomics Visium or Xenium datasets and looking for an annotated, step-by-step reference</li>
</ul>

<p>A working knowledge of Python is assumed throughout. Familiarity with single-cell concepts such as count matrices, normalization, and dimensionality reduction is helpful but not required — core concepts are explained as they appear.</p>

<hr>

<!-- ═══════════════════════════════════════════════════════ PREREQUISITES -->
<h2 class="h-general" id="prerequisites">Prerequisites</h2>

<table class="table-general">
  <thead><tr><th>Area</th><th>What You Need</th></tr></thead>
  <tbody>
    <tr><td>Programming</td><td>Python — loops, functions, lists, dictionaries</td></tr>
    <tr><td>Data handling</td><td>NumPy and Pandas at a basic level</td></tr>
    <tr><td>Biology</td><td>A general understanding of gene expression and RNA sequencing</td></tr>
    <tr><td>Tools</td><td>Ability to open and run a Jupyter or Google Colab notebook</td></tr>
  </tbody>
</table>

<hr>

<!-- ═══════════════════════════════════════════════════════ SETUP -->
<h2 class="h-general" id="setup">Environment Setup</h2>

<p>All notebooks are designed to run on <strong>Google Colab</strong> with no local installation. Each notebook contains a setup cell at the top that handles all dependency installation automatically.</p>

<h3>Option A — pip</h3>
<pre><code>python -m venv spatial-env
source spatial-env/bin/activate          # Windows: spatial-env\Scripts\activate

pip install scanpy squidpy anndata matplotlib seaborn
pip install spatialdata spatialdata-io   # Required for Module 4 (Xenium)</code></pre>

<h3>Option B — Conda</h3>
<pre><code>conda env create -f environment.yml
conda activate spatial-env</code></pre>

<h3>Google Colab (Recommended)</h3>
<p>Each notebook begins with an installation cell. Run it first before any other cell:</p>
<pre><code># Modules 1, 2A, 2B
!pip install scanpy squidpy anndata -q

# Module 4 — Xenium (additional packages required)
!pip install spatialdata spatialdata-io -q</code></pre>

<p>No GPU is required. A standard Colab CPU runtime handles all four modules without issue.</p>

<hr>

<!-- ═══════════════════════════════════════════════════════ MODULE 1 -->
<h2 class="h-module1" id="module1">Module 1 — Spatial Analysis with Scanpy</h2>

<p>
  <span class="module-badge badge-beginner">Beginner</span>
  <strong>Reference:</strong> <a href="https://scanpy-tutorials.readthedocs.io/en/latest/spatial/basic-analysis.html">Basic Spatial Analysis with Scanpy</a>
</p>

<p>This is the entry point for the entire series. It introduces the <code>AnnData</code> data structure, establishes the standard analysis pipeline that all subsequent modules build upon, and demonstrates how gene expression data and tissue images coexist within a single unified object. Every step here — QC, normalization, dimensionality reduction, clustering — is a prerequisite for understanding the spatial-specific analyses introduced from Module 2 onwards.</p>

<h3>What This Module Teaches</h3>
<ul>
  <li>How spatial transcriptomics data is structured in Python using <code>AnnData</code></li>
  <li>The complete standard pipeline: QC → normalization → feature selection → PCA → UMAP → clustering</li>
  <li>How to overlay gene expression and cluster labels on a tissue image</li>
  <li>How to identify marker genes and spatially variable genes per cluster</li>
</ul>

<div class="io-grid">
  <div class="io-card io-input">
    <div class="io-card-header">Input</div>
    <table>
      <thead><tr><th>Item</th><th>Format</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td>Space Ranger output</td><td>Directory</td><td>Root folder of the pipeline output</td></tr>
        <tr><td>Feature-barcode matrix</td><td><code>.h5</code> / <code>.mtx.gz</code></td><td>Gene expression counts per spot</td></tr>
        <tr><td>Tissue positions</td><td><code>.csv</code></td><td>x/y pixel coordinates per barcode</td></tr>
        <tr><td>Tissue image</td><td><code>.png</code></td><td>Hires and lowres tissue scans</td></tr>
        <tr><td>Scale factors</td><td><code>.json</code></td><td>Pixel-to-micron conversion ratios</td></tr>
      </tbody>
    </table>
  </div>
  <div class="io-card io-output">
    <div class="io-card-header">Output</div>
    <table>
      <thead><tr><th>Item</th><th>Format</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td>Processed AnnData</td><td><code>.h5ad</code></td><td>QC-filtered, normalized, clustered dataset</td></tr>
        <tr><td>UMAP plot</td><td><code>.png</code></td><td>Embedding colored by cluster</td></tr>
        <tr><td>Spatial cluster map</td><td><code>.png</code></td><td>Clusters overlaid on tissue image</td></tr>
        <tr><td>HVG flags</td><td><code>adata.var</code></td><td>Boolean mask of top variable genes</td></tr>
        <tr><td>Marker gene table</td><td><code>adata.uns</code></td><td>Top DE genes per cluster</td></tr>
      </tbody>
    </table>
  </div>
</div>

<h3>Step-by-Step</h3>

<div class="step">
  <div class="step-title"><span class="step-num num-m1">1</span> Load data</div>
<pre><code>import scanpy as sc

adata = sc.read_visium("path/to/spaceranger/output/")
adata.var_names_make_unique()
sc.pl.spatial(adata, img_key="hires", color="total_counts")</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-m1">2</span> Quality control</div>
<pre><code>sc.pp.calculate_qc_metrics(adata, inplace=True)
sc.pp.filter_cells(adata, min_counts=1000)
sc.pp.filter_genes(adata, min_cells=10)</code></pre>
  <p>Spots with very few counts are likely outside the tissue or represent low-quality capture areas. Genes expressed in fewer than 10 spots carry insufficient statistical power for downstream analysis.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-m1">3</span> Normalization and log-transformation</div>
<pre><code>sc.pp.normalize_total(adata, target_sum=1e4)
sc.pp.log1p(adata)</code></pre>
  <p>Library-size normalization scales every spot to the same total count. Log-transformation compresses dynamic range and makes data suitable for linear methods like PCA.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-m1">4</span> Highly variable gene selection</div>
<pre><code>sc.pp.highly_variable_genes(adata, flavor="seurat", n_top_genes=2000)
adata = adata[:, adata.var.highly_variable]</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-m1">5</span> Dimensionality reduction</div>
<pre><code>sc.pp.pca(adata)
sc.pp.neighbors(adata)
sc.tl.umap(adata)</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-m1">6</span> Clustering</div>
<pre><code>sc.tl.leiden(adata, key_added="clusters", random_state=42)
sc.pl.umap(adata, color="clusters")
sc.pl.spatial(adata, color="clusters", size=1.5)</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-m1">7</span> Marker gene identification</div>
<pre><code>sc.tl.rank_genes_groups(adata, "clusters", method="t-test")
sc.pl.rank_genes_groups_heatmap(adata, n_genes=5, groupby="clusters")</code></pre>
</div>

<hr>

<!-- ═══════════════════════════════════════════════════════ VISIUM -->
<h2 class="h-visium" id="visium">Module 2 &amp; 3 — Visium Analysis</h2>

<p>
  <span class="module-badge badge-intermediate">Intermediate</span>
  <strong>Reference:</strong> <a href="https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/">Squidpy Tutorials</a>
</p>

<p>Both Visium modules use <strong>Squidpy</strong>, which extends Scanpy with spatial graph construction, spatial statistics, and image analysis. They share the same platform — 10x Genomics Visium — but differ in staining method and the analytical steps each staining type enables. Their upstream pipelines are identical to Module 1. The modules diverge at image processing and spatial statistics. They are presented together here because they share the same platform, the same data loading approach, and the same spatial graph construction step.</p>

<!-- ── 2A ── -->
<h3 id="visium-fluo">2A — Visium Fluorescence Data</h3>
<p><strong>Reference:</strong> <a href="https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_fluo.html">Analyze Visium Fluorescence Data</a></p>

<p>Fluorescence imaging uses dyes or antibodies that emit light at specific wavelengths. The resulting image contains multiple distinct channels — each representing a different biological marker (e.g., DAPI for nuclei, a labeled antibody for a protein of interest). This module demonstrates how to work with multichannel fluorescence images alongside gene expression data and how to extract quantitative image features at the spot level.</p>

<h4>What This Module Teaches</h4>
<ul>
  <li>Loading multichannel fluorescence images using Squidpy's <code>ImageContainer</code></li>
  <li>Extracting spot-level image features: texture, histogram statistics, and segmentation-based features</li>
  <li>Building a spatial neighborhood graph based on physical tissue coordinates</li>
  <li>Measuring spatial autocorrelation with Moran's I to identify spatially structured genes</li>
  <li>Combining image features with gene expression for joint clustering and analysis</li>
</ul>

<div class="io-grid">
  <div class="io-card io-input">
    <div class="io-card-header">Input</div>
    <table>
      <thead><tr><th>Item</th><th>Format</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td>Visium expression data</td><td><code>.h5ad</code></td><td>Spot-level count matrix</td></tr>
        <tr><td>Fluorescence image</td><td><code>.tiff</code> (multichannel)</td><td>One channel per biological marker</td></tr>
        <tr><td>Spatial coordinates</td><td><code>.csv</code></td><td>Spot positions in pixel units</td></tr>
        <tr><td>Scale factors</td><td><code>.json</code></td><td>Pixel-to-micron conversion</td></tr>
      </tbody>
    </table>
  </div>
  <div class="io-card io-output">
    <div class="io-card-header">Output</div>
    <table>
      <thead><tr><th>Item</th><th>Format</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td>AnnData with image features</td><td><code>.h5ad</code></td><td>Expression + extracted image features</td></tr>
        <tr><td>Moran's I scores</td><td><code>adata.uns['moranI']</code></td><td>Autocorrelation values per gene</td></tr>
        <tr><td>Spatially variable genes</td><td><code>.csv</code></td><td>Genes ranked by Moran's I score</td></tr>
        <tr><td>Image feature matrix</td><td><code>adata.obsm</code></td><td>Texture, histogram values per spot</td></tr>
        <tr><td>Spatial scatter plots</td><td><code>.png</code></td><td>Gene and cluster maps on tissue</td></tr>
      </tbody>
    </table>
  </div>
</div>

<h4>Step-by-Step</h4>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">1</span> Load data and image</div>
<pre><code>import squidpy as sq

adata = sq.datasets.visium_fluo_adata()
img   = sq.datasets.visium_fluo_image_crop()</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">2</span> Extract image features</div>
<pre><code>sq.im.calculate_image_features(
    adata,
    img,
    features=["summary", "texture", "histogram"],
    key_added="img_features"
)</code></pre>
  <p>Summary features capture mean and standard deviation of pixel intensities per channel. Texture features (GLCM-based) capture spatial relationships between pixel intensities. Histogram features capture the full distribution of pixel values across intensity bins.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">3</span> Build spatial neighborhood graph</div>
<pre><code>sq.gr.spatial_neighbors(adata, coord_type="grid", n_neighs=6)</code></pre>
  <p><code>coord_type="grid"</code> is appropriate for Visium because spots are arranged in a regular hexagonal grid. This graph is the foundation for all spatial statistics in Squidpy.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">4</span> Spatial autocorrelation — Moran's I</div>
<pre><code>sq.gr.spatial_autocorr(adata, mode="moran")
adata.uns["moranI"].sort_values("I", ascending=False).head(10)</code></pre>
  <p>Moran's I ranges from -1 to +1. A score near +1 means the gene is expressed in spatially clustered patterns. A score near 0 indicates random spatial distribution. Negative scores indicate spatial dispersion.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">5</span> Visualize</div>
<pre><code>sq.pl.spatial_scatter(adata, color=["leiden", "GENE_OF_INTEREST"])</code></pre>
</div>

<!-- ── 2B ── -->
<h3 id="visium-hne">2B — Visium H&amp;E Data</h3>
<p><strong>Reference:</strong> <a href="https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_visium_hne.html">Analyze Visium H&amp;E Data</a></p>

<p>Hematoxylin and eosin staining is the gold standard in histopathology. Hematoxylin stains nuclei dark blue-purple; eosin stains cytoplasm and extracellular matrix pink. H&amp;E images are single brightfield images — not multichannel — and analysis focuses on tissue morphology and cellular organization rather than fluorescence intensity. This module introduces the most biologically interpretive analyses in the series: neighborhood enrichment, co-occurrence analysis, and ligand-receptor interaction inference.</p>

<h4>What This Module Teaches</h4>
<ul>
  <li>H&amp;E image handling and tissue segmentation using Squidpy</li>
  <li>Neighborhood enrichment scoring — which clusters are spatially adjacent more than expected by chance</li>
  <li>Co-occurrence analysis — how cluster proximity changes as a function of spatial distance</li>
  <li>Ligand-receptor interaction inference between neighboring cell-type clusters</li>
  <li>Interpreting enrichment matrices and interaction dot plots</li>
</ul>

<div class="io-grid">
  <div class="io-card io-input">
    <div class="io-card-header">Input</div>
    <table>
      <thead><tr><th>Item</th><th>Format</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td>Visium expression data</td><td><code>.h5ad</code></td><td>Spot-level count matrix</td></tr>
        <tr><td>H&amp;E tissue image</td><td><code>.png</code> / <code>.tiff</code></td><td>Single-channel brightfield image</td></tr>
        <tr><td>Cluster annotations</td><td><code>adata.obs['leiden']</code></td><td>Leiden labels from standard pipeline</td></tr>
        <tr><td>LR database</td><td>Internal to Squidpy</td><td>Curated gene interaction pairs</td></tr>
      </tbody>
    </table>
  </div>
  <div class="io-card io-output">
    <div class="io-card-header">Output</div>
    <table>
      <thead><tr><th>Item</th><th>Format</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td>Segmentation mask</td><td><code>img['segmentation']</code></td><td>Per-pixel cell segmentation</td></tr>
        <tr><td>Enrichment matrix</td><td><code>adata.uns</code></td><td>Z-scores of cluster co-localization</td></tr>
        <tr><td>Co-occurrence scores</td><td><code>adata.uns</code></td><td>Proximity curves across distances</td></tr>
        <tr><td>LR results</td><td><code>adata.uns</code></td><td>p-values per interaction pair</td></tr>
        <tr><td>Enrichment heatmap</td><td><code>.png</code></td><td>Cluster-by-cluster spatial matrix</td></tr>
        <tr><td>Interaction dot plot</td><td><code>.png</code></td><td>LR pairs colored by significance</td></tr>
      </tbody>
    </table>
  </div>
</div>

<h4>Step-by-Step</h4>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">1</span> Load data and image</div>
<pre><code>adata = sq.datasets.visium_hne_adata()
img   = sq.datasets.visium_hne_image_crop()</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">2</span> Tissue segmentation</div>
<pre><code>sq.im.segment(img, method="watershed", layer="image", channel=0)

sq.im.calculate_image_features(
    adata, img,
    features=["segmentation"],
    layer="image",
    key_added="seg_features"
)</code></pre>
  <p>Segmentation identifies individual cell boundaries within the tissue using the watershed algorithm, enabling spot-level morphological quantification even in spot-based data.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">3</span> Spatial neighborhood graph</div>
<pre><code>sq.gr.spatial_neighbors(adata, coord_type="grid")</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">4</span> Neighborhood enrichment</div>
<pre><code>sq.gr.nhood_enrichment(adata, cluster_key="leiden")
sq.pl.nhood_enrichment(adata, cluster_key="leiden", method="average", cmap="inferno")</code></pre>
  <p>Output is a symmetric z-score matrix. A positive z-score between two clusters means they are found as spatial neighbors more often than random chance predicts. Negative values indicate spatial avoidance.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">5</span> Co-occurrence analysis</div>
<pre><code>sq.gr.co_occurrence(adata, cluster_key="leiden")
sq.pl.co_occurrence(adata, cluster_key="leiden", clusters="Cluster_1")</code></pre>
  <p>Scores describe how the conditional probability of observing one cluster changes as you move outward from another, across a series of distance thresholds. This distinguishes short-range from long-range spatial relationships.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-vis">6</span> Ligand-receptor interaction inference</div>
<pre><code>sq.gr.ligrec(
    adata,
    n_perms=1000,
    cluster_key="leiden",
    copy=False,
    use_raw=False
)
sq.pl.ligrec(adata, cluster_key="leiden", source_groups="Cluster_1")</code></pre>
  <p>Tests whether known ligand-receptor pairs are co-expressed in neighboring clusters above chance (permutation test, n=1000). Identifies candidate cell-cell communication pathways operating within the tissue.</p>
</div>

<!-- ── Comparison ── -->
<h3 id="visium-compare">Visium Platform Comparison</h3>

<div class="compare-banner">
  Both modules share the same upstream pipeline (QC, normalization, clustering) and the same spatial graph construction step. Differences arise entirely from the imaging modality and the biological questions each enables.
</div>

<table class="table-visium">
  <thead>
    <tr><th>Feature</th><th>Module 2A — Fluorescence</th><th>Module 2B — H&amp;E</th></tr>
  </thead>
  <tbody>
    <tr><td>Staining type</td><td>Multichannel fluorescence</td><td>Brightfield hematoxylin and eosin</td></tr>
    <tr><td>Image channels</td><td>Multiple (DAPI, marker channels)</td><td>Single</td></tr>
    <tr><td>Image analysis focus</td><td>Intensity per channel, texture</td><td>Morphology, cell segmentation</td></tr>
    <tr><td>Key image features</td><td>Summary, texture, histogram</td><td>Segmentation-based cell counts</td></tr>
    <tr><td>Primary spatial statistic</td><td>Moran's I autocorrelation</td><td>Neighborhood enrichment, co-occurrence</td></tr>
    <tr><td>Cell communication</td><td>Not covered</td><td>Ligand-receptor inference (ligrec)</td></tr>
    <tr><td>Primary biological output</td><td>Spatially structured gene programs</td><td>Cell-cell communication pathways</td></tr>
  </tbody>
</table>

<hr>

<!-- ═══════════════════════════════════════════════════════ XENIUM -->
<h2 class="h-xenium" id="xenium">Module 4 — Xenium In Situ Analysis</h2>

<p>
  <span class="module-badge badge-advanced">Advanced</span>
  <strong>Reference:</strong> <a href="https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_xenium.html">Analyze Xenium Data</a>
</p>

<p>10x Genomics Xenium is an in situ spatial transcriptomics platform that provides <strong>subcellular resolution</strong>. Unlike Visium, which captures gene expression at the level of spatial spots — each containing several cells — Xenium directly images RNA molecules inside tissue using fluorescence-labeled probes and assigns each transcript to a specific cell based on its physical coordinates. This is true single-cell spatial transcriptomics.</p>

<p>This module introduces <code>SpatialData</code>, a framework designed for the complexity of subcellular spatial data where transcripts, cell boundaries, and tissue images must all coexist in a shared coordinate system.</p>

<h3>Visium vs Xenium — Platform Comparison</h3>

<table class="table-xenium">
  <thead>
    <tr><th>Feature</th><th>Visium (Modules 2 &amp; 3)</th><th>Xenium (Module 4)</th></tr>
  </thead>
  <tbody>
    <tr><td>Resolution</td><td>Spot-level (~55 µm, ~10 cells/spot)</td><td>Single-cell and subcellular</td></tr>
    <tr><td>Transcript assignment</td><td>Passively captured by spot position</td><td>Localized by fluorescence probe imaging</td></tr>
    <tr><td>Gene panel</td><td>Whole transcriptome</td><td>Targeted panel (100–5000 genes)</td></tr>
    <tr><td>Cells per unit</td><td>Multiple cells per spot</td><td>One cell per barcode</td></tr>
    <tr><td>Output format</td><td>Feature-barcode matrix + image</td><td>Transcript coords + boundaries + image</td></tr>
    <tr><td>Data structure</td><td><code>AnnData</code></td><td><code>SpatialData</code></td></tr>
    <tr><td>Image type</td><td>H&amp;E or fluorescence scan</td><td>OME-TIFF z-stack (DAPI morphology)</td></tr>
    <tr><td>Spatial resolution</td><td>~55 µm</td><td>&lt; 1 µm (subcellular)</td></tr>
  </tbody>
</table>

<h3>What This Module Teaches</h3>
<ul>
  <li>Loading and exploring a <code>SpatialData</code> object produced from Xenium output</li>
  <li>Understanding the relationship between transcripts, cells, boundaries, and images in one coordinate system</li>
  <li>Applying a standard scRNA-seq pipeline to true single-cell Xenium data</li>
  <li>Visualizing individual transcript positions at subcellular resolution</li>
  <li>Performing spatial statistics at single-cell rather than spot-level resolution</li>
</ul>

<div class="io-grid">
  <div class="io-card io-input">
    <div class="io-card-header">Input</div>
    <table>
      <thead><tr><th>Item</th><th>Format</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td>Cell feature matrix</td><td>MEX directory</td><td>Gene-by-cell count matrix</td></tr>
        <tr><td>Transcript coordinates</td><td><code>transcripts.csv.gz</code></td><td>x, y, z of every RNA molecule</td></tr>
        <tr><td>Cell metadata</td><td><code>cells.csv.gz</code></td><td>Transcript count, nucleus area, centroid</td></tr>
        <tr><td>Nucleus boundaries</td><td><code>nucleus_boundaries.csv.gz</code></td><td>Polygon vertices for each nucleus</td></tr>
        <tr><td>Cell boundaries</td><td><code>cell_boundaries.csv.gz</code></td><td>Polygon vertices for each cell</td></tr>
        <tr><td>Morphology image</td><td><code>.ome.tif</code></td><td>Maximum intensity projection DAPI</td></tr>
      </tbody>
    </table>
  </div>
  <div class="io-card io-output">
    <div class="io-card-header">Output</div>
    <table>
      <thead><tr><th>Item</th><th>Format</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td>SpatialData object</td><td><code>sdata</code></td><td>All data layers in one coordinate system</td></tr>
        <tr><td>Processed AnnData</td><td><code>.h5ad</code></td><td>QC-filtered, normalized, clustered cells</td></tr>
        <tr><td>UMAP embedding</td><td><code>.png</code></td><td>Cell-level UMAP colored by cluster</td></tr>
        <tr><td>Spatial cell map</td><td><code>.png</code></td><td>Clusters on DAPI morphology image</td></tr>
        <tr><td>Transcript scatter</td><td><code>.png</code></td><td>Individual RNA positions across tissue</td></tr>
        <tr><td>Marker gene table</td><td><code>.csv</code></td><td>Top DE genes per cluster</td></tr>
      </tbody>
    </table>
  </div>
</div>

<h3>Step-by-Step</h3>

<div class="step">
  <div class="step-title"><span class="step-num num-xen">1</span> Load Xenium output into SpatialData</div>
<pre><code>import spatialdata_io as sdio

sdata = sdio.xenium("path/to/xenium/output/")
print(sdata)</code></pre>

  <table class="table-xenium" style="margin-top:12px;">
    <thead><tr><th>Element</th><th>Key</th><th>Content</th></tr></thead>
    <tbody>
      <tr><td>Cell circles</td><td><code>sdata["cell_circles"]</code></td><td>Simplified circular cell representations</td></tr>
      <tr><td>Cell boundaries</td><td><code>sdata["cell_boundaries"]</code></td><td>Precise polygon outlines per cell</td></tr>
      <tr><td>Nucleus boundaries</td><td><code>sdata["nucleus_boundaries"]</code></td><td>Polygon outlines per nucleus</td></tr>
      <tr><td>Transcripts</td><td><code>sdata["transcripts"]</code></td><td>All detected RNA molecules with x, y, z</td></tr>
      <tr><td>Morphology image</td><td><code>sdata["morphology_mip"]</code></td><td>DAPI maximum intensity projection</td></tr>
    </tbody>
  </table>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-xen">2</span> Extract cell-level AnnData</div>
<pre><code>import scanpy as sc

adata = sdata["cell_circles"].values</code></pre>
  <p>This produces a standard <code>AnnData</code> object — one row per cell, one column per gene. All standard Scanpy functions apply from this point.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-xen">3</span> Quality control</div>
<pre><code>sc.pp.calculate_qc_metrics(adata, percent_top=None, log1p=False, inplace=True)
sc.pp.filter_cells(adata, min_counts=10)
sc.pp.filter_genes(adata, min_cells=5)</code></pre>
  <p>A threshold of 10 transcripts is conservative for targeted panels of 100–500 genes. Adjust based on your panel size.</p>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-xen">4</span> Normalization</div>
<pre><code>sc.pp.normalize_total(adata)
sc.pp.log1p(adata)</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-xen">5</span> Dimensionality reduction and clustering</div>
<pre><code>sc.pp.pca(adata)
sc.pp.neighbors(adata)
sc.tl.umap(adata)
sc.tl.leiden(adata, random_state=42)</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-xen">6</span> Spatial visualization at single-cell resolution</div>
<pre><code>import squidpy as sq

sq.pl.spatial_scatter(adata, color="leiden", shape="hex", size=20)</code></pre>
</div>

<div class="step">
  <div class="step-title"><span class="step-num num-xen">7</span> Transcript-level visualization</div>
<pre><code>sq.pl.spatial_scatter(adata, color="GENE_OF_INTEREST", shape=None, size=3)</code></pre>
  <p>This capability is unique to in situ platforms — individual RNA molecule positions plotted directly on tissue, revealing subcellular localization of any gene of interest.</p>
</div>

<hr>

<!-- ═══════════════════════════════════════════════════════ FORMATS -->
<h2 class="h-general" id="formats">Data and File Formats</h2>

<h3>AnnData Object Structure</h3>

<table class="table-general">
  <thead><tr><th>Slot</th><th>Content</th><th>Type</th></tr></thead>
  <tbody>
    <tr><td><code>adata.X</code></td><td>Gene expression count matrix (spots or cells × genes)</td><td>Sparse matrix</td></tr>
    <tr><td><code>adata.obs</code></td><td>Per-observation metadata: QC metrics, cluster labels</td><td>DataFrame</td></tr>
    <tr><td><code>adata.var</code></td><td>Per-gene metadata: gene names, highly variable flags</td><td>DataFrame</td></tr>
    <tr><td><code>adata.obsm['spatial']</code></td><td>2D tissue coordinates for each spot or cell</td><td>NumPy array</td></tr>
    <tr><td><code>adata.uns['spatial']</code></td><td>Tissue images and scale factors (Visium)</td><td>Dictionary</td></tr>
    <tr><td><code>adata.obsp['spatial_connectivities']</code></td><td>Spatial neighborhood graph adjacency matrix</td><td>Sparse matrix</td></tr>
    <tr><td><code>adata.uns['moranI']</code></td><td>Moran's I autocorrelation results per gene</td><td>DataFrame</td></tr>
    <tr><td><code>adata.uns['leiden_nhood_enrichment']</code></td><td>Neighborhood enrichment z-scores (cluster × cluster)</td><td>Dictionary</td></tr>
    <tr><td><code>adata.uns['leiden_co_occurrence']</code></td><td>Co-occurrence scores across distance bins</td><td>Dictionary</td></tr>
    <tr><td><code>adata.uns['leiden_ligrec']</code></td><td>Ligand-receptor interaction p-values and means</td><td>Dictionary</td></tr>
  </tbody>
</table>

<h3>Space Ranger Output (Visium)</h3>
<div class="tree">
<pre><code>spaceranger_output/
├── filtered_feature_bc_matrix/
│   ├── barcodes.tsv.gz          # One barcode per spot passing the tissue filter
│   ├── features.tsv.gz          # Ensembl gene IDs and HGNC gene names
│   └── matrix.mtx.gz            # Sparse count matrix in Matrix Market format
└── spatial/
    ├── tissue_positions_list.csv # Barcode-to-pixel coordinate mapping
    ├── scalefactors_json.json    # Image downscale factors per resolution
    ├── tissue_hires_image.png    # High-resolution tissue scan
    └── tissue_lowres_image.png   # Low-resolution tissue scan</code></pre>
</div>

<h3>Xenium Output</h3>
<div class="tree">
<pre><code>xenium_output/
├── cell_feature_matrix/          # Gene-by-cell count matrix (MEX format)
│   ├── barcodes.tsv.gz
│   ├── features.tsv.gz
│   └── matrix.mtx.gz
├── transcripts.csv.gz            # Every transcript: x, y, z, gene name, cell_id
├── cells.csv.gz                  # Per-cell summary: centroid, transcript count, area
├── nucleus_boundaries.csv.gz     # Polygon vertex coordinates for each nucleus
├── cell_boundaries.csv.gz        # Polygon vertex coordinates for each cell
└── morphology_mip.ome.tif        # Maximum intensity projection DAPI image (OME-TIFF)</code></pre>
</div>

<hr>

<!-- ═══════════════════════════════════════════════════════ STACK -->
<h2 class="h-general" id="stack">Software Stack</h2>

<table class="table-general">
  <thead><tr><th>Package</th><th>Version</th><th>Role in This Series</th></tr></thead>
  <tbody>
    <tr><td><code>Python</code></td><td>&gt;= 3.9</td><td>Programming language</td></tr>
    <tr><td><code>scanpy</code></td><td>&gt;= 1.9</td><td>QC, normalization, PCA, UMAP, clustering, marker genes — all modules</td></tr>
    <tr><td><code>squidpy</code></td><td>&gt;= 1.3</td><td>Spatial graph, Moran's I, enrichment, co-occurrence, ligrec, image features — Modules 2–4</td></tr>
    <tr><td><code>anndata</code></td><td>&gt;= 0.9</td><td>Core data structure shared across all modules</td></tr>
    <tr><td><code>spatialdata</code></td><td>&gt;= 0.1</td><td>Unified multi-element data structure for Xenium — Module 4</td></tr>
    <tr><td><code>spatialdata-io</code></td><td>&gt;= 0.1</td><td>Data loaders for Xenium and other in situ platforms — Module 4</td></tr>
    <tr><td><code>matplotlib</code></td><td>&gt;= 3.6</td><td>Base plotting library</td></tr>
    <tr><td><code>seaborn</code></td><td>&gt;= 0.12</td><td>Statistical plots and heatmaps</td></tr>
    <tr><td><code>numpy</code></td><td>&gt;= 1.23</td><td>Numerical operations</td></tr>
    <tr><td><code>pandas</code></td><td>&gt;= 1.5</td><td>Tabular data manipulation</td></tr>
    <tr><td><code>scikit-learn</code></td><td>&gt;= 1.1</td><td>PCA internals and machine learning utilities</td></tr>
    <tr><td><code>leidenalg</code></td><td>&gt;= 0.9</td><td>Leiden clustering algorithm</td></tr>
  </tbody>
</table>

<hr>

<!-- ═══════════════════════════════════════════════════════ COLAB -->
<h2 class="h-general" id="colab">Running in Google Colab</h2>

<div class="step">
  <div class="step-title"><span class="step-num num-gen">1</span> Open the notebook</div>
  <p>Click the Colab link next to the module you want to run. The notebook opens in your browser with all code pre-loaded.</p>
</div>
<div class="step">
  <div class="step-title"><span class="step-num num-gen">2</span> Connect to a runtime</div>
  <p>Click <strong>Connect</strong> in the top-right corner of the Colab interface. Select a CPU runtime. No GPU is required for any module in this series.</p>
</div>
<div class="step">
  <div class="step-title"><span class="step-num num-gen">3</span> Run the setup cell</div>
  <p>The first code cell installs all required packages. Run it before any other cell. This typically takes 2 to 4 minutes. Skipping this step will cause import errors throughout the notebook.</p>
</div>
<div class="step">
  <div class="step-title"><span class="step-num num-gen">4</span> Run cells sequentially</div>
  <p>Every cell depends on the variables created by the cells before it. Running out of order will produce <code>NameError</code>, <code>KeyError</code>, or <code>AttributeError</code> exceptions. Work strictly from top to bottom.</p>
</div>
<div class="step">
  <div class="step-title"><span class="step-num num-gen">5</span> Save your progress</div>
  <p>Go to <strong>File &gt; Save a copy in Drive</strong> to save the notebook — including all outputs — to your Google Drive. Colab does not automatically save outputs between sessions.</p>
</div>

<h3>Runtime Reference</h3>
<table class="table-general">
  <thead><tr><th>Consideration</th><th>Detail</th></tr></thead>
  <tbody>
    <tr><td>Session timeout</td><td>Disconnects after ~90 minutes of inactivity</td></tr>
    <tr><td>On reconnect</td><td>All in-memory variables are lost — re-run from the top</td></tr>
    <tr><td>Dataset source</td><td>All tutorial datasets are fetched automatically — no upload needed</td></tr>
    <tr><td>RAM limit</td><td>~12 GB on standard Colab CPU runtime; sufficient for all four modules</td></tr>
    <tr><td>Disk limit</td><td>~107 GB on standard Colab runtime; sufficient for all sample datasets</td></tr>
  </tbody>
</table>

<hr>

<!-- ═══════════════════════════════════════════════════════ TROUBLESHOOTING -->
<h2 class="h-general" id="troubleshooting">Troubleshooting</h2>

<h3>Import errors after the setup cell</h3>
<p>Restart the Colab runtime (<strong>Runtime &gt; Restart runtime</strong>) and re-run all cells from the top. Version conflicts during package installation are typically resolved on a clean restart.</p>

<h3><code>ValueError: obsm['spatial'] not found</code></h3>
<p>This appears when a spatial plot is attempted before spatial coordinates have been loaded. Ensure you used <code>sc.read_visium()</code> for Visium data and that the <code>spatial/</code> directory is present in the Space Ranger output folder passed to the reader.</p>

<h3>Memory error during image feature extraction</h3>
<p>Reduce the processing scale for large tissue sections:</p>
<pre><code>sq.im.calculate_image_features(adata, img, scale=0.5, key_added="img_features")</code></pre>

<h3>Leiden clustering gives different results on each run</h3>
<p>Set an explicit seed to reproduce results:</p>
<pre><code>sc.tl.leiden(adata, random_state=42)</code></pre>

<h3>Dataset download fails mid-notebook</h3>
<p>Squidpy's built-in dataset loaders download from external servers. If a download fails, check your internet connection and retry the cell.</p>

<h3><code>KeyError</code> when accessing <code>adata.uns['moranI']</code></h3>
<p>Confirm that <code>sq.gr.spatial_neighbors()</code> was run before <code>sq.gr.spatial_autocorr()</code>. The spatial graph is a prerequisite for autocorrelation computation.</p>

<h3>Xenium loading fails with <code>FileNotFoundError</code></h3>
<p>Confirm the path passed to <code>sdio.xenium()</code> points to the root Xenium output directory — the folder directly containing <code>cells.csv.gz</code>, <code>transcripts.csv.gz</code>, and the <code>cell_feature_matrix/</code> subfolder. Do not point to a subdirectory.</p>

<hr>

<!-- ═══════════════════════════════════════════════════════ REFERENCES -->
<h2 class="h-general" id="references">References</h2>

<h3>Tutorial Documentation</h3>
<ul>
  <li>Scanpy Spatial Tutorials — <a href="https://scanpy-tutorials.readthedocs.io/en/latest/spatial/">scanpy-tutorials.readthedocs.io</a></li>
  <li>Squidpy Documentation — <a href="https://squidpy.readthedocs.io/en/stable/">squidpy.readthedocs.io</a></li>
  <li>SpatialData Documentation — <a href="https://spatialdata.scverse.org/en/latest/">spatialdata.scverse.org</a></li>
</ul>

<h3>Key Publications</h3>
<ul>
  <li>Palla et al. (2022). Squidpy: a scalable framework for spatial omics analysis. <em>Nature Methods</em>, 19, 171–178.</li>
  <li>Wolf et al. (2018). SCANPY: large-scale single-cell gene expression data analysis. <em>Genome Biology</em>, 19, 15.</li>
  <li>Ståhl et al. (2016). Visualization and analysis of gene expression in tissue sections by spatial transcriptomics. <em>Science</em>, 353(6294), 78–82.</li>
  <li>Virshup et al. (2021). anndata: Annotated data. <em>bioRxiv</em>.</li>
</ul>

<h3>10x Genomics Platform Documentation</h3>
<ul>
  <li>Visium Spatial Gene Expression — <a href="https://www.10xgenomics.com/products/spatial-gene-expression">10xgenomics.com</a></li>
  <li>Xenium In Situ — <a href="https://www.10xgenomics.com/products/xenium-in-situ">10xgenomics.com</a></li>
  <li>Space Ranger Pipeline — <a href="https://support.10xgenomics.com/spatial-gene-expression/software/pipelines/latest/what-is-space-ranger">support.10xgenomics.com</a></li>
</ul>

<h3>Community Resources</h3>
<ul>
  <li>scverse ecosystem — <a href="https://scverse.org">scverse.org</a></li>
  <li>Bioinformatics Stack Exchange — <a href="https://bioinformatics.stackexchange.com">bioinformatics.stackexchange.com</a></li>
</ul>

<!-- ═══════════════════════════════════════════════════════ FOOTER -->
<div class="page-footer">
  <p>This tutorial series is intended for educational use. All datasets are publicly available and fetched automatically within each notebook.</p>
  <div class="footer-pills">
    <span class="pill pill-blue">Scanpy</span>
    <span class="pill pill-purple">Squidpy</span>
    <span class="pill pill-red">SpatialData</span>
    <span class="pill pill-green">AnnData</span>
  </div>
</div>

</body>
</html>
