# 1 **Obtaining data. Genome sequence.**
	downloading data from here 
> http://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/001/949/185/GCA_001949185.1_Rvar_4.0/GCA_001949185.1_Rvar_4.0_genomic.fna.gz

that's assembled genome from https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=947166




# 2 **Structural annotation.**

for now install precomputed **AUGUSTUS results**  **[protein fasta](https://drive.google.com/file/d/1hCEywBlqNzTrIpQsZTVuZk1S9qKzqQAq/view?usp=sharing) and [gff](https://drive.google.com/file/d/12ShwrgLkvJIYQV2p1UlXklmxSOOxyxj4/view?usp=sharing).** 

```{bash}
grep -c ">" augustus.whole.aa
16435
```


# 3 **Physical localization**

downloaded peptides fasta 
https://disk.yandex.ru/d/xJqQMGX77Xueqg

## Blast

### firstly I did it db using fna file, so it failed

> blast
> 
> 	create db
> ```{Bash}
> 
> makeblastdb -in raw/GCA_001949185.1_Rvar_4.0_genomic.fna/GCA_001949185.1_Rvar_4.0_genomic.fna -dbtype prot -out r_varieornatus_db
> 
> ```
> 	
> 	Building a new DB, current time: 04/07/2025 20:34:06
> 	New DB name:   /home/mhprs/PISH/Genomics/project_4_tardigrades/data/r_varieornatus_db
> 	New DB title:  raw/GCA_001949185.1_Rvar_4.0_genomic.fna/GCA_001949185.1_Rvar_4.0_genomic.fna
> 	Sequence type: Protein
> 	Keep MBits: T
> 	Maximum file size: 3000000000B
> 	Adding sequences from FASTA; added 200 sequences in 0.394072 seconds.
> 
> ```
> (GP4_blast) mhprs@Seraph:~/PISH/Genomics/project_4_tardigrades/data$ ls
> r_varieornatus_db.pdb  r_varieornatus_db.pin  r_varieornatus_db.pot  r_varieornatus_db.ptf  raw/
> r_varieornatus_db.phr  r_varieornatus_db.pjs  r_varieornatus_db.psq  r_varieornatus_db.pto
> ```
> 
> 
> 
> blast didn't show any result
> 
> ```
> (GP4_blast) mhprs@Seraph:~/PISH/Genomics/project_4_tardigrades/data$ blastp -db r_varieornatus_db -query raw/peptides.fa -outfmt 6 -out peptides_blast.txt
> (GP4_blast) mhprs@Seraph:~/PISH/Genomics/project_4_tardigrades/data$ cat peptides_blast.txt
> (GP4_blast) mhprs@Seraph:~/PISH/Genomics/project_4_tardigrades/data$
> ```
> 
> because i used wrong file to create db 


### let's do it again 



```{bash}
(GP4_blast) mhprs@Seraph:~/PISH/Genomics/project_4_tardigrades/data$ makeblastdb -in raw/augustus.whole.aa -dbtype prot
-out db/r_varieornatus_db


Building a new DB, current time: 04/07/2025 20:47:48
New DB name:   /home/mhprs/PISH/Genomics/project_4_tardigrades/data/db/r_varieornatus_db
New DB title:  raw/augustus.whole.aa
Sequence type: Protein
Keep MBits: T
Maximum file size: 3000000000B
Adding sequences from FASTA; added 16435 sequences in 0.188657 seconds.

```

#### output 

> 1       g5641.t1        100.000 9       0       0       1       9       25      33      2.0     21.6
> 1       g15153.t1       100.000 9       0       0       1       9       25      33      2.1     21.6
> 2       g5641.t1        100.000 9       0       0       1       9       36      44      1.5     21.9
> 2       g12562.t1       88.889  9       1       0       1       9       36      44      3.3     20.8
> 2       g5616.t1        88.889  9       1       0       1       9       36      44      6.7     20.0
> 2       g15153.t1       77.778  9       2       0       1       9       36      44      7.5     20.0
> 3       g5641.t1        100.000 13      0       0       1       13      155     167     0.26    24.6
> 4       g5641.t1        100.000 11      0       0       1       11      144     154     0.053   26.2
> 4       g12562.t1       72.727  11      3       0       1       11      144     154     1.6     21.9
> 4       g5616.t1        72.727  11      3       0       1       11      144     154     3.1     21.2
> 4       g702.t1 63.636  11      4       0       1       11      145     155     3.7     21.2
> 4       g15153.t1       63.636  11      4       0       1       11      144     154     5.4     20.4
> 5       g13530.t1       100.000 15      0       0       1       15      224     238     0.002   30.8
> 6       g13530.t1       100.000 15      0       0       1       15      88      102     6.14e-04        32.0
> 7       g14472.t1       100.000 16      0       0       1       16      368     383     0.002   30.8
> 8       g4106.t1        100.000 18      0       0       1       18      222     239     2.00e-06        39.3
> 8       g7861.t1        62.500  16      6       0       1       16      64      79      0.65    23.5
> 8       g4970.t1        57.143  14      6       0       5       18      134     147     8.8     20.4
> 9       g10513.t1       100.000 15      0       0       1       15      305     319     0.003   30.0
> 9       g11806.t1       66.667  15      5       0       1       15      152     166     3.9     21.2
> 9       g16318.t1       66.667  15      5       0       1       15      224     238     4.9     21.2
> 9       g16368.t1       66.667  15      5       0       1       15      204     218     5.2     20.8
> 10      g3428.t1        100.000 11      0       0       1       11      160     170     0.055   26.2
> 10      g11513.t1       87.500  8       1       0       2       9       872     879     8.3     20.0
> 11      g3428.t1        100.000 11      0       0       1       11      129     139     0.11    25.4
> 12      g5616.t1        100.000 12      0       0       1       12      155     166     0.42    23.9
> 13      g5616.t1        100.000 12      0       0       1       12      143     154     0.026   27.3
> 13      g12562.t1       100.000 11      0       0       2       12      144     154     0.052   26.6
> 13      g5443.t1        91.667  12      1       0       1       12      56      67      0.094   25.8
> 13      g5502.t1        91.667  12      1       0       1       12      151     162     0.14    25.0
> 13      g15153.t1       90.909  11      1       0       2       12      144     154     0.21    24.6
> 13      g5503.t1        91.667  12      1       0       1       12      151     162     0.28    24.3
> 13      g5467.t1        100.000 9       0       0       1       9       128     136     1.3     22.3
> 13      g5641.t1        72.727  11      3       0       2       12      144     154     3.0     21.6
> 14      g5641.t1        100.000 9       0       0       1       9       25      33      2.0     21.6
> 14      g15153.t1       100.000 9       0       0       1       9       25      33      2.1     21.6
> 15      g15153.t1       100.000 10      0       0       1       10      45      54      0.19    24.6
> 15      g12562.t1       80.000  10      2       0       1       10      45      54      0.95    22.7
> 15      g5641.t1        80.000  10      2       0       1       10      45      54      1.1     22.3
> 15      g5616.t1        80.000  10      2       0       1       10      45      54      1.4     21.9
> 15      g5503.t1        80.000  10      2       0       1       10      53      62      5.4     20.4
> 15      g5502.t1        80.000  10      2       0       1       10      53      62      6.6     20.0
> 16      g5510.t1        100.000 9       0       0       1       9       169     177     0.32    23.9
> 17      g12510.t1       100.000 12      0       0       1       12      306     317     0.16    25.0
> 19      g5237.t1        100.000 13      0       0       1       13      91      103     0.004   29.6
> 20      g12510.t1       100.000 18      0       0       1       18      425     442     3.47e-07        41.6
> 20      g11320.t1       66.667  12      4       0       4       15      166     177     4.1     21.6
> 20      g8312.t1        61.538  13      5       0       1       13      148     160     5.6     21.2
> 21      g12510.t1       100.000 22      0       0       1       22      443     464     1.49e-09        48.5
> 21      g5927.t1        56.250  16      7       0       7       22      598     613     2.4     22.3
> 21      g8100.t1        56.250  16      7       0       1       16      927     942     9.9     20.8
> 22      g5641.t1        100.000 9       0       0       1       9       25      33      2.0     21.6
> 22      g15153.t1       100.000 9       0       0       1       9       25      33      2.1     21.6
> 23      g5641.t1        100.000 9       0       0       1       9       36      44      1.5     21.9
> 23      g12562.t1       88.889  9       1       0       1       9       36      44      3.3     20.8
> 23      g5616.t1        88.889  9       1       0       1       9       36      44      6.7     20.0
> 23      g15153.t1       77.778  9       2       0       1       9       36      44      7.5     20.0
> 24      g12562.t1       100.000 9       0       0       1       9       128     136     1.00    22.3
> 24      g5502.t1        100.000 9       0       0       1       9       136     144     1.0     22.3
> 24      g5641.t1        100.000 9       0       0       1       9       128     136     1.2     22.3
> 24      g5503.t1        100.000 9       0       0       1       9       136     144     1.2     22.3
> 24      g1285.t1        100.000 9       0       0       1       9       138     146     1.2     22.3
> 24      g15153.t1       100.000 9       0       0       1       9       128     136     1.3     22.3
> 24      g5616.t1        88.889  9       1       0       1       9       128     136     2.3     21.6
> 24      g12388.t1       88.889  9       1       0       1       9       136     144     4.9     20.4
> 24      g5467.t1        88.889  9       1       0       1       9       113     121     5.3     20.4
> 25      g5641.t1        100.000 13      0       0       1       13      155     167     0.26    24.6
> 26      g5641.t1        100.000 9       0       0       1       9       25      33      2.0     21.6
> 26      g15153.t1       100.000 9       0       0       1       9       25      33      2.1     21.6
> 27      g15153.t1       100.000 10      0       0       1       10      45      54      0.19    24.6
> 27      g12562.t1       80.000  10      2       0       1       10      45      54      0.95    22.7
> 27      g5641.t1        80.000  10      2       0       1       10      45      54      1.1     22.3
> 27      g5616.t1        80.000  10      2       0       1       10      45      54      1.4     21.9
> 27      g5503.t1        80.000  10      2       0       1       10      53      62      5.4     20.4
> 27      g5502.t1        80.000  10      2       0       1       10      53      62      6.6     20.0
> 28      g12562.t1       100.000 9       0       0       1       9       128     136     1.00    22.3
> 28      g5502.t1        100.000 9       0       0       1       9       136     144     1.0     22.3
> 28      g5641.t1        100.000 9       0       0       1       9       128     136     1.2     22.3
> 28      g5503.t1        100.000 9       0       0       1       9       136     144     1.2     22.3
> 28      g1285.t1        100.000 9       0       0       1       9       138     146     1.2     22.3
> 28      g15153.t1       100.000 9       0       0       1       9       128     136     1.3     22.3
> 28      g5616.t1        88.889  9       1       0       1       9       128     136     2.3     21.6
> 28      g12388.t1       88.889  9       1       0       1       9       136     144     4.9     20.4
> 28      g5467.t1        88.889  9       1       0       1       9       113     121     5.3     20.4
> 29      g4106.t1        100.000 18      0       0       1       18      222     239     2.00e-06        39.3
> 29      g7861.t1        62.500  16      6       0       1       16      64      79      0.65    23.5
> 29      g4970.t1        57.143  14      6       0       5       18      134     147     8.8     20.4
> 30      g5616.t1        100.000 12      0       0       1       12      155     166     0.42    23.9
> 31      g5616.t1        100.000 12      0       0       1       12      143     154     0.026   27.3
> 31      g12562.t1       100.000 11      0       0       2       12      144     154     0.052   26.6
> 31      g5443.t1        91.667  12      1       0       1       12      56      67      0.094   25.8
> 31      g5502.t1        91.667  12      1       0       1       12      151     162     0.14    25.0
> 31      g15153.t1       90.909  11      1       0       2       12      144     154     0.21    24.6
> 31      g5503.t1        91.667  12      1       0       1       12      151     162     0.28    24.3
> 31      g5467.t1        100.000 9       0       0       1       9       128     136     1.3     22.3
> 31      g5641.t1        72.727  11      3       0       2       12      144     154     3.0     21.6
> 32      g15484.t1       100.000 10      0       0       1       10      831     840     0.46    23.5
> 33      g15484.t1       100.000 12      0       0       1       12      791     802     0.31    24.3
> 34      g15484.t1       100.000 18      0       0       1       18      755     772     9.59e-06        37.4
> 34      g11960.t1       53.846  13      6       0       4       16      556     568     3.5     21.6
> 35      g702.t1 100.000 11      0       0       1       11      158     168     0.47    23.5
> 37      g10514.t1       100.000 10      0       0       1       10      221     230     0.97    22.7
> 37      g2203.t1        77.778  9       2       0       1       9       359     367     8.4     19.6
> 38      g10514.t1       100.000 11      0       0       1       11      42      52      0.60    23.1
> 39      g12562.t1       100.000 10      0       0       1       10      161     170     0.82    22.7
> 40      g12562.t1       100.000 9       0       0       1       9       128     136     1.00    22.3
> 40      g5502.t1        100.000 9       0       0       1       9       136     144     1.0     22.3
> 40      g5641.t1        100.000 9       0       0       1       9       128     136     1.2     22.3
> 40      g5503.t1        100.000 9       0       0       1       9       136     144     1.2     22.3
> 40      g1285.t1        100.000 9       0       0       1       9       138     146     1.2     22.3
> 40      g15153.t1       100.000 9       0       0       1       9       128     136     1.3     22.3
> 40      g5616.t1        88.889  9       1       0       1       9       128     136     2.3     21.6
> 40      g12388.t1       88.889  9       1       0       1       9       136     144     4.9     20.4
> 40      g5467.t1        88.889  9       1       0       1       9       113     121     5.3     20.4
> 41      g3428.t1        100.000 11      0       0       1       11      160     170     0.055   26.2
> 41      g11513.t1       87.500  8       1       0       2       9       872     879     8.3     20.0
> 42      g3679.t1        100.000 12      0       0       1       12      21      32      0.046   26.6
> 43      g5237.t1        100.000 13      0       0       1       13      91      103     0.004   29.6



extract most relevant
---

```
awk '$3 > 90 {print $2}' peptides_blast.txt | uniq  > relevant_genes.txt
```

and extract sequences of these genes

```
xargs samtools faidx ../raw/augustus.whole.aa < relevant_genes.txt
```


# 4. Localization prediction

## ***4a* WoLF PSORT**

### output

g5641.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed1.html#g5641.t1) extr: 31, lyso: 1  
g15153.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed2.html#g15153.t1) extr: 32  
g5641.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed3.html#g5641.t1) extr: 31, lyso: 1  
g13530.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed4.html#g13530.t1) extr: 13, nucl: 6.5, lyso: 5, cyto_nucl: 4.5, plas: 3, E.R.: 3, cyto: 1.5  
g14472.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed5.html#g14472.t1) nucl: 28, plas: 2, cyto: 1, cysk: 1  
g4106.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed6.html#g4106.t1) E.R.: 14.5, E.R._golg: 9.5, extr: 7, golg: 3.5, lyso: 3, pero: 2, plas: 1, mito: 1  
g10513.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed7.html#g10513.t1) nucl: 20, cyto_nucl: 14.5, cyto: 7, extr: 3, E.R.: 1, golg: 1  
g3428.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed8.html#g3428.t1) mito: 18, cyto: 11, extr: 2, nucl: 1  
g5616.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed9.html#g5616.t1) extr: 31, mito: 1  
g12562.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed10.html#g12562.t1) extr: 30, lyso: 2  
g5443.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed11.html#g5443.t1) extr: 28, nucl: 3, cyto: 1  
g5502.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed12.html#g5502.t1) extr: 31, lyso: 1  
g15153.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed13.html#g15153.t1) extr: 32  
g5503.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed14.html#g5503.t1) extr: 29, plas: 1, mito: 1, lyso: 1  
g5467.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed15.html#g5467.t1) extr: 27, plas: 4, mito: 1  
g5641.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed16.html#g5641.t1) extr: 31, lyso: 1  
g15153.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed17.html#g15153.t1) extr: 32  
g5510.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed18.html#g5510.t1) plas: 23, mito: 7, E.R.: 1, golg: 1  
g12510.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed19.html#g12510.t1) plas: 29, cyto: 3  
g5237.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed20.html#g5237.t1) plas: 24, mito: 8  
g12510.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed21.html#g12510.t1) plas: 29, cyto: 3  
g5641.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed22.html#g5641.t1) extr: 31, lyso: 1  
g15153.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed23.html#g15153.t1) extr: 32  
g5641.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed24.html#g5641.t1) extr: 31, lyso: 1  
g12562.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed25.html#g12562.t1) extr: 30, lyso: 2  
g5502.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed26.html#g5502.t1) extr: 31, lyso: 1  
g5641.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed27.html#g5641.t1) extr: 31, lyso: 1  
g5503.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed28.html#g5503.t1) extr: 29, plas: 1, mito: 1, lyso: 1  
g1285.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed29.html#g1285.t1) extr: 25, plas: 5, mito: 1, lyso: 1  
g15153.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed30.html#g15153.t1) extr: 32  
g5641.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed31.html#g5641.t1) extr: 31, lyso: 1  
g15153.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed32.html#g15153.t1) extr: 32  
g12562.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed33.html#g12562.t1) extr: 30, lyso: 2  
g5502.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed34.html#g5502.t1) extr: 31, lyso: 1  
g5641.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed35.html#g5641.t1) extr: 31, lyso: 1  
g5503.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed36.html#g5503.t1) extr: 29, plas: 1, mito: 1, lyso: 1  
g1285.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed37.html#g1285.t1) extr: 25, plas: 5, mito: 1, lyso: 1  
g15153.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed38.html#g15153.t1) extr: 32  
g4106.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed39.html#g4106.t1) E.R.: 14.5, E.R._golg: 9.5, extr: 7, golg: 3.5, lyso: 3, pero: 2, plas: 1, mito: 1  
g5616.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed40.html#g5616.t1) extr: 31, mito: 1  
g12562.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed41.html#g12562.t1) extr: 30, lyso: 2  
g5443.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed42.html#g5443.t1) extr: 28, nucl: 3, cyto: 1  
g5502.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed43.html#g5502.t1) extr: 31, lyso: 1  
g15153.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed44.html#g15153.t1) extr: 32  
g5503.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed45.html#g5503.t1) extr: 29, plas: 1, mito: 1, lyso: 1  
g5467.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed46.html#g5467.t1) extr: 27, plas: 4, mito: 1  
g15484.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed47.html#g15484.t1) nucl: 17.5, cyto_nucl: 15.3333, cyto: 12, cyto_mito: 6.83333, plas: 1, golg: 1  
g702.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed48.html#g702.t1) extr: 29, plas: 2, lyso: 1  
g10514.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed49.html#g10514.t1) nucl: 19, cyto_nucl: 15, cyto: 9, extr: 3, mito: 1  
g12562.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed50.html#g12562.t1) extr: 30, lyso: 2  
g5502.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed51.html#g5502.t1) extr: 31, lyso: 1  
g5641.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed52.html#g5641.t1) extr: 31, lyso: 1  
g5503.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed53.html#g5503.t1) extr: 29, plas: 1, mito: 1, lyso: 1  
g1285.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed54.html#g1285.t1) extr: 25, plas: 5, mito: 1, lyso: 1  
g15153.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed55.html#g15153.t1) extr: 32  
g3428.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed56.html#g3428.t1) mito: 18, cyto: 11, extr: 2, nucl: 1  
g3679.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed57.html#g3679.t1) extr: 26, mito: 2, lyso: 2, plas: 1, E.R.: 1  
g5237.t1 [details](https://wolfpsort.hgc.jp/results/aKCd3c9ddc7ba4f29747f1fdcac85ea207a.detailed58.html#g5237.t1) plas: 24, mito: 8






### **Relevant Proteins for DNA Protection/Repair Analysis**

to understand result let's choose most associated with nuclear 

| Gene ID       | Predicted Localization (Scores)                         | Main Localization |
| ------------- | ------------------------------------------------------- | ----------------- |
| **g14472.t1** | `nucl: 28, plas: 2, cyto: 1, cysk: 1`                   | Nucleus           |
| **g10514.t1** | `nucl: 19, cyto_nucl: 15, cyto: 9, extr: 3, mito: 1`    | Nucleus           |
| **g15484.t1** | `nucl: 17.5, cyto_nucl: 15.3, cyto: 12, cyto_mito: 6.8` | Nucleus/Cytoplasm |
| **g10513.t1** | `nucl: 20, cyto_nucl: 14.5, cyto: 7, extr: 3, E.R.: 1`  | Nucleus/Cytoplasm |
| **g3428.t1**  | `mito: 18, cyto: 11, extr: 2, nucl: 1`                  | mitochondrial     |


---
## ***4b.* TargetP Server**

### output


 TargetP-2.0 Results (Non-Plant Organism)

**Timestamp:** 2025-04-07 20:47:12

|ID|Prediction|OTHER|SP|mTP|CS Position|
|---|---|---|---|---|---|
|g5641.t1|SP|0.000130|0.999869|0.000001|CS pos: 16-17. ACA-AS. Pr: 0.4873|
|g15153.t1|SP|0.000014|0.999986|0.000000|CS pos: 16-17. AYA-AN. Pr: 0.8378|
|g5641.t1|SP|0.000130|0.999869|0.000001|CS pos: 16-17. ACA-AS. Pr: 0.4873|
|g13530.t1|SP|0.116007|0.883840|0.000153|CS pos: 19-20. TIP-FT. Pr: 0.3552|
|g14472.t1|OTHER|0.999999|0.000001|0.000000||
|g4106.t1|OTHER|0.729658|0.266917|0.003425||
|g10513.t1|OTHER|0.999999|0.000001|0.000000||
|g3428.t1|OTHER|0.999903|0.000033|0.000064||
|g5616.t1|SP|0.000067|0.999933|0.000000|CS pos: 16-17. ACA-AN. Pr: 0.5270|
|g12562.t1|SP|0.000076|0.999923|0.000001|CS pos: 16-17. SYA-AN. Pr: 0.7910|
|g5443.t1|OTHER|0.952853|0.043784|0.003363||
|g5502.t1|SP|0.001134|0.998823|0.000043|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g15153.t1|SP|0.000014|0.999986|0.000000|CS pos: 16-17. AYA-AN. Pr: 0.8378|
|g5503.t1|SP|0.001222|0.998720|0.000058|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g5467.t1|SP|0.000096|0.999845|0.000059|CS pos: 16-17. ASA-GS. Pr: 0.6543|
|g5641.t1|SP|0.000130|0.999869|0.000001|CS pos: 16-17. ACA-AS. Pr: 0.4873|
|g15153.t1|SP|0.000014|0.999986|0.000000|CS pos: 16-17. AYA-AN. Pr: 0.8378|
|g5510.t1|OTHER|0.999108|0.000016|0.000876||
|g12510.t1|OTHER|0.999738|0.000099|0.000163||
|g5237.t1|OTHER|0.999545|0.000345|0.000111||
|g12510.t1|OTHER|0.999738|0.000099|0.000163||
|g5641.t1|SP|0.000130|0.999869|0.000001|CS pos: 16-17. ACA-AS. Pr: 0.4873|
|g15153.t1|SP|0.000014|0.999986|0.000000|CS pos: 16-17. AYA-AN. Pr: 0.8378|
|g5641.t1|SP|0.000130|0.999869|0.000001|CS pos: 16-17. ACA-AS. Pr: 0.4873|
|g12562.t1|SP|0.000076|0.999923|0.000001|CS pos: 16-17. SYA-AN. Pr: 0.7910|
|g5502.t1|SP|0.001134|0.998823|0.000043|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g5641.t1|SP|0.000130|0.999869|0.000001|CS pos: 16-17. ACA-AS. Pr: 0.4873|
|g5503.t1|SP|0.001222|0.998720|0.000058|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g1285.t1|SP|0.003029|0.996798|0.000173|CS pos: 16-17. ASA-TS. Pr: 0.7127|
|g15153.t1|SP|0.000014|0.999986|0.000000|CS pos: 16-17. AYA-AN. Pr: 0.8378|
|g5641.t1|SP|0.000130|0.999869|0.000001|CS pos: 16-17. ACA-AS. Pr: 0.4873|
|g15153.t1|SP|0.000014|0.999986|0.000000|CS pos: 16-17. AYA-AN. Pr: 0.8378|
|g12562.t1|SP|0.000076|0.999923|0.000001|CS pos: 16-17. SYA-AN. Pr: 0.7910|
|g5502.t1|SP|0.001134|0.998823|0.000043|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g5641.t1|SP|0.000130|0.999869|0.000001|CS pos: 16-17. ACA-AS. Pr: 0.4873|
|g5503.t1|SP|0.001222|0.998720|0.000058|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g1285.t1|SP|0.003029|0.996798|0.000173|CS pos: 16-17. ASA-TS. Pr: 0.7127|
|g15153.t1|SP|0.000014|0.999986|0.000000|CS pos: 16-17. AYA-AN. Pr: 0.8378|
|g4106.t1|OTHER|0.729658|0.266917|0.003425||
|g5616.t1|SP|0.000067|0.999933|0.000000|CS pos: 16-17. ACA-AN. Pr: 0.5270|
|g12562.t1|SP|0.000076|0.999923|0.000001|CS pos: 16-17. SYA-AN. Pr: 0.7910|
|g5443.t1|OTHER|0.952853|0.043784|0.003363||
|g5502.t1|SP|0.001134|0.998823|0.000043|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g15153.t1|SP|0.000014|0.999986|0.000000|CS pos: 16-17. AYA-AN. Pr: 0.8378|
|g5503.t1|SP|0.001222|0.998720|0.000058|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g5467.t1|SP|0.000096|0.999845|0.000059|CS pos: 16-17. ASA-GS. Pr: 0.6543|
|g15484.t1|OTHER|0.999980|0.000010|0.000010||
|g702.t1|SP|0.000347|0.999652|0.000001|CS pos: 16-17. ALA-AN. Pr: 0.8153|
|g10514.t1|OTHER|0.999543|0.000349|0.000107||
|g12562.t1|SP|0.000076|0.999923|0.000001|CS pos: 16-17. SYA-AN. Pr: 0.7910|
|g5502.t1|SP|0.001134|0.998823|0.000043|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g5641.t1|SP|0.000130|0.999869|0.000001|CS pos: 16-17. ACA-AS. Pr: 0.4873|
|g5503.t1|SP|0.001222|0.998720|0.000058|CS pos: 16-17. ASA-GS. Pr: 0.6833|
|g1285.t1|SP|0.003029|0.996798|0.000173|CS pos: 16-17. ASA-TS. Pr: 0.7127|
|g15153.t1|SP|0.000014|0.999986|0.000000|CS pos: 16-17. AYA-AN. Pr: 0.8378|
|g3428.t1|OTHER|0.999903|0.000033|0.000064||
|g3679.t1|SP|0.001755|0.998023|0.000222|CS pos: 18-19. TFA-AR. Pr: 0.5523|
|g5237.t1|OTHER|0.999545|0.000345|0.000111||

we interested in OTHER




_At that moment, I realized that my filtered file had duplicates, so I cleaned it, but it didn’t change the results_

# **5. BLAST search**

https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Get&VIEW_RESULTS=FromRes&RID=Z89UZCH9016&UNIQ_OBJ_NAME=A_SearchResults_1u1s91_1SQ3_dgNaAKJ62Pu0_GTJ71_mUwIZ&QUERY_INDEX=0




# **6. Pfam prediction**


| Query             | Hit type    | PSSM-ID | From |  To | E-Value     | Bitscore | Accession | Short name | Incomplete | Superfamily |
| ----------------- | ----------- | ------- | ---: | --: | ----------- | -------: | --------- | ---------- | ---------- | ----------- |
| Q#1 - >g5641.t1   | specific    | 426342  |   73 | 123 | 9.54232e-09 |  47.7935 | pfam01607 | CBM_14     | -          | cl02629     |
| Q#2 - >g15153.t1  | specific    | 426342  |   73 | 123 | 1.80675e-08 |  47.0231 | pfam01607 | CBM_14     | -          | cl02629     |
| Q#7 - >g3428.t1   | specific    | 463900  |   95 | 158 | 7.26835e-06 |  39.9294 | pfam13499 | EF-hand_7  | -          | cl38414     |
| Q#7 - >g3428.t1   | specific    | 463900  |   26 |  85 | 2.59016e-05 |  38.7738 | pfam13499 | EF-hand_7  | -          | cl38414     |
| Q#8 - >g5616.t1   | specific    | 426342  |   73 | 123 | 8.33981e-09 |  48.1787 | pfam01607 | CBM_14     | -          | cl02629     |
| Q#9 - >g12562.t1  | specific    | 426342  |   69 | 123 | 3.45402e-09 |  49.3343 | pfam01607 | CBM_14     | -          | cl02629     |
| Q#11 - >g5502.t1  | specific    | 426342  |   81 | 131 | 6.91114e-09 |  48.9491 | pfam01607 | CBM_14     | -          | cl02629     |
| Q#12 - >g5503.t1  | specific    | 426342  |   81 | 131 | 7.84355e-08 |  45.8675 | pfam01607 | CBM_14     | -          | cl02629     |
| Q#13 - >g5467.t1  | specific    | 426342  |   58 | 108 | 2.88722e-08 |  45.4823 | pfam01607 | CBM_14     | -          | cl02629     |
| Q#15 - >g12510.t1 | superfamily | 383772  |  144 | 295 | 0.00457919  |  35.7694 | cl04571   | MARVEL     | -          | -           |
| Q#17 - >g1285.t1  | specific    | 426342  |   77 | 133 | 2.73677e-09 |  49.3343 | pfam01607 | CBM_14     | -          | cl02629     |
| Q#18 - >g15484.t1 | specific    | 462568  |   10 |  93 | 8.55484e-22 |  88.4912 | pfam08700 | Vps51      | -          | cl46298     |
| Q#18 - >g15484.t1 | superfamily | 473160  |   39 | 235 | 1.59045e-10 |  61.2869 | cl19297   | COG2       | C          | -           |
| Q#18 - >g15484.t1 | specific    | 433643  |  862 | 897 | 0.00193586  |  35.2262 | pfam14013 | MT0933     | C          | cl16536     |
| Q#19 - >g702.t1   | specific    | 426342  |   73 | 124 | 4.17094e-07 |  43.5563 | pfam01607 | CBM_14     | -          | cl02629     |
| Q#21 - >g3679.t1  | superfamily | 426242  |   73 | 280 | 8.30682e-32 |  114.685 | cl27699   | Astacin    | -          | -           |

# **7. Integrate your various pieces of evidence**





| Gene ID   | Best BLAST Hit (Annotation/E-Value)                                                                                                                        | Pfam Domains (E-Value)            | WoLF PSORT Localization (Top Score) | TargetP Prediction |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- | ----------------------------------- | ------------------ |
| g5641.t1  | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1") <br>5e-13    | CBM_14 (9.54e-09)                 | Extracellular (31)                  | SP                 |
| g15153.t1 | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>2e-14     | CBM_14 (1.81e-08)                 | Extracellular (32)                  | SP                 |
| g13530.t1 | -                                                                                                                                                          | -                                 | Extracellular (13)/Nuclear (6.5)    | SP                 |
| g14472.t1 | [P0DOW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DOW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DOW4.1")<br>0         | -                                 | Nuclear (28)                        | OTHER              |
| g4106.t1  | -                                                                                                                                                          | -                                 | ER (14.5)                           | OTHER              |
| g10513.t1 | -                                                                                                                                                          | -                                 | Nuclear (20)                        | OTHER              |
| g3428.t1  | [Q09510.1](https://www.ncbi.nlm.nih.gov/protein/Q09510.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q09510.1")<br>9e-65     | EF-hand_7 (7.27e-06)              | Mitochondrial (18)                  | OTHER              |
| g5616.t1  | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>2e-14     | CBM_14 (8.34e-09)                 | Extracellular (31)                  | SP                 |
| g12562.t1 | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br><br>7e-13 | CBM_14 (3.45e-09)                 | Extracellular (30)                  | SP                 |
| g5443.t1  | -                                                                                                                                                          | -                                 | Extracellular (28)                  | OTHER              |
| g5502.t1  | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>6e-14     | CBM_14 (6.91e-09)                 | Extracellular (31)                  | SP                 |
| g5503.t1  | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>7e-14     | CBM_14 (7.84e-08)                 | Extracellular (29)                  | SP                 |
| g5467.t1  | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>4e-13     | CBM_14 (2.89e-08)                 | Extracellular (27)                  | SP                 |
| g5510.t1  | -                                                                                                                                                          | -                                 | Plasma membrane (23)                | OTHER              |
| g12510.t1 | -                                                                                                                                                          | MARVEL (0.00458)                  | Plasma membrane (29)                | OTHER              |
| g5237.t1  | -                                                                                                                                                          | -                                 | Plasma membrane (24)                | OTHER              |
| g1285.t1  | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>2e-12     | CBM_14 (2.74e-09)                 | Extracellular (25)                  | SP                 |
| g15484.t1 | [Q155U0.1](https://www.ncbi.nlm.nih.gov/protein/Q155U0.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q155U0.1")<br><br>0     | Vps51 (8.55e-22), COG2 (1.59e-10) | Nuclear (17.5)/Cytoplasm (15.3)     | OTHER              |
| g702.t1   | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>1e-11     | CBM_14 (4.17e-07)                 | Extracellular (29)                  | SP                 |
| g10514.t1 | -                                                                                                                                                          | -                                 | Nuclear (19)                        | OTHER              |
| g3679.t1  | [Q19269.2](https://www.ncbi.nlm.nih.gov/protein/Q19269.2?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q19269.2")<br>7e-22     | Astacin (8.31e-32)                | Extracellular (26)                  | SP                 |


most relevant


| Gene ID   | Best BLAST Hit (E-Value)                                                                                                                               | Pfam Domains         | WoLF PSORT Localization | TargetP |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------- | ----------------------- | ------- |
| g14472.t1 | [P0DOW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DOW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DOW4.1")<br>0     | -                    | Nuclear (28)            | OTHER   |
| g15484.t1 | [Q155U0.1](https://www.ncbi.nlm.nih.gov/protein/Q155U0.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q155U0.1")<br><br>0 | Vps51 (8.55e-22)     | Nuclear (17.5)          | OTHER   |
| g10514.t1 | -                                                                                                                                                      | -                    | Nuclear (19)            | OTHER   |
| g3428.t1  | [Q09510.1](https://www.ncbi.nlm.nih.gov/protein/Q09510.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q09510.1")<br>9e-65 | EF-hand_7 (7.27e-06) | Mitochondrial (18)      | OTHER   |


### Recommended Entries for Experimental Verification as Novel Tardigrade-Specific DNA-Binding Proteins:

#### **1. g14472.t1**

- **BLAST Hit**:
    
    - **Protein**: Damage suppressor protein ([P0DOW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DOW4.1))
        
    - **E-value**: `0.0` (100% identity to _Ramazzottius varieornatus_ protein)
        
- **Key Features**:
    
    - **Function**: Binds chromatin, protects DNA from hydroxyl radicals and X-ray damage (experimentally validated in tardigrades).
        
    - **Localization**: Nuclear (`nucl: 28` via WoLF PSORT).
        
    - **Domain**: None in Pfam, but UniProt annotation confirms DNA-protective role.
        
- **Reliability**:
    
    - **High confidence**: Direct experimental evidence of DNA interaction in tardigrades.
        
    - **Specificity**: Tardigrade-specific homolog with no close relatives in other species.
        

---

#### **2. g15484.t1**

- **BLAST Hit**:
    
    - **Protein**: Vacuolar protein sorting-associated protein 51 homolog ([Q155U0.1](https://www.ncbi.nlm.nih.gov/protein/Q155U0.1))
        
    - **E-value**: `0.0` (high-confidence match)
        
- **Key Features**:
    
    - **Function**: Golgi/endosome trafficking (GARP/EARP complexes).
        
    - **Localization**: Nuclear (`nucl: 17.5`) despite UniProt annotation for Golgi/endosomes.
        
    - **Domain**: Vps51 (`8.55e-22`), involved in vesicle transport.
        
- **Reliability**:
    
    - **Moderate caution**: Nuclear localization conflicts with UniProt data.
        
    - **Hypothesis**: May moonlight in DNA repair via chromatin organization (requires validation).
        

---

#### **3. g10514.t1**

- **BLAST Hit**: No significant hits.
    
- **Key Features**:
    
    - **Localization**: Nuclear (`nucl: 19`).
        
    - **Domains**: None detected.
        
- **Reliability**:
    
    - **Novelty candidate**: No homologs in databases, suggesting tardigrade-specificity.
        
    - **Risk**: Unknown function; prioritize if g14472.t1 validation succeeds.
        

---

#### **4. g3428.t1**

- **BLAST Hit**:
    
    - **Protein**: Myosin regulatory light chain ([Q09510.1](https://www.ncbi.nlm.nih.gov/protein/Q09510.1))
        
    - **E-value**: `9e-65` (strong match to _C. elegans_ myosin).
        
- **Key Features**:
    
    - **Function**: Regulates myosin II activity (cytoskeleton, no DNA-binding role).
        
    - **Domain**: EF-hand_7 (`7.27e-06`; calcium-binding).
        
    - **Localization**: Mitochondrial (`mito: 18`).
        
- **Reliability**:
    
    - **Low priority**: No evidence of DNA interaction; likely a false positive.