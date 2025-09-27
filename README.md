# rna-seq-learning
RNA-seq analysis pipeline on Google Colab and R

## Overview
This repository contains hands-on training pipelines for RNA-seq differential expression analysis. 
The workflow is designed for a lightweight dataset, focusing on  learning the basic steps from preprocessing to DEG analysis. 

## Pipeline
1. **Quality control**: FastQC  
2. **Adapter trimming**: Cutadapt, fastp, SortMeRNA  
3. **Alignment**: HISAT2  
4. **Counting**: featureCounts  
5. **Differential expression**: DESeq2 (R / RMarkdown)

## Repository Structure
```
rna-seq-learning/
│── RNAseq_01/ # Colab notebook for first practice run
│── RNAseq_02/ # Alternative pipeline variations
│── RNAseq_03/ # Additional test cases
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