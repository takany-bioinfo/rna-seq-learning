# rna-seq-learning
RNA-seq analysis pipeline on Google Colab and R 

## Overview
This repository contains hands-on training pipelines for RNA-seq differential expression analysis using ***Arabidopsis thaliana*** pollen development data.
The workflow is designed for a lightweight dataset, focusing on learning the basic steps from preprocessing to DGE analysis. 

## Pipeline
1. **Quality control**: FastQC  
2. **Adapter trimming**: Cutadapt, fastp, SortMeRNA  
3. **Alignment**: HISAT2  
4. **Counting**: featureCounts  
5. **Differential expression**: DESeq2 (R / RMarkdown)

## Repository Structure
```
rna-seq-learning/
│── rna-seq_01/   # Step-by-step notebook for the first practice run
│── rna-seq_02/   # Streaming notebook (fastp) and Rmarkdown for DGE (DESeq2)
│── rna-seq_02_c/ # Streaming notebook (cutadapt + sortMeRNA)
│── rna-seq_03    # Notebook for paired-end reads 
│── docs/ # Rendered RMarkdown (.html) results
└── README.md
```
## Usage
- Open the Jupyter/Colab notebooks directly from GitHub.  
- For RMarkdown (`.Rmd`), copy or download the file and run it in RStudio.  
- Input data: Reference files (`.fa`, `.gtf`, sortMeRNA database) are **not included**; please prepare your own in the specified paths.

## Notes
- The example data used here is simplified for training purposes.  
- Colab environment requires mounting Google Drive to handle reference and output files.  
- See individual subfolders for more detailed instructions.