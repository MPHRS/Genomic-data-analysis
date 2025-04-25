# Data install 

```{Bash}
wget http://ftp.sra.ebi.ac.uk/vol1/fastq/SRR941/SRR941816/SRR941816.fastq.gz

wget http://ftp.sra.ebi.ac.uk/vol1/fastq/SRR941/SRR941817/SRR941817.fastq.gz

wget http://ftp.sra.ebi.ac.uk/vol1/fastq/SRR941/SRR941818/SRR941818.fastq.gz

wget http://ftp.sra.ebi.ac.uk/vol1/fastq/SRR941/SRR941819/SRR941819.fastq.gz

wget http://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/146/045/GCF_000146045.2_R64/GCF_000146045.2_R64_genomic.fna.gz

wget http://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/146/045/GCF_000146045.2_R64/GCF_000146045.2_R64_genomic.gff.gz
```

and unzip all

```
gunzip *.gz
```
# **a). Aligning with HISAT2**

	index

```{Bash}
hisat2-build GCF_000146045.2_R64_genomic.fna yeast_index
```

	align

```{bash}
# Для SRR941816 (0 минут, репликат 1)
hisat2 -x yeast_index -U SRR941816.fastq | samtools sort -o SRR941816.bam

# Для SRR941817 (0 минут, репликат 2)
hisat2 -x yeast_index -U SRR941817.fastq | samtools sort -o SRR941817.bam

# Для SRR941818 (30 минут, репликат 1)
hisat2 -x yeast_index -U SRR941818.fastq | samtools sort -o SRR941818.bam

# Для SRR941819 (30 минут, репликат 2)
hisat2 -p 4 -x yeast_index -U SRR941819.fastq | samtools sort -o SRR941819.bam
```

	output 
```
hisat2 -x yeast_index -U SRR941816.fastq | samtools so
rt -o SRR941816.bam

---

9043877 reads; of these:
  9043877 (100.00%) were unpaired; of these:
    512971 (5.67%) aligned 0 times
    7930603 (87.69%) aligned exactly 1 time
    600303 (6.64%) aligned >1 times
94.33% overall alignment rate
[bam_sort_core] merging from 2 files and 1 in-memory blocks...
```


```
ls
total 5394084
-rw-r--r-- 1 mhprs mhprs   12310392 Apr 25 08:44 GCF_000146045.2_R64_genomic.fna
-rw-r--r-- 1 mhprs mhprs   12390288 Apr 25 08:44 GCF_000146045.2_R64_genomic.gff
-rw-r--r-- 1 mhprs mhprs  324591181 Apr 25 09:06 SRR941816.bam
-rw-r--r-- 1 mhprs mhprs 1513637479 Apr 25 08:44 SRR941816.fastq
-rw-r--r-- 1 mhprs mhprs  359408069 Apr 25 09:10 SRR941817.bam
-rw-r--r-- 1 mhprs mhprs 1661982311 Apr 25 08:44 SRR941817.fastq
-rw-r--r-- 1 mhprs mhprs   68442403 Apr 25 09:10 SRR941818.bam
-rw-r--r-- 1 mhprs mhprs  287249121 Apr 25 08:44 SRR941818.fastq
-rw-r--r-- 1 mhprs mhprs  227941127 Apr 25 09:12 SRR941819.bam
-rw-r--r-- 1 mhprs mhprs 1032705059 Apr 25 08:44 SRR941819.fastq
-rw-r--r-- 1 mhprs mhprs    8248454 Apr 25 09:02 yeast_index.1.ht2
-rw-r--r-- 1 mhprs mhprs    3039284 Apr 25 09:02 yeast_index.2.ht2
-rw-r--r-- 1 mhprs mhprs        161 Apr 25 09:02 yeast_index.3.ht2
-rw-r--r-- 1 mhprs mhprs    3039277 Apr 25 09:02 yeast_index.4.ht2
-rw-r--r-- 1 mhprs mhprs    5399069 Apr 25 09:02 yeast_index.5.ht2
-rw-r--r-- 1 mhprs mhprs    3092708 Apr 25 09:02 yeast_index.6.ht2
-rw-r--r-- 1 mhprs mhprs         12 Apr 25 09:02 yeast_index.7.ht2
-rw-r--r-- 1 mhprs mhprs          8 Apr 25 09:02 yeast_index.8.ht2

```
# **b) Quantifying with featureCounts**

