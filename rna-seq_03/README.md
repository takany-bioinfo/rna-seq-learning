
🧷 [English version here](https://takany-bioinfo.github.io/rna-seq-learning/rna-seq_03/README.en.md)
## はじめに  

　このノートブックは、Google Colab上でNGS解析を行うためのものです。 Colab無料版の制限（ストレージ容量やセッションの自動切断など）を考慮した処理内容です。  
　なお、公開されているデータセットを使用していますが、元論文の著者や研究内容とは一切関係ありません。この解析は学習目的で独自に行ったものであり、元の研究成果を引用・再現・評価するものではありません。

---
  
  <br>

## 処理内容について

このノートブックは、*Arabidopsis thaliana* の**ペアエンド RNA-Seq** 公開データを用いて、**遺伝子発現の差分解析（**DGE解析）のための前処理**を行います。  

このパイプラインでは、**RNA-SeqデータをNCBI SRAから取得**し、一部パイプ入力による**ストリーミング処理**を通じて、各サンプルごとにBAMファイルの生成までを実行します。その後、生成されたBAMファイルをまとめて、featureCountsでカウントを行います。**fastpで自動トリミング**を行い、**HISAT2でマッピング**を実行します。ストレージ使用量を抑えるため中間ファイルは保存していません。**実行ログをGoogle Driveに保存**しています。すべての処理を、**ひとつの %%bash セル内**で実行しています。


###  Google Driveフォルダのファイル
以下のファイルがGoogle Driveの指定フォルダに保存されていることを前提としています：

- `accession_list.txt`  
- `araport11.gtf`   
- `at_index1.ht2` ～ `at_index8.ht2` 

###  参照ファイルについて
   
- `accession_list.txt` : 解析するRNAseqデータのアクセッションIDを記入したファイル  
使用する9件のアクセションIDを記入してあります

- `araport11.gtf` : Arabidopsis thaliana の遺伝子アノテーションファイル  
  元データ: https://www.arabidopsis.org/download/file?path=Genes/Araport11_genome_release/Araport11_GTF_genes_transposons.20241001.gtf.gz  
※ 解凍後、ファイル名を `araport11.gtf` にリネームしてください。
- `at_index*.ht2` : HISAT2用の Arabidopsis thaliana のゲノムインデックスファイル  <br>
ファイルは `at_index1.ht2` から `at_index8.ht2` まで8個あります。  
これらは **rna-seq_01** の実行時に作成されたものを流用します。  

<br>

## 処理対象データについて
RNA-Seqデータは、NCBIのSequence Read Archive（SRA）に公開されている、***Arabidopsis thaliana***の**花粉発達段階**に関するデータです。  
以下の9個のサンプルを対象としています（全て**Paired-end** ）

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
このノートブックは個人の学習目的で作成したものであり、内容の正確性・再現性を保証するものではありません。また、外部データやツールについては、それぞれのライセンスに従ってご利用ください。