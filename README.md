# Reanalyzing Pediatric AML Single-Cell Data Using Machine Learning

Understanding the cellular landscape of pediatric Acute Myeloid Leukemia (AML) during treatment is crucial for improving therapeutic strategies. In this project, I reanalyzed publicly available single-cell RNA sequencing (scRNA-seq) data from [GSE235063](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE235063), recreating the cellular annotations described in *A Longitudinal Single-Cell Atlas of Treatment Response in Pediatric AML* using a different computational pipeline. Below, I outline my approach, from data preprocessing to differential expression analysis.

<p align="center">
    <img src="https://github.com/user-attachments/assets/0eead41d-6918-403d-97c4-f30e7014269a" alt="Cell Type" width="400">
</p>


## Data Preprocessing and Quality Control

To ensure high-quality data, I implemented several preprocessing steps:

1. **Ambient RNA Removal:** I used [CellBender](https://github.com/broadinstitute/CellBender) to eliminate ambient RNA contamination.
2. **Quality Control Metrics:** Cells were filtered based on:
   - The number of counts per barcode (count depth)
   - The number of genes per barcode
   - The fraction of counts from mitochondrial genes per barcode
3. **Doublet Removal:** I applied [DoubletDetection](https://github.com/JonathanShor/DoubletDetection) to remove potential doublets and improve cell classification accuracy.

## Cell Type Annotation Using Machine Learning

I trained three machine learning models using [CellTypist](https://github.com/Teichlab/celltypist) to predict cell types. These models were trained on the following datasets:

- [GSE139369](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE139369): A mixed phenotype dataset
- [GSE116256](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE116256): A dataset on AML hierarchies relevant to disease progression and immunity

Using **single-cell variational inference (SCVI)** tools, I integrated the predicted cell type labels and visualized the data using **Uniform Manifold Approximation and Projection (UMAP)**. I refined my cell annotations using both model predictions and well-established genetic markers, leading to the identification of key AML cell types, including AML blasts.

## Differential Gene Expression Analysis

To gain insights into transcriptional changes across different treatment time points, I performed **pseudobulk differential expression analysis**, following best practices outlined by [TheisLab](https://www.sc-best-practices.org/preamble.html). Below are the top ten differentially expressed genes by P-Value (95% Confidence) along with volcano plots of the AML Blast Cell Type, where genes past the significance threshold are in blue, and the top 10 genes are labeled. 

<p style="center;">
    <img src="https://github.com/user-attachments/assets/86565a2e-c49a-471c-ae89-b5636d7fdc2c" alt="AML Blast Volcano Plot 1" width="400">
    <img src="https://github.com/user-attachments/assets/718229e3-a95a-4953-9195-df35afdfddde" alt="AML Blast Volcano Plot 2" width="400">
</p>


| Gene    | logFC  | PValue       | -logQ  | Cell Type            | Condition          |
|---------|--------|-------------|--------|----------------------|-------------------|
| CRIP1   | -1.57  | 1.21e-19     | 18.92  | AML_Blast            | REL vs DX        |
| S100A10 | -2.14  | 1.47e-17     | 16.83  | AML_Blast            | REL vs DX        |
| CAPN2   | -1.32  | 1.04e-16     | 15.98  | AML_Blast            | REL vs DX        |
| SAMHD1  | -1.39  | 9.84e-14     | 13.01  | AML_Blast            | REL vs DX        |
| CAST    | -1.11  | 2.16e-13     | 12.67  | AML_Blast            | REL vs DX        |
| SOCS3   |  1.79  | 1.02e-12     | 11.99  | AML_Blast            | REL vs DX        |
| NCF1    | -1.60  | 4.17e-12     | 11.38  | AML_Blast            | REL vs DX        |
| LGALS3  | -1.64  | 5.26e-11     | 10.28  | AML_Blast            | REL vs DX        |
| MARCH1  | -1.32  | 9.59e-11     | 10.02  | AML_Blast            | REL vs DX        |
| LILRB4  | -1.21  | 1.42e-10     | 9.85   | AML_Blast            | REL vs DX        |
| JCHAIN  |  5.21  | 2.43e-47     | 46.61  | AML_Blast            | REM vs DX        |

## Themes in Differential Expression Between Relapse and Diagnosis

The differential expression analysis reveals significant changes in gene expression between relapse (REL) and diagnosis (DX), suggesting shifts in cellular behavior and potential mechanisms of AML progression. 

- Several genes downregulated at relapse, such as **CRIP1, S100A10, and CAPN2**, have been implicated in cytoskeletal organization, cell adhesion, and tumor suppression. Their reduced expression may reflect a shift toward a more aggressive, invasive phenotype.
- **SOCS3**, which is upregulated in REL vs DX, is a known regulator of cytokine signaling and immune modulation. This suggests that AML blasts at relapse may exhibit altered interactions with the immune system, potentially contributing to therapy resistance.
- **JCHAIN**, significantly upregulated in remission (REM vs DX), is associated with B-cell development and immunoglobulin production. This could indicate immune reconstitution during remission and a shift in the tumor microenvironment away from leukemic dominance.
- The presence of genes related to oxidative stress and inflammation, such as **NCF1** and **LGALS3**, suggests that AML progression may involve increased stress response pathways that allow leukemic cells to survive under treatment pressure.

These findings provide valuable insights into how AML blasts evolve in response to treatment and may highlight potential therapeutic targets for improving patient outcomes.

## Conclusion

This analysis provides means of studying pediatric AML single-cell data with machine learning approaches. By leveraging multiple scRNA-seq datasets and state-of-the-art computational tools, I successfully reannotated cell types, visualized high-dimensional data, and identified some differential expression patterns. While this was done to help practice and build my skills it may be interesting to see these tools in more research work in this area!
