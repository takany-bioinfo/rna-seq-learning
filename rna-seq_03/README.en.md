## Introduction  

This notebook is designed for performing NGS analysis using Google Colab. It considers the limitations of the free Colab environment, such as limited storage capacity and potential session disconnections from sessions.

Please note that while public datasets are used in this notebook, they are not directly related to the original studies or authors.  
The analyses presented here are intended solely for private learning purposes and are not meant for citation, reproducibility, or evaluation of any original research.


🧷 [日本語版はこちら](https://takany-bioinfo.github.io/rna-seq-learning/rna-seq_03/README.md)
---
  <br>

## About this notebook
This notebook preprocesses **Arabidopsis thaliana paired-end RNA-Seq public data** for subsequent differential gene expression **(DGE) analysis**.  
<br>
###  Google Drive files
The following files are expected to be located in a specific folder on Google Drive :

- `accession_list.txt`  
- `araport11.gtf`   
- `at_index1.ht2` ～ `at_index8.ht2` 

###  付属の参照ファイルについて
### Reference files

- `accession_list.txt` :
This file contains the accession IDs of the RNA-Seq data to be analyzed.
A total of 9 accession IDs are listed.

- `araport11.gtf` :
Arabidopsis thaliana gene annotation file.
Download from:
https://www.arabidopsis.org/download/file?path=Genes/Araport11_genome_release/Araport11_GTF_genes_transposons.20241001.gtf.gz

※ After extraction, please rename the file to araport11.gtf.

- `at_index*.ht2` :
HISAT2 genome index files for *Arabidopsis thaliana*, built with splice site and exon annotations. <br>
There are 8 files, named from at_index.1.ht2 to at_index.8.ht2.
This index set can be reused from the output of the rna-seq_01 run.

<br>

## 処理対象データについて
RNA-Seq データは、Evorepro database より取得した **Arabidopsis thaliana の花粉発達段階の公開データ**を使用しています。  
以下の9個のサンプルを対象としています（全てPaired-end ）

| Accession ID | 発達段階        |
|--------------|-----------------|
|SRR23387025  |bicellular       |
|SRR23387024  |bicellular|
|SRR23387026  |bicellular|
|SRR23387021  |tricellular|
|SRR23387054  |tricellular|
|SRR23387022  |tricellular|
|SRR23387055  |uninucleate|
|SRR23387056  |uninucleate|
|SRR23387044  |uninucleate |
|

- DB:NCBI: RNA-Seq analysis of synchronised developing pollen isolated from a single anther
- Library Strategy: RNA-Seq  
- Library Layout: paired-end 
- Read Length: 150bp  
- Read Count: 約22～54 million reads  
- Library Prep: oligo-dT primer with Template Switching Oligo→Nextera XT,   mRNA enriched using beads   
- Instrument: Illumina HiSeq2500  
- Selection: PolyA
---
このNotebookは個人の学習目的で作成したものであり、内容の正確性・再現性を保証するものではありません。また、外部データやツールについては、それぞれのライセンスに従ってご利用ください。