```
conda install gffread
```
we need to convert GFF to GTF

```{Bash}
gffread GCF_000146045.2_R64_genomic.gff -T -o GCF_000146045.2_R64_genomic.gtf
```

```{Bash}
mamba install subread
```

```
featureCounts -g gene_id -a GCF_000146045.2_R64_genomic.gtf -o counts.txt SRR941816.bam SRR941817.bam SRR941818.bam SRR941819.bam
```


but have issue

```


        ==========     _____ _    _ ____  _____  ______          _____
        =====         / ____| |  | |  _ \|  __ \|  ____|   /\   |  __ \
          =====      | (___ | |  | | |_) | |__) | |__     /  \  | |  | |
            ====      \___ \| |  | |  _ <|  _  /|  __|   / /\ \ | |  | |
              ====    ____) | |__| | |_) | | \ \| |____ / ____ \| |__| |
        ==========   |_____/ \____/|____/|_|  \_\______/_/    \_\_____/
          v2.0.8

//========================== featureCounts setting ===========================\\
||                                                                            ||
||             Input files : 4 BAM files                                      ||
||                                                                            ||
||                           SRR941816.bam                                    ||
||                           SRR941817.bam                                    ||
||                           SRR941818.bam                                    ||
||                           SRR941819.bam                                    ||
||                                                                            ||
||             Output file : counts.txt                                       ||
||                 Summary : counts.txt.summary                               ||
||              Paired-end : no                                               ||
||        Count read pairs : no                                               ||
||              Annotation : GCF_000146045.2_R64_genomic.gtf (GTF)            ||
||      Dir for temp files : ./                                               ||
||                                                                            ||
||                 Threads : 1                                                ||
||                   Level : meta-feature level                               ||
||      Multimapping reads : not counted                                      ||
|| Multi-overlapping reads : not counted                                      ||
||   Min overlapping bases : 1                                                ||
||                                                                            ||
\\============================================================================//

//================================= Running ==================================\\
||                                                                            ||
|| Load annotation file GCF_000146045.2_R64_genomic.gtf ...                   ||

ERROR: failed to find the gene identifier attribute in the 9th column of the provided GTF file.
The specified gene identifier attribute is 'gene_id'
An example of attributes included in your GTF annotation is 'transcript_id "gene-Q0158"; gene_name "21S_RRNA";'.
```

two rows have no "gene_id", so it failed - here is code to fix it : https://colab.research.google.com/drive/1yjUwu6Xey68g-GfQKJA9SoBX6RwVZk_r?usp=sharing


```
(GP_6) mhprs@Seraph:~/PISH/Genomics/project_6_rna/data/processed$ cat counts.txt.summary
Status  SRR941816.bam   SRR941817.bam   SRR941818.bam   SRR941819.bam
Assigned        7305701 8016368 1404579 4983580
Unassigned_Unmapped     512971  505207  65072   229677
Unassigned_Read_Type    0       0       0       0
Unassigned_Singleton    0       0       0       0
Unassigned_MappingQuality       0       0       0       0
Unassigned_Chimera      0       0       0       0
Unassigned_FragmentLength       0       0       0       0
Unassigned_Duplicate    0       0       0       0
Unassigned_MultiMapping 1330252 1682079 312460  1202450
Unassigned_Secondary    0       0       0       0
Unassigned_NonSplit     0       0       0       0
Unassigned_NoFeatures   587358  591324  99101   368759
Unassigned_Overlapping_Length   0       0       0       0
Unassigned_Ambiguity    37544   37710   4329    15803
```




# **c) Find differentially expressed genes with Deseq2**


take only needed columns

```
cat counts.txt | cut -f 1,7-10 > simple_counts.txt
```


	note that we have 17 uniq "chr" in output, so  NC_001224.1 - mitochonrial chromosome 

```
mamba install r-base
```

```
R # to start r 

if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("DESeq2")

library(DESeq2)  # Должно заработать без ошибок
q()  # Выход
```

didn't work 

```
mamba install -c bioconda bioconductor-deseq2
mamba install -c conda-forge r-gplots
```

