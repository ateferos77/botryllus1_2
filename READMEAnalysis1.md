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

## AnnData Object Creation and Metadata Integration

### 1. Data Structure Conversion

The preprocessed count matrix was converted into an AnnData object, the standard format for single-cell analysis in Python. This structure efficiently organizes expression data alongside cell and gene metadata.

**Table 8: AnnData Object Structure**
| Component | Description | Dimension |
|-----------|-------------|-----------|
| X (Expression Matrix) | Gene expression counts | 683 cells × 44,727 genes |
| obs (Cell Metadata) | Sample/cell annotations | 683 observations |
| var (Gene Metadata) | Gene annotations | 44,727 variables |
| Data Format | Transposed from original | Cells as rows, Genes as columns |

### 2. Sample Metadata Extraction

Cell identifiers were parsed to extract biological and technical metadata, creating structured annotations for downstream analysis.

**Table 9: Extracted Metadata Categories**
| Metadata Field | Description | Example Values | 
|----------------|-------------|----------------|
| Sample_ID | Biological sample identifier | ILW100, ILW101, ILW102 | 
| Region | Brain region analyzed | B2, AB | 
| Age | Developmental time point | 19_months | 
| Replicate | Technical replicate info | p05c01r01, p06c05r02 | 
| Sample_Number | Sequential sample ID | S385, S401, S417 | 

### 3. Data Organization Summary

**Table 10: AnnData Object Validation**
| Validation Check | Result | Status |
|------------------|--------|--------|
| Matrix dimensions | 683 × 44,727 | ✓ Correct |
| Cell metadata completeness | 100% | ✓ Complete |
| Metadata fields extracted | 5 fields | ✓ Success |
| Data type consistency | Numeric matrix | ✓ Valid |
| Index alignment | Cells-Genes matched | ✓ Aligned |

The AnnData object successfully integrates the expression matrix with comprehensive sample metadata, providing a structured foundation for quality control and downstream single-cell analysis. All 683 cells retain their biological annotations across 5 metadata categories.

## Quality Control and Filtering

### 1. Initial Quality Metrics Calculation

Quality control metrics were computed to assess cell and gene quality before filtering. Two key metrics were calculated for each cell: total UMI counts and number of expressed genes.

**Table 11: Pre-filtering Dataset Statistics**
| Metric | Value |
|--------|-------|
| Initial cells | 683 |
| Initial genes | 44,727 |
| Cells with ≥100 total counts | 581 |
| Cell retention rate | 85.1% |

### 2. Cell Filtering Results

Stringent quality control filters were applied to remove low-quality cells that could introduce noise in downstream analysis.

**Table 12: Cell Filtering Summary**
| Metric | Value |
|--------|-------|
| Initial cells | 683 |
| Cells removed (< 100 counts) | 102 |
| Final cells retained | 581 |
| **Cell retention rate** | **85.1%** |

### 3. Gene Filtering Results

Genes expressed in fewer than 10 cells were removed to focus on reliably detected genes and reduce computational burden.

**Table 13: Gene Filtering Summary**
| Metric | Value |
|--------|-------|
| Initial genes | 44,727 |
| Genes removed (< 10 cells) | 30,069 |
| Final genes retained | 14,658 |
| **Gene retention rate** | **32.8%** |

### 4. Gene Expression Sparsity Analysis

Analysis of gene expression sparsity reveals the distribution of zero values across genes, indicating the challenge of single-cell data sparsity.

**Table 14: Gene Expression Sparsity Metrics**
| Sparsity Metric | Value | Interpretation |
|-----------------|-------|----------------|
| Genes with >95% zeros | 38.4% | High sparsity genes |
| Genes with ≤95% zeros | 61.6% | Moderately expressed genes |
| Average sparsity threshold | 95% | Stringent detection limit |

### 5. Final Quality Control Summary

**Table 15: Post-QC Dataset Characteristics**
| Quality Metric | Value | Change from Raw | Status |
|----------------|-------|-----------------|--------|
| **Final cells** | 581 | -14.9% | ✓ High quality retained |
| **Final genes** | 14,658 | -67.2% | ✓ Reliable genes selected |
| **Matrix sparsity** | Reduced | Improved | ✓ Enhanced signal |
| **Data dimensions** | 581 × 14,658 | Optimized | ✓ Analysis ready |

### 6. Quality Control Impact Assessment

**Table 16: Filtering Effectiveness**
| Aspect | Benefit | Quantification |
|--------|---------|----------------|
| Cell quality | Removed low-count cells | 102 cells (14.9%) filtered |
| Gene reliability | Retained broadly expressed genes | 14,658 genes (32.8%) kept |
| Computational efficiency | Reduced matrix size | 67% reduction in features |
| Signal-to-noise ratio | Enhanced by sparsity reduction | 38.4% highly sparse genes identified |

The quality control process successfully filtered the dataset from 683 × 44,727 to 581 × 14,658, removing 14.9% of cells and 67.2% of genes while retaining high-quality, reliably expressed features. This filtered dataset provides a robust foundation for subsequent normalization and cell type identification analysis.

## Normalization

Standard log normalization was applied to prepare the data for downstream analysis. This approach is particularly suitable for Botryllus data due to limited gene annotation and cross-species mapping challenges.

**Table 17: Normalization Summary**
| Step | Method | Purpose | Result |
|------|--------|---------|--------|
| 1 | Total count normalization | Remove sequencing depth variation | Target: 10,000 counts/cell |
| 2 | Log transformation log(x+1) | Variance stabilization | Log-scale expression values |

**Table 18: Normalization Rationale for Non-Model Organisms**
| Factor | Why Log Normalization |
|--------|----------------------|
| Limited annotation (25.4%) | Robust, assumption-free method |
| High sparsity (38.4% sparse genes) | Handles zero inflation effectively |
| Cross-species mapping | Reduces technical artifacts |

The normalized dataset maintains 581 cells × 14,658 genes with standardized expression values ready for dimensionality reduction and clustering analysis.


