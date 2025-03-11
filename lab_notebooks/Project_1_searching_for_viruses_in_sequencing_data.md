  
# Step 0(downloading data)
we have article https://www.nature.com/articles/s41586-020-2008-3 
and data **PRJNA603194** 
it's recommended  to use fastq-dumb( #WL what difference vs **prefetch** )
```Bash
conda create -n GP_data
conda install -c bioconda sra-tools
prefetch PRJNA603194
```
# Step 0.1 (optional) Assembly 
```Bash
conda create -n GP_spades -c bioconda sra-tools spades
cd ~/PISH/Genomics/SRR10971381
fastq-dump --split-files --gzip SRR10971381.sra

mkdir -p ~/PISH/Genomics/SPAdes_output
```
#RER run spades later, now download [scaffolds](https://drive.google.com/file/d/1PU6gQiF2CvxYhmWxVCWuESnG1XCZvibf/view?usp=drive_link)

```Bash
spades.py -1 SRR10971381_1.fastq.gz -2 SRR10971381_2.fastq.gz -o PISH/Genomics/SPAdes_output --threads 2
```


# Step 1(data research)
```Bash
mamba install quast
```
```Bash
 quast assembly/spades_scaffolds.fasta -o quast_output/
```
#INF -o it's not necessary to already exist, quast create this directory 
here is output
```Bash
(GP_data) mhprs@Seraph:~/PISH/Genomics$ ls quast_output/
basic_stats  icarus_viewers  report.html  report.tex  report.txt             transposed_report.tsv
icarus.html  quast.log       report.pdf   report.tsv  transposed_report.tex  transposed_report.txt
```
[here](https://quast.sourceforge.net/docs/manual.html#sec3) we have description:
QUAST output contains:

| **FILE**             | **DESCRIPRION**                                                                                                                                         |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| report.txt           | assessment summary in plain text format,                                                                                                                |
| report.tsv           | tab-separated version of the summary, suitable for spreadsheets (Google Docs, Excel, etc),                                                              |
| report.tex           | LaTeX version of the summary,                                                                                                                           |
| icarus.html          | Icarus main menu with links to interactive viewers. See [section 3.4](https://quast.sourceforge.net/docs/manual.html#sec3.4) for details,               |
| report.pdf           | all other plots combined with all tables (file is created if matplotlib python library is installed),                                                   |
| report.html          | HTML version of the report with interactive plots inside,                                                                                               |
| contigs_reports/     | (only if a reference genome is provided)                                                                                                                |
| misassemblies_report | detailed report on misassemblies. See [section 3.1.2](https://quast.sourceforge.net/docs/manual.html#sec3.1.2) for details,                             |
| unaligned_report     | detailed report on unaligned and partially unaligned contigs. See [section 3.1.3](https://quast.sourceforge.net/docs/manual.html#sec3.1.3) for details, |
| k_mer_stats/         | (only if [`--k-mer-stats`](https://quast.sourceforge.net/docs/manual.html#k_mer_stats) option is specified)                                             |
| kmers_report         | detailed report on k-mer-based metrics,                                                                                                                 |
| reads_stats/         | (only if [reads](https://quast.sourceforge.net/docs/manual.html#reads_opt) are provided)                                                                |
| reads_report         | detailed report on mapped reads statistics.                                                                                                             |
```Bash
cat quast_output/report.txt
```

#IMP 
Largest contig              29907
contigs (>= 1000 bp)      666

# **Step 2.**(filtering data)
we have something like this:
>NODE_102207_length_79_cov_186.000000
>NODE_102208_length_79_cov_185.500000
>NODE_102209_length_79_cov_185.500000
>NODE_102210_length_79_cov_185.000000
>NODE_102211_length_79_cov_185.000000
>NODE_102212_length_79_cov_184.500000
>NODE_102213_length_79_cov_184.000000

filter low coverage(<5) and short sequences(<100)
```Bash
awk '/^>/ {match($0, /_length_([0-9]+)_cov_([0-9.]+)/, a); if (a[1] >= 100 && a[2] >= 5) {keep=1; print} else {keep=0} next} keep {print}' assembly/spades_scaffolds.fasta > assembly/filtered_scaffolds.fasta


(GP_data) mhprs@Seraph:~/PISH/Genomics/assembly$ wc -l filtered_scaffolds.fasta
74616 assembly/filtered_scaffolds.fasta
(GP_data) mhprs@Seraph:~/PISH/Genomics/assembly$ wc -l spades_scaffolds.fasta
624562 spades_scaffolds.fasta
```

a bit smaller amount 
now use [virsorter](https://github.com/jiarong/VirSorter2?ysclid=m83ej6ixp1833178705) 
i have issues with 
```Bash
virsorter setup -d db -j 4
```
so I downloaded it manually here: `https://osf.io/v46sc/download`   
```Bash
 virsorter run -w virsorter_out -i assembly/filtered_scaffolds.fasta --db-dir ./db/db all --min-length 1000
```

it took about 10 minutes 

`ls virsorter_out/ 
- config.yaml  
- final-viral-boundary.tsv
- final-viral-combined.fa
- final-viral-score.tsv  
- iter-0  
- log
```Bash
column -s $'\t' -t virsorter_out/final-viral-score.tsv | less -S
```
Output:

seqname                                      dsDNAphage  NCLDV  RNA    ssDNA  lavidaviridae  max_score  max_score_group>
NODE_1_length_29907_cov_150.822528||full     0.640       0.307  0.960  0.100  0.820          0.960      RNA            >
NODE_3_length_5584_cov_13759.613946||full    0.627       0.287  0.175  0.607  0.067          0.627      dsDNAphage     >
NODE_9_length_3842_cov_27.242963||full       0.493       0.967  0.620  0.907  1.000          1.000      lavidaviridae  >
NODE_15_length_3618_cov_6.273652||full       0.987       0.700  0.560  0.947  0.720          0.987      dsDNAphage     >

select high-scored samples:
```Bash
 awk -F'\t' '$7 > 0.9 {print $1}' virsorter_out/final-viral-score.tsv > process/filtered_seqnames.txt
```

so far output have that format:
(GP_virsorter) mhprs@Seraph:~/PISH/Genomics$ head process/filtered_seqnames.txt
seqname
NODE_1_length_29907_cov_150.822528||full

we need to change it a bit(exclude `||full||` )

```Bash
sed 's/||full//g' process/filtered_seqnames.txt > process/filtered_seqnames_clean.txt

seqtk subseq assembly/filtered_scaffolds.fasta process/filtered_seqnames_clean.txt > process/filtered_sequences.fa
```


# Step 3(BLAST time)
send filtered_sequences.fa(n=30) to BLAST, meanwhile read the paper: https://academic.oup.com/ve/article/3/2/vex030/4629376  #WL
where we can find 

# Step 4(Prokka time)
```Bash
prokka --kingdom Viruses --outdir prokka_annotation --prefix viral_sample process/filtered_sequences.fa
```

(GP_balst) mhprs@Seraph:~/PISH/Genomics$ ls prokka_annotation/
- viral_sample.err 
- -viral_sample.ffn
- viral_sample.fsa
- viral_sample.gff
- viral_sample.sqn
- viral_sample.tsv
- viral_sample.faa
- viral_sample.fna
- viral_sample.gbk  
- viral_sample.log
- viral_sample.tbl
- viral_sample.txt

but now extract only one seq that hit the most with SARS-COV2 
```Bash
seqtk subseq process/filtered_sequences.fa < (echo"NODE_1_length_29907_cov_150.822528") > NODE_1.fasta
```

	but it turned out that our sequences are inversely directed(because gene `Replicase poliprotein 1a` in our case has position 16420..29637 but in ref seq it is 266..13468)
	so revert our results
```Bash
seqkit seq -r -p NODE_1.fasta > NODE_1_rc.fasta
prokka --outdir prokka_NODE1_rc --prefix NODE_1 --kingdom Viruses NODE_1_rc.fasta
```

here we have:

 #IMP Prokka Annotation for NODE_1
```Bash
grep "CDS" prokka_NODE1_rc/NODE_1.gff
```

| Start | End   | Strand | Gene | Product                   | UniProt Reference | Inference                                                                       |
| ----- | ----- | ------ | ---- | ------------------------- | ----------------- | ------------------------------------------------------------------------------- |
| 271   | 13488 | +      | 1a   | Replicase polyprotein 1a  | P0C6U8            | ab initio prediction: Prodigal:002006, similar to AA sequence: UniProtKB:P0C6U8 |
| 13773 | 21560 | +      | rep  | Replicase polyprotein 1ab | P0C6X7            | ab initio prediction: Prodigal:002006, similar to AA sequence: UniProtKB:P0C6X7 |
| 21568 | 25389 | +      | S    | Spike glycoprotein        | P59594            | ab initio prediction: Prodigal:002006, similar to AA sequence: UniProtKB:P59594 |
| 25398 | 26225 | +      | 3a   | Protein 3a                | P59632            | ab initio prediction: Prodigal:002006, similar to AA sequence: UniProtKB:P59632 |
| 26528 | 27196 | +      | M    | Membrane protein          | P59596            | ab initio prediction: Prodigal:002006, similar to AA sequence: UniProtKB:P59596 |
| 27207 | 27392 | +      | -    | Hypothetical protein      | -                 | ab initio prediction: Prodigal:002006                                           |
| 27399 | 27764 | +      | 7a   | Protein 7a                | P59635            | ab initio prediction: Prodigal:002006, similar to AA sequence: UniProtKB:P59635 |
| 27899 | 28264 | +      | -    | Hypothetical protein      | -                 | ab initio prediction: Prodigal:002006                                           |
| 28279 | 29538 | +      | N    | Nucleoprotein             | P59595            | ab initio prediction: Prodigal:002006, similar to AA sequence: UniProtKB:P59595 |

---
 **Notes:**
- **Start / End** — Coordinates of the gene on the contig.
- **Strand** — Direction (`+` = forward strand, `-` = reverse strand).
- **Gene** — Gene name (if predicted).
- **Product** — Protein product.
- **UniProt Reference** — Link to the analogous protein in UniProt.
- **Inference** — The method used to predict the gene and its similarity to known sequences (e.g., Prodigal, UniProt references).

so I downloaded gff3 file from gene bank to compare
> GenBank file by clicking on **Send to** → **File** → **GenBank** → **gff3** format.

| Start | End   | Strand | Gene   | Product                     | UniProt Reference  | Inference                                                                   |
| ----- | ----- | ------ | ------ | --------------------------- | ------------------ | --------------------------------------------------------------------------- |
| 266   | 13483 | +      | 1a     | ORF1a polyprotein           | YP_009725295.1     | pp1a                                                                      |
| 13468 | 21555 | +      | rep    | ORF1ab polyprotein          | YP_009724389.1     | translated by -1 ribosomal frameshift; ribosomal slippage                   |
| 21563 | 25384 | +      | S      | surface glycoprotein        | YP_009724390.1     | structural protein; spike protein                                         |
| 25393 | 26220 | +      | ORF3a  | ORF3a protein               | YP_009724391.1     |                                                                           |
| 26245 | 26472 | +      | E      | envelope protein            | YP_009724392.1     | ORF4; structural protein                                                   |
| 26523 | 27191 | +      | M      | membrane glycoprotein       | YP_009724393.1     | ORF5; structural protein                                                   |
| 27202 | 27387 | +      | ORF6   | ORF6 protein                | YP_009724394.1     |                                                                           |
| 27394 | 27759 | +      | ORF7a  | ORF7a protein               | YP_009724395.1     |                                                                           |
| 27756 | 27887 | +      | ORF7b  | ORF7b                     | YP_009725318.1     |                                                                           |
| 27894 | 28259 | +      | ORF8   | ORF8 protein                | YP_009724396.1     |                                                                           |
| 28274 | 29533 | +      | N      | nucleocapsid phosphoprotein | YP_009724397.2     | ORF9; structural protein                                                   |
| 29558 | 29674 | +      | ORF10  | ORF10 protein               | YP_009725255.1     |                                                                           |


here i open it in UGENE
![[Pasted image 20250311110602.png]]
and GeneBank
![[Pasted image 20250311110651.png]]