```
cat simple_counts.txt | R -f ../scripts/deseq2.r
cat norm-matrix-deseq2.txt | R -f ../scripts/draw-heatmap.r
```

select only 50 top genes 

```
head -n 50 result.txt | cut -f 1 | cut -d "-" -f 2 > genes.txt
```


	(GP_6) mhprs@Seraph:~/PISH/Genomics/project_6_rna/data/processed$ head genes.txt
		YER062C
		YDR536W
		YHR094C
		YNL065W
		YKL120W
		YJL122W
		YLR264W
		YGR159C
```
sed -i '/^$/d' genes.txt  # to remove all empty rows
```

go to 

http://www.yeastgenome.org/cgi-bin/GO/goSlimMapper.pl


and upload "genes.txt" file


and get the table 

| GO Term (GO ID)                                                     | Genes Annotated to the GO Term                                                                    | GO Term Usage in Gene List | Genome Frequency of Use             |
| ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | -------------------------- | ----------------------------------- |
| rRNA processing (GO:0006364)                                        | YDR449C, YEL026W, YGR159C, YHR066W, YHR196W, YJL069C, YMR093W, YNL112W, YNL182C, YOL041C, YOL080C | 11 of 48 genes (22.92%)    | 329 of 6489 annotated genes (5.07%) |
| ribosomal large subunit biogenesis (GO:0042273)                     | YCR072C, YDL063C, YHR066W, YIR012W, YJL122W, YNL182C, YOL041C, YOL080C                            | 8 of 48 genes (16.67%)     | 126 of 6489 annotated genes (1.94%) |
| transcription by RNA polymerase I (GO:0006360)                      | YHR196W, YJL148W, YJR063W, YML043C, YMR093W, YNL248C                                              | 6 of 48 genes (12.50%)     | 69 of 6489 annotated genes (1.06%)  |
| ribosomal small subunit biogenesis (GO:0042274)                     | YDR449C, YEL026W, YGR159C, YHR196W, YJL069C, YMR093W                                              | 6 of 48 genes (12.50%)     | 135 of 6489 annotated genes (2.08%) |
| ribosome assembly (GO:0042255)                                      | YCR072C, YGR159C, YHR066W, YIR012W, YNL182C, YOL080C                                              | 6 of 48 genes (12.50%)     | 77 of 6489 annotated genes (1.19%)  |
| transmembrane transport (GO:0055085)                                | YDR536W, YHR094C, YKL120W, YNL065W                                                                | 4 of 48 genes (8.33%)      | 278 of 6489 annotated genes (4.28%) |
| nucleobase-containing small molecule metabolic process (GO:0055086) | YBL039C, YMR300C, YNL141W, YOL136C                                                                | 4 of 48 genes (8.33%)      | 178 of 6489 annotated genes (2.74%) |
| carbohydrate metabolic process (GO:0005975)                         | YBR105C, YER062C, YKR097W, YOL136C                                                                | 4 of 48 genes (8.33%)      | 160 of 6489 annotated genes (2.47%) |
| RNA catabolic process (GO:0006401)                                  | YLR264W, YNL112W, YOR359W                                                                         | 3 of 48 genes (6.25%)      | 162 of 6489 annotated genes (2.50%) |
| DNA-templated transcription elongation (GO:0006354)                 | YJL148W, YNL248C                                                                                  | 2 of 48 genes (4.17%)      | 105 of 6489 annotated genes (1.62%) |
| tRNA processing (GO:0008033)                                        | YOL124C, YPL212C                                                                                  | 2 of 48 genes (4.17%)      | 119 of 6489 annotated genes (1.83%) |
| transcription by RNA polymerase II (GO:0006366)                     | YJR063W, YNL112W                                                                                  | 2 of 48 genes (4.17%)      | 489 of 6489 annotated genes (7.54%) |
| regulation of DNA metabolic process (GO:0051052)                    | YNL182C, YOR359W                                                                                  | 2 of 48 genes (4.17%)      | 115 of 6489 annotated genes (1.77%) |
| proteolysis involved in protein catabolic process (GO:0051603)      | YBR105C, YLR224W                                                                                  | 2 of 48 genes (4.17%)      | 225 of 6489 annotated genes (3.47%) |
| RNA modification (GO:0009451)                                       | YOL124C, YPL212C                                                                                  | 2 of 48 genes (4.17%)      | 175 of 6489 annotated genes (2.70%) |
| DNA-templated transcription termination (GO:0006353)                | YJR063W, YNL112W                                                                                  | 2 of 48 genes (4.17%)      | 42 of 6489 annotated genes (0.65%)  |
| carbohydrate transport (GO:0008643)                                 | YDR536W, YHR094C                                                                                  | 2 of 48 genes (4.17%)      | 36 of 6489 annotated genes (0.55%)  |
| lipid metabolic process (GO:0006629)                                | YBL039C, YOL151W                                                                                  | 2 of 48 genes (4.17%)      | 297 of 6489 annotated genes (4.58%) |
| ribosomal subunit export from nucleus (GO:0000054)                  | YLR264W                                                                                           | 1 of 48 genes (2.08%)      | 67 of 6489 annotated genes (1.03%)  |
| regulation of organelle organization (GO:0033043)                   | YLR180W                                                                                           | 1 of 48 genes (2.08%)      | 227 of 6489 annotated genes (3.50%) |
| amino acid transport (GO:0006865)                                   | YNL065W                                                                                           | 1 of 48 genes (2.08%)      | 45 of 6489 annotated genes (0.69%)  |
| monoatomic ion transport (GO:0006811)                               | YNR060W                                                                                           | 1 of 48 genes (2.08%)      | 99 of 6489 annotated genes (1.53%)  |
| tRNA aminoacylation for protein translation (GO:0006418)            | YDR037W                                                                                           | 1 of 48 genes (2.08%)      | 36 of 6489 annotated genes (0.55%)  |
| amino acid metabolic process (GO:0006520)                           | YLR180W                                                                                           | 1 of 48 genes (2.08%)      | 157 of 6489 annotated genes (2.42%) |
| DNA recombination (GO:0006310)                                      | YGR159C                                                                                           | 1 of 48 genes (2.08%)      | 194 of 6489 annotated genes (2.99%) |
| response to chemical (GO:0042221)                                   | YOR271C                                                                                           | 1 of 48 genes (2.08%)      | 414 of 6489 annotated genes (6.38%) |
| RNA splicing (GO:0008380)                                           | YEL026W                                                                                           | 1 of 48 genes (2.08%)      | 139 of 6489 annotated genes (2.14%) |
| organelle assembly (GO:0070925)                                     | YLR180W                                                                                           | 1 of 48 genes (2.08%)      | 116 of 6489 annotated genes (1.79%) |
| protein targeting (GO:0006605)                                      | YBR105C                                                                                           | 1 of 48 genes (2.08%)      | 171 of 6489 annotated genes (2.64%) |
| DNA replication (GO:0006260)                                        | YNL182C                                                                                           | 1 of 48 genes (2.08%)      | 139 of 6489 annotated genes (2.14%) |
| generation of precursor metabolites and energy (GO:0006091)         | YOL136C                                                                                           | 1 of 48 genes (2.08%)      | 84 of 6489 annotated genes (1.29%)  |
| response to osmotic stress (GO:0006970)                             | YER062C                                                                                           | 1 of 48 genes (2.08%)      | 88 of 6489 annotated genes (1.36%)  |
| monocarboxylic acid metabolic process (GO:0032787)                  | YOL136C                                                                                           | 1 of 48 genes (2.08%)      | 132 of 6489 annotated genes (2.03%) |
| nucleobase-containing compound transport (GO:0015931)               | YHR196W                                                                                           | 1 of 48 genes (2.08%)      | 121 of 6489 annotated genes (1.86%) |
| cytoplasmic translation (GO:0002181)                                | YLR264W                                                                                           | 1 of 48 genes (2.08%)      | 169 of 6489 annotated genes (2.60%) |
| mRNA processing (GO:0006397)                                        | YEL026W                                                                                           | 1 of 48 genes (2.08%)      | 170 of 6489 annotated genes (2.62%) |

edit graph 

```
cat norm-matrix-deseq2.txt | Rscript ../scripts/draw-heatmap_top_genes.r genes.txt output_2.pdf
```


# **3. Result Interpretation**




