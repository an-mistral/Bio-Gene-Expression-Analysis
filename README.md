# Gene Expression Analysis

## Project Overview
Breast cancer is a heterogeneous disease comprising multiple molecular subtypes. One aggressive subtype is HER2-positive cancer, characterized by 
amplification of the human epidermal growth factor receptor 2 (HER2/ERBB2) oncogene and accounting for roughly 11–30% of cases. This project compares RNA-seq profiles of HER2-amplified 
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
Three data modalities were used:
- RNA-seq–derived gene-level mRNA expression (RSEM-normalized estimates) for breast tumor samples.
- Copy-number alteration (CNA) data to identify HER2 (ERBB2) amplification status.
- Clinical data: patient identifiers and overall survival fields (survival status and time-to-event in months).

## Analysis Workflow
The analysis was conducted in R (v4.4.2), using established Bioconductor and CRAN packages. 
The pipeline included the following steps:
1. **Data Acquisition & Preprocessing:** RNA-seq expression, CNA and clinical data for TCGA Breast Invasive Carcinoma were downloaded via cBioPortal. Patient/sample identifiers were standardized to a common TCGA barcode format and intersected across modalities to ensure consistent cohort alignment. HER2 (ERBB2) amplification status was assigned based on copy-number alterations values. Gene expression counts were assembled into a samples-by-genes matrix and rounded/filtered for analysis. Metadata was constructed to map each sample to its HER2 status.
2. **Expression preprocessing:** Gene-level RNA-seq expression was converted to a DESeq2-compatible integer matrix by rounding. Negative values (if present) were set to zero. Lowly expressed genes were filtered to reduce noise and improve statistical power.
3. **Differential Expression Analysis:** Differential expression testing was performed with DESeq2, including library-size normalization and Wald testing. Significance was assessed using Benjamini–Hochberg adjusted p-values. Results were summarized with:
    - **Volcano plot** (EnhancedVolcano) showing log₂ fold-change vs adjusted p-value.
    - **PCA** (ggplot2) on variance-stabilized expression to visualize global separation by HER2 status.
    - **Heatmap** (pheatmap) of top DEGs to highlight expression patterns and sample clustering.
4. **Pathway Enrichment:** Enriched biological pathways among the up- and down-regulated DEGs were identified using ReactomePA and clusterProfiler (with human gene annotations). Enrichment results were visualized with dotplots and tree plots (enrichplot) and mapped onto pathway diagrams using Pathview.
5. **Survival Analysis:** A Lasso-regularized Cox regression model was built (glmnet) on the variance-stabilized expression of DEGs to create a prognostic signature. Patients were stratified into high-risk and low-risk groups based on the median prognostic score, and Kaplan–Meier survival curves were plotted (survival/survminer) to compare outcomes between the risk groups.

## Results Summary
- **Key DEGs:** A total of ~18,600 genes were tested; DESeq2 analysis confirmed strong overexpression of ERBB2 (involved in cell growth and differentiation) in the HER2-amplified group, as expected. Among the most upregulated genes, SPANXA2 showed one of the largest increases (~20×); SPANX-family genes are described as sperm homolog proteins linked to reproduction and cell division, consistent with a proliferative program when aberrantly expressed. Several
GAGE family genes (including GAGE12D) were also strongly upregulated (~8–20×); GAGE genes are described as cancer-associated and linked to tumour adaptation and immune evasion. In contrast, mammary differentiation markers were markedly reduced: CSN2 (~21×; involved in lactation and epithelial function), CSN3 (~13×; supports structural stability of milk proteins**), and LALBA (~10×; regulates lactose production). Their coordinated downregulation is consistent with suppression of normal mammary/epithelial programs in aggressive HER2-amplified tumours.
- **Pathway Enrichment:** Upregulated DEGs in HER2-amplified tumors were significantly enriched in cell cycle and DNA replication pathways, consistent with increased proliferation. Downregulated DEGs were enriched in translation and mRNA surveillance pathways (e.g., peptide elongation, nonsense-mediated decay), indicating reduced protein synthesis activity and quality control in the HER2+ group.
- **PCA Findings:** The first two principal components captured about 28% of the total variance (PC1 ~20%, PC2 ~8%). The components explain a significant part of the variation in the data, reflecting molecular differ- ences between HER2-amplified and non-amplified tumours, which confirms the role of HER2 in the formation of the tumour phenotype. However, it also highlights the complexity of the data and the influence of additional biological factors.
- **Heatmap Clustering:** The expression heatmap of top DEGs revealed distinct clustering by HER2 status. HER2-amplified samples formed one cluster characterized by high expression of known amplicon genes (ERBB2, GRB7, STARD3) and cell-cycle regulators (CDK12, CDC6). Non-amplified samples formed a separate cluster with lower expression of those genes.
- **Survival Stratification:** 
Survival analysis demonstrated a correlation between gene expression profiles and clinical outcomes of patients (log-rank p<0.0001). Cox regression model with Lasso regularization identified gene sets associated with high- and low-risk groups, highlighting the prognostic value of molecular markers and the need for targeted therapy and personalized treatment strategies.

## Target Audience
This work is intended for researchers in computational biology, cancer genomics, and related fields. It will be most useful to bioinformaticians 
and data scientists with interest in transcriptomics and machine learning (AI/ML) methods, as well as oncologists seeking computational insights 
into HER2-positive breast cancer. The analyses assume familiarity with R programming and genomic data analysis techniques.

__References:__ Full citations and discussion of methods are provided in the report and accompanying bibliography.
