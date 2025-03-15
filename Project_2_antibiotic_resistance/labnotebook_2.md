# Install data
## Reference
	first of all install reference from here:
	https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/845/GCF_000005845.2_ASM584v2/
	
```Bash
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/845/GCF_000005845.2_ASM584v2/GCF_000005845.2_ASM584v2_genomic.fna.gz

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/845/GCF_000005845.2_ASM584v2/GCF_000005845.2_ASM584v2_genomic.gff.gz

```

**so we have .gff and .fna files**

now install shotgun sequences https://figshare.com/articles/dataset/amp_res_2_fastq_zip/10006541/3


```Bash
unzip 1000495.zip
```

```
10006541.zip  amp_res_1.fastq  amp_res_2.fastq
(base) mhprs@Seraph:~/PISH/Genomics/project_2_antibiotic/data/raw/shotgun_seq$ head -10 amp_res_1.fastq
@SRR1363257.37 GWZHISEQ01:153:C1W31ACXX:5:1101:14027:2198 length=101
GGTTGCAGATTCGCAGTGTCGCTGTTCCAGCGCATCACATCTTTGATGTTCACGCCGTGGCGTTTAGCAATGCTTGAAAGCGAATCGCCTTTGCCCACACG
+
@?:=:;DBFADH;CAECEE@@E:FFHGAE4?C?DE<BFGEC>?>FHE4BFFIIFHIBABEECA83;>>@>@CCCDC9@@CC08<@?@BB@9:CC#######
@SRR1363257.46 GWZHISEQ01:153:C1W31ACXX:5:1101:19721:2155 length=101
GTATGAGGTTTTGCTGCATTCTCTGNGCGAATATTAACTCCNTNNNNNTTATAGTTCAAAGCAAGTACCTGTCTCTTATACACATCTCCGAGCCCACGAGC
+
```

#IMP 

Commands to use
	head -20 [filename.format]  # View the first 20 lines of the file  
	cat [filename.fasta]  # Display the entire fasta file  
	wc -l [filename.fastq]  # Count the number of lines in the FASTQ file  
	conda install -c bioconda seqkit  # Install seqkit via conda  
	seqkit stats [filename.fastq.gz]  # Get detailed statistics about the FASTQ file  

stat of seq
```Bash
 awk '{n++} END {print n/4}' amp_res_1.fastq
455876
```

```Bash
(GP_data) mhprs@Seraph:~/PISH/Genomics/project_2_antibiotic/data/raw/shotgun_seq$ seqkit stats amp_res_1.fastq

file             format  type  num_seqs     sum_len  min_len  avg_len  max_len
amp_res_1.fastq  FASTQ   DNA    455,876  46,043,476      101      101      101

```

# **Inspect raw sequencing data with FastQC. Filtering the reads.**
for amp_res_1_fastqc.html 

| Measure                           | Value                   |
| --------------------------------- | ----------------------- |
| Filename                          | amp_res_1.fastq         |
| File type                         | Conventional base calls |
| Encoding                          | Sanger / Illumina 1.9   |
| Total Sequences                   | 455876                  |
| Total Bases                       | 46 Mbp                  |
| Sequences flagged as poor quality | 0                       |
| Sequence length                   | 101                     |
| %GC                               | 50                      |
basic statistics matches with calculated before
but here we have red flag in "per sequence quality" for reverse reads
![[fastqc_read_2.png]]
and "per sequence quality" & "Per tile sequence quality" for forward reads
![[fastq_quality_read_1.png]]![[fastqc_tile_read_1.png]]

- **Per Sequence Quality (reverse reads)** – The average quality of individual reads is too low, indicating potential sequencing errors.
- **Per Sequence Quality (forward reads)** – The same issue: read quality is dropping.
- **Per Tile Sequence Quality (forward reads)** – A specific area (tile) on the sequencing chip shows low quality, possibly due to contamination or hardware issues.

# ** (Optional, 1 bonus point) Filtering the reads**
