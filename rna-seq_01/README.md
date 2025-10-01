## はじめに

このNotebookは、Google Colab上でNGS解析を行うためのものです。  
Colab無料版の制限（セッションの自動切断など）を考慮し、また1セルずつ順に実行・確認しながら進める構成になっています。  
なお、このNotebookでは公開されているデータセットを使用していますが、元論文の著者や研究内容とは一切関係ありません。この解析は学習目的で独自に行ったものであり、元の研究成果を引用・再現・評価するものではありません。

---

### 🔧 事前準備

- Googleアカウントにログインする  
- ブラウザで Colab を開く： [https://colab.research.google.com/](https://colab.research.google.com/)
- ブラウザの固定幅フォントを英字用フォント（例：Courier New、Consolas など）に設定する  
  - Chromeの場合：「設定」→「デザイン」→「フォントをカスタマイズ」から変更できます
  - これにより `¥`（円マーク）が `\`（バックスラッシュ）として表示されるようになります
  
  <br>
## 処理内容について

このNotebookは、**Arabidopsis thaliana** の**single-end RNA-Seq**公開データを用いて、**DGE解析**に向けたデータの前処理を行います。



###  Google Driveフォルダの前提ファイル

以下のファイルがGoogle Driveの指定フォルダに保存されていることを前提としています：

- `accession_list.txt`  
- `araport11.gtf`  
- `TAIR10_chr_all.fas`  
- `smr_v4.3_default_db.fasta`  
- `params1.txt` ～ `params4.txt`



###  参照ファイルについて
リポジトリに付属しているファイルの元データの入手先はそれぞれ以下の通りです。


   
- `accession_list.txt` : 解析するRNAseqデータのアクセッションIDを記入したファイル  
使用する４件のアクセションIDを記入してあります

- `araport11.gtf` : Arabidopsis thaliana の遺伝子アノテーションファイル  
  元データ: https://www.arabidopsis.org/download/file?path=Genes/Araport11_genome_release/Araport11_GTF_genes_transposons.20241001.gtf.gz  
※ 解凍後、ファイル名を `araport11.gtf` にリネームしてください。

- `TAIR10_chr_all.fas` : Arabidopsis thaliana のゲノム配列ファイル  
  元データ: https://www.arabidopsis.org/download/file?path=Genes/TAIR10_genome_release/TAIR10_chromosome_files/TAIR10_chr_all.fas.gz

- `smr_v4.3_default_db.fasta` : sortMeRNA用rRNAデータベース  
  元データ: https://github.com/biocore/sortmerna/releases/download/v4.3.4/database.tar.gz

- `params1.txt` ～ `params4.txt` : cutadapt用オプション設定ファイル（任意で作成）  
<sub>※このノートブックでは、cutadaptのオプションを外部ファイルとして定義し、読み込む形式をとっています。アダプター配列などのトリミング条件を検討して作成、またはコマンド内で直接指定してください。  </sub>

<br>

## 処理対象データについて
RNA-Seq データは、Evorepro database より取得した **Arabidopsis thaliana の花粉発達段階の公開データ**を使用しています。  
以下の4つのサンプルを対象としています（全てSingle-end 75bp）：

| Accession ID | 発達ステージ     |
|--------------|-----------------|
| ERR4471712   | uninucleate     |
| ERR4471708   | uninucleate     |
| ERR4471680   | tricellular     |
| ERR4471684   | tricellular     |
|
- 出典: EEVOREPRO: RNA- seq of Arabidopsis thaliana pollen developmental stages for Ler and Col ecotypes
- Library Strategy: RNA-Seq  
- Library Layout: SINGLE  
- Read Length: 75bp  
- Read Count: 約5–6.5 million reads  
- Library Prep: modified SmartSeq2 → Nextera low-input  
- Instrument: Illumina NextSeq 500  
- Selection: PolyA
---
このNotebookは個人の学習目的で作成したものであり、内容の正確性・再現性を保証するものではありません。また、外部データやツールについては、それぞれのライセンスに従ってご利用ください。

なお、このNotebookで生成した `count.txt` を用い、Rにて差次的遺伝子発現解析（DGE解析）を行ったR Markdownの出力（HTMLファイル）は以下よりご覧いただけます：

🔗 [DGE解析例を見る](https://takany-bioinfo.github.io/rna-seq-learning/dge_01.html)
