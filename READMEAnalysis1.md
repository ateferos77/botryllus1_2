# Botryllus Brain scRNA-seq Analysis - Mapping 1 Report

## Introduction

This notebook presents the analysis of Botryllus brain single-cell RNA sequencing (scRNA-seq) data using the first human gene reference mapping approach. Botryllus schlosseri, a colonial tunicate, represents an important model organism for studying regeneration, development, and evolutionary biology due to its unique biological characteristics including whole-body regeneration capabilities.

### Dataset Overview
- **Organism**: *Botryllus schlosseri* (colonial tunicate)
- **Tissue**: Brain tissue
- **Technology**: Single-cell RNA sequencing
- **Mapping Reference**: [First human gene reference ]
- **Analysis Goal**: Characterize cell types and gene expression patterns in Botryllus brain tissue

### Objectives
1. Quality control and preprocessing of scRNA-seq data
2. Cell clustering and identification of distinct cell populations
3. Differential gene expression analysis
4. Functional annotation using human gene orthologs
5. Visualization of cell type distributions and marker genes

### Analysis Pipeline
This analysis follows standard scRNA-seq processing workflows including:
- Data loading and initial quality assessment
- Filtering and normalization
- Dimensionality reduction (PCA, UMAP/t-SNE)
- Clustering analysis
- Marker gene identification
- Cell type annotation

*Note: This is the first of two mapping approaches. A comparative analysis with the second mapping will be conducted separately.*
## Data Preprocessing

### 1. Dataset Overview

The raw dataset contains single-cell RNA sequencing data from Botryllus brain tissue samples. Initial examination reveals the structure and quality of the data before processing.

**Table 1: Raw Dataset Dimensions**
| Metric | Value |
|--------|-------|
| Total Genes | 44,727 |
| Total Samples/Cells | 683 |
| Data Matrix Size | 44,727 × 683 |

### 2. Sample Information Extraction

Column names were parsed to extract biological metadata from the filename structure. Each sample contains the following information:

**Table 2: Sample Metadata Structure**
| Component | Example | Description |
|-----------|---------|-------------|
| Sample ID | ILW100, ILW101 | Unique biological sample identifier |
| Brain Region | B2, AB | Specific brain region analyzed |
| Age | 19_months | Developmental time point |
| Replicate | p05c01r01 | Plate-cell-replicate information |
| Sample Number | S385, S401 | Sequential sample numbering |


### 3. Gene Annotation Quality Assessment

**Table 3: Gene Annotation Completeness**
| Metric | Count | Percentage |
|--------|-------|-----------|
| Genes with Gene_ID | 44,727 | 100.0% |
| Genes with Gene_Name | 11,348 | 25.4% |
| Genes without Gene_Name (NaN) | 33,379 | 74.6% |
| Duplicate Gene_IDs | 0 | 0.0% |
| Duplicate Gene_Names | Yes | - |

### 4. Data Cleaning and Standardization

**Table 5: Gene ID Standardization Results**
| Processing Step | Before | After |
|----------------|--------|-------|
| Original Gene_ID format | FUN_000001 | g1 |
| Genes retained after filtering | 44,727 | 44,727 |
| Final unique gene identifiers | - | g_unique format |

**Table 6: Final Processed Dataset Dimensions**
| Component | Dimension |
|-----------|-----------|
| Genes (rows) | 44,727 |
| Samples (columns) | 683 |
| Matrix format | Gene × Sample |
| Index format | g_unique (Gene_ID_Gene_Name) |

### 5. Data Quality Summary

**Table 7: Preprocessing Summary Statistics**
| Quality Metric | Value | Status |
|----------------|-------|--------|
| Missing Gene_IDs | 0 | ✓ Clean |
| Gene annotation coverage | 25.4% | ⚠ Limited |
| Duplicate entries | None | ✓ Clean |
| Data completeness | 100% | ✓ Ready |
| Final matrix ready for analysis | Yes | ✓ Proceed |

The preprocessing successfully standardized sample naming conventions, cleaned gene annotations, and prepared a analysis-ready matrix with 44,727 genes across 683 single-cell samples from Botryllus brain tissue.

