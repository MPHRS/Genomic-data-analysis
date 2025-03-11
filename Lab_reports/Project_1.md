# Virus Detection in Sequencing Data: A Metagenomic Approach to Identifying Novel Viral Genomes

## Abstract
Viral changes can lead to outbreaks of morbidity and epidemics, which require methods of analysis and detection of these changes.. In this study  we approach a metagenomic pipeline to search for viral sequences in BioProject PRJNA603194. Raw data were assembled with SPAdes. Quality was checked using QUAST, and contigs were filtered based on length and coverage. Viral candidates were identified using VirSorter2, and validated using BLAST. Notably, a candidate contig (NODE_1) showed significant similarity to bat SARS-like coronaviruses. After orienting NODE_1, the S gene was extracted and used for primer design. This pipeline provides a robust framework for rapid virus variants discovery.



## **Introduction**


Metagenomic analysis provides a rapid method for identifying new species and strains that is critical during epidemics. This approach enables fast determination of novel virus variants by comparing them with related strains in databases, identifying key differences in genes that make them pathogenic, thereby facilitating early pathogen detection and timely public health responses. For instance, Zhou et al. demonstrated the effectiveness of metatranscriptomic sequencing in identifying a novel coronavirus in patients with severe respiratory disease [1].

Coronaviruses, as described by biologists Jean and Peter Medawar, are "simply a piece of bad news wrapped up in protein." These enveloped, nonsegmented, single-stranded, positive-sense RNA viruses are distinguished by their unique crown-like appearance under electron microscopy. Their genomes, approximately 30,000 nucleotides long, encode up to 29 proteins essential for replication and immune evasion. This distinctive genomic structure has enabled the early identification of novel strains, such as SARS-CoV, MERS-CoV, and SARS-CoV-2, each of which has triggered significant global health crises through efficient respiratory transmission. In January, scientists sequenced the SARS-CoV-2 genome from a patient in Wuhan, sparking an urgent global effort to decipher this viral blueprint and develop effective drugs, vaccines, and tools to combat the Covid-19 pandemic.[2,3]

## **Methods**

This study analyzed SARS-CoV-2 genomic data (PRJNA603194, Wu et al., 2020). Raw sequencing reads (SRR10971381) were retrieved from the SRA and assembled using SPAdes (v3.15.2). Scaffolds were quality-checked with QUAST (v5.0.2) and filtered based on coverage (>5) and length (>100 bp) using an AWK script.

Viral sequences were identified with VirSorter2, selecting those with scores >0.9. The resulting sequences were analyzed via NCBI BLAST, restricting hits to pre-December 2019 entries. Functional annotation was performed with Prokka (v1.14.6), revealing key SARS-CoV-2 genes. A notable sequence (NODE_1) was reverse-complemented, re-annotated, and compared to reference genomes(Bat SARS-like coronavirus). The spike gene was extracted for primer design using NCBI Primer-BLAST. Custom scripts and additional details are in the supplementary materials.

## **Results**

The dataset **PRJNA603194** was downloaded using `prefetch`, and reads were extracted using `fastq-dump`. As SPAdes assembly was not performed, preassembled scaffolds were used for further analysis. Quality assessment with **QUAST** showed that the largest contig had a length of 29,907 bp, and there were 666 contigs of at least 1000 bp.

Filtering was performed based on read length (≥100 bp) and coverage (≥5x), reducing the dataset from 624,562 reads to 74,616 reads. Potential viral sequences were identified using **VirSorter2**, with the highest-scoring sequences selected for further analysis. Among them, NODE_1 (29,907 bp) had the highest score for RNA viruses (0.960), while NODE_15 (3,618 bp) was classified as a dsDNA phage with a score of 0.987. Only sequences with a **max_score > 0.9** were extracted for further analysis(n=30).

A `BLAST` search was conducted for the selected viral sequences against the NCBI nucleotide database. The search was limited to viral sequences taxid 10239(viruses) with publication dates between 1980 and January 2020(1980/01/01:2020/01/01[PDAT]). For NODE 1 29907 bp coverage 150.82x the best match was Bat SARS-like coronavirus isolate bat-SL-CoVZC45 MG772933.1 with 89.12 percent identity and 95 percent query coverage. The second closest match was Bat SARS-like coronavirus isolate bat-SL-CoVZXC21 MG772934.1 with 88.65 percent identity and 95 percent query coverage. For NODE 101 1980 bp coverage 5.56x the highest-scoring alignment was with Microviridae sp. isolate ctig683 MH617454.2 showing 83.58 percent identity with 76 percent query coverage. Other matches results are available here: https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Get&VIEW_RESULTS=FromRes&RID=X0GD8WM0013&UNIQ_OBJ_NAME=A_SearchResults_1ts08O_44tE_dmaMbXwu6FZ8_GTJ71_23kKoz&QUERY_INDEX=0
To further characterize NODE 1 annotation was performed using Prokka which identified multiple genes including replicase polyprotein 1a replicase polyprotein 1ab spike glycoprotein S membrane protein M and nucleoprotein N. The sequence was found to be in reverse orientation compared to the reference genome from GenBank and was reverse complemented. A subsequent comparison using UGENE confirmed that the gene structure matched the reference genome though some coordinates differed. The S gene spike glycoprotein 21568 to 25389 bp was extracted using samtools faidx and used as input for Primer BLAST to design primers for potential downstream applications. Results are available here:https://www.ncbi.nlm.nih.gov/tools/primer-blast/primertool.cgi?ctg_time=1741684341&job_key=p615UcCgzQjqNsgzxVPsAb9I_TOSW-Yukw

## **Discussion**

The results indicate that NODE_1 is closely related to bat SARS-like coronaviruses, suggesting it may represent a novel strain or a recombinant variant. The high sequence identity (89.12%) and gene annotation, including key structural proteins (S, M, N), further support its classification as a coronavirus. Differences in the structure of genes may be the key to understanding the invasiveness and pathogenicity of the coronavirus strain being studied in relation to humans. 

The presence of other NODEs may classify as contamination or a noise signal rather than a novel virus of interest(there was low similarity and no match cases to the patient's, which  clearly had a respiratory infection).

These findings emphasize the utility of metagenomic pipelines in viral discovery but also highlight the challenges of distinguishing true novel viruses from sequencing artifacts. Further analysis, such as phylogenetic reconstruction and functional validation, would be necessary to confirm the evolutionary significance of NODE_1. Additionally, the identification and extraction of the S gene enable the design of specific primers, which can aid in the development of diagnostic test systems for rapid virus detection. Understanding the genetic composition of NODE_1 could also provide insights for vaccine development, as spike glycoproteins play a crucial role in immune response and viral entry into host cells.
## **References**

1. Wu, F., Zhao, S., Yu, B. _et al._ A new coronavirus associated with human respiratory disease in China. _Nature_ **579**, 265–269 (2020). https://doi.org/10.1038/s41586-020-2008-3
2. Guarner, J. (2020). Three Emerging Coronaviruses in Two Decades. American Journal of Clinical Pathology, 153(4), 420–421. doi:10.1093/ajcp/aqaa029 
3. "Bad News Wrapped in Protein: Inside the Coronavirus Genome." _The New York Times_, April 3, 2020. [https://www.nytimes.com/interactive/2020/04/03/science/coronavirus-genome-bad-news-wrapped-in-protein.html](https://www.nytimes.com/interactive/2020/04/03/science/coronavirus-genome-bad-news-wrapped-in-protein.html)
