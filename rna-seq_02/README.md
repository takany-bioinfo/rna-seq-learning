## はじめに

このNotebookは、Google Colab上でNGS解析を行うためのものです。公開されているデータセットを使用していますが、元論文の著者や研究内容とは一切関係ありません。この解析は学習目的で独自に行ったものであり、元の研究成果を引用・再現・評価するものではありません。

---
  
## 処理内容について

このNotebookでは、**Arabidopsis thaliana** の **single-end RNA-Seq** 公開データを用いて、DGE解析（差次的遺伝子発現解析）に向けた前処理を行います。
前回の Notebook **rna-seq_01** では、各処理ごとに結果を確認しながら進めるスタイルでしたが、このNotebook **rna-seq_02** では、%%bash セルを用い、パイプによるストリーミング処理で一連の処理をリードファイルごとに連続して実行するスタイルになっています。
なお、rna-seq_01 の実行によって作成された **HISAT2用インデックスファイル**を使用します。



###  Google Driveフォルダの前提ファイル

以下のファイルがGoogle Driveの指定フォルダに保存されていることを前提としています。

- `accession_list.txt`  
- `araport11.gtf`  
- `at_index1.ht2 ～ at_index8.ht2`

###  参照ファイルについて
   
- `accession_list.txt` : 解析するRNAseqデータのアクセッションIDを記入したファイル  
使用する15件のアクセションIDを記入してあります。

- `araport11.gtf` : Arabidopsis thaliana の遺伝子アノテーションファイル  
  元データ: https://www.arabidopsis.org/download/file?path=Genes/Araport11_genome_release/Araport11_GTF_genes_transposons.20241001.gtf.gz  
※ 解凍後、ファイル名を `araport11.gtf` にリネームしてください。

- `at_index*.ht2` : HISAT2のインデックスファイル  
ファイルは `at_index1.ht2` から `at_index8.ht2` まで8個あります。  
これらは rna-seq_01 の実行時に作成されたものを流用します。

<br>

## 処理対象データについて
RNA-Seq データは、Evorepro database より取得した **Arabidopsis thaliana の花粉発達段階ごとの公開データ**を使用しています。  
以下の15 個のリードファイルを対象としています（全てSingle-end 75bp）：
<div style="display: flex; gap: 1rem;">
<div>
<table>
<thead>
<tr><th>Accession ID</th><th>発達ステージ</th></tr>
</thead>
<tbody>
<tr><td>ERR4471617</td><td>bicellular</td></tr>
<tr><td>ERR4471621</td><td>bicellular</td></tr>
<tr><td>ERR4471625</td><td>bicellular</td></tr>
<tr><td>ERR4471641</td><td>late bicellular</td></tr>
<tr><td>ERR4471645</td><td>late bicellular</td></tr>
<tr><td>ERR4471649</td><td>late bicellular</td></tr>
<tr><td>ERR4471654</td><td>mature</td></tr>
<tr><td>ERR4471658</td><td>mature</td></tr>
</tbody>
</table>
</div>
<div>
<table>
<thead>
<tr><th>Accession ID</th><th>発達ステージ</th></tr>
</thead>
<tbody>
<tr><td>ERR4471662</td><td>mature</td></tr>
<tr><td>ERR4471680</td><td>tricellular</td></tr>
<tr><td>ERR4471684</td><td>tricellular</td></tr>
<tr><td>ERR4471688</td><td>tricellular</td></tr>
<tr><td>ERR4471704</td><td>uninucleate</td></tr>
<tr><td>ERR4471708</td><td>uninucleate</td></tr>
<tr><td>ERR4471712</td><td>uninucleate</td></tr>
<tr><td> ー </td><td> ー</td></tr>
</tbody>
</table>
</div>
</div>

---

- DB: EEVOREPRO: RNA- seq of Arabidopsis thaliana pollen developmental stages for Ler and Col ecotypes
- Library Strategy: RNA-Seq  
- Library Layout: SINGLE  
- Read Length: 75bp  
- Read Count: approximately 6 million reads  
- Library Prep: modified SmartSeq2 → Nextera low-input  
- Instrument: Illumina NextSeq 500  
- Selection: PolyA
---
このNotebookは個人の学習目的で作成したものであり、内容の正確性・再現性を保証するものではありません。また、外部データやツールについては、それぞれのライセンスに従ってご利用ください。

※なお、このNotebookで生成した `count.txt` を用い、Rにて差次的遺伝子発現解析を行ったR Markdownの出力（HTMLファイル）は以下よりご覧いただけます：

🔗 [DGE解析例を見る](https://takany-bioinfo.github.io/rna-seq-learning/dge_02.html)