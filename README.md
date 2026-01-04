# Gene Expression Analysis

## Project Overview
Breast cancer is a heterogeneous disease comprising multiple molecular subtypes. One aggressive subtype is HER2-positive cancer, characterized by 
amplification of the ERBB2 (HER2) oncogene and accounting for roughly 11–30% of cases. This project compares RNA-seq profiles of HER2-amplified 
versus non-amplified breast tumors to identify differentially expressed genes (DEGs), key pathways, and potential clinical 
biomarkers. The aim is to elucidate HER2-driven molecular mechanisms and their clinical significance. In particular, we seek DEGs and enriched 
pathways that distinguish HER2+ tumors and may suggest targets for therapy or prognosis.

## Repository Structure
- __GeneExpressionAnalysis.qmd__ – Main analysis document (Quarto/Markdown) containing R code and narrative of the workflow.
- __Gene expression analysis.pdf__ – Final report in PDF format with figures, detailed results, and interpretations.
- __GeneExpressionAnalysis.html__ – Rendered HTML report of the analysis (output from the .qmd).
- __GeneExpressionAnalysis.R__ – Alternative R script version of the analysis for users preferring a script (optional, mirrors the .qmd steps).
- __chunks_description_for_R_format.pdf__ – An explanation of the code execution (by code labelled as chunks corresponding to the GeneExpressionAnalysis.R file)
(optional, mirrors the .qmd steps).
- __books.bib__ — BibTeX file with references cited in the analysis. 
> [!TIP]
> I recommend using files **Gene expression analysis.pdf** and **GeneExpressionAnalysis.qmd**, as they contain all the necessary information.

## Data Description
Data were obtained from the cBioPortal for Cancer Genomics, specifically the [TCGA Breast Invasive Carcinoma (BRCA) PanCancer Atlas 2018 dataset](https://www.cbioportal.org/study/summary?id=brca_tcga_pan_can_atlas_2018). 
Three data types were used:
- RNA-Seq gene expression (mRNA-seq counts) for tumor samples.
- Copy number variation (CNV) data to identify HER2 (ERBB2) amplification status.
- Clinical data including patient identifiers, survival outcomes, and other covariates.
These datasets together allow classification of tumors by HER2 amplification (via CNV) and correlation of gene expression with clinical outcomes.

## Analysis Workflow
The analysis was conducted in R, using established Bioconductor and CRAN packages. 
The pipeline included the following steps:

## Results Summary




## Target Audience
This work is intended for researchers in computational biology, cancer genomics, and related fields. It will be most useful to bioinformaticians 
and data scientists with interest in transcriptomics and machine learning (AI/ML) methods, as well as oncologists seeking computational insights 
into HER2-positive breast cancer. The analyses assume familiarity with R programming and genomic data analysis techniques.

__References:__ Full citations and discussion of methods are provided in the report and accompanying bibliography.
