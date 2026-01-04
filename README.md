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
- **Data Acquisition:** RNA-seq expression, copy-number and clinical data for TCGA Breast Invasive Carcinoma (PanCancer Atlas 2018) were downloaded via cBioPortal. Patient identifiers were matched across datasets, and HER2 (ERBB2) amplification status was assigned based on copy-number values (amplified vs non-amplified).
- **Preprocessing:** Gene expression counts were assembled into a samples-by-genes matrix and appropriately rounded/filtered for analysis. Metadata was constructed to map each sample to its HER2 status.
- **Differential Expression Analysis:** I used the DESeq2 package (R/Bioconductor) to normalize the count data and test for differentially expressed genes (DEGs) between HER2-amplified and non-amplified tumors.
- **Visualization:** An EnhancedVolcano plot was generated to display log₂ fold-changes versus adjusted p-values, highlighting significant DEGs. Principal Component Analysis (PCA) was performed (using ggplot2) to examine sample variance and separation by HER2 status. A heatmap (pheatmap) of the top DEGs was created to visualize expression patterns and clustering across samples.
- **Pathway Enrichment:** Enriched biological pathways among the up- and down-regulated DEGs were identified using ReactomePA and clusterProfiler (with human gene annotations). Enrichment results were visualized with dotplots and tree plots (enrichplot) and mapped onto pathway diagrams using Pathview.
- **Survival Analysis:** A LASSO-regularized Cox regression model was built (glmnet) on the variance-stabilized expression of DEGs to create a prognostic signature. Patients were stratified into high-risk and low-risk groups based on the median prognostic score, and Kaplan–Meier survival curves were plotted (survival/survminer) to compare outcomes between the risk groups.

## Results Summary

- **Key Differential Genes:** A total of ~18,600 genes were tested; DESeq2 analysis confirmed strong overexpression of ERBB2 in the HER2-amplified group, as expected. Notably, SPANXA2 (a cancer/testis antigen) was among the most upregulated genes (~20-fold increase). In contrast, genes like CSN2 (casein beta) were highly downregulated in HER2+ tumors, reflecting loss of normal breast epithelial function.
- **Pathway Enrichment:** Upregulated DEGs in HER2-amplified tumors were significantly enriched in cell cycle and DNA replication pathways, consistent with increased proliferation. Downregulated DEGs were enriched in translation and protein synthesis pathways (e.g., peptide elongation, nonsense-mediated decay), indicating reduced protein synthesis activity in the HER2+ group.
- **PCA Findings:** The first two principal components captured about 28% of the total variance (PC1 ~20%, PC2 ~8%). The PCA showed a tendency for HER2-amplified samples to separate from non-amplified ones along PC1 (driven by genes like ERBB2, CDC6), although there was some overlap between groups.
- **Heatmap Clustering:** The expression heatmap of top DEGs revealed distinct clustering by HER2 status. HER2-amplified samples formed one cluster characterized by high expression of known amplicon genes (ERBB2, GRB7, STARD3) and cell-cycle regulators (CDK12, CDC6). Non-amplified samples formed a separate cluster with lower expression of those genes.
- **Survival Stratification:** The LASSO-Cox model stratified patients into “high-risk” and “low-risk” groups with significantly different survival outcomes (log-rank p<0.0001). High-risk patients (with elevated expression of the signature genes) had substantially poorer overall survival, demonstrating the prognostic value of the gene-expression signature.

## Target Audience
This work is intended for researchers in computational biology, cancer genomics, and related fields. It will be most useful to bioinformaticians 
and data scientists with interest in transcriptomics and machine learning (AI/ML) methods, as well as oncologists seeking computational insights 
into HER2-positive breast cancer. The analyses assume familiarity with R programming and genomic data analysis techniques.

__References:__ Full citations and discussion of methods are provided in the report and accompanying bibliography.
