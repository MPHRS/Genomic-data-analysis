
original here: https://github.com/MPHRS/Genomic-data-analysis/blob/main/Project_2_antibiotic_resistance/report_2_antibiotic_resistance.md

---

# Bioinformatics Workflow for Variant Detection in Antibiotic-Resistant Bacteria
## Abstract

This study applied alignment and variant calling methods to investigate the mechanism of antibiotic resistance in _E. coli_ , strain K-12 substrain MG1655.  We used fastqc to inspect raw sequences, trimmomatic to filter, bwa for alignment and VasrScna with snpEff to identify relevant SNP. In the course of this study, several missense mutations were found in key resistance-related genes. These findings help identify key genetic changes associated with antibiotic resistance, providing valuable insights for optimizing treatment strategies.

## **Introduction**

Antibiotic resistance is a critical global health issue that is currently being actively researched and funded, as it reduces the effectiveness of antibacterial drugs at a rapid pace, leading to an increase in morbidity and mortality. Understanding the key changes in the bacterial genome helps to understand the molecular mechanisms of antibiotic resistance..[1]

_Escherichia coli_ is a bacterium that plays a pivotal role in microbiology. While it can induce severe infections in both humans and animals, it also constitutes a natural part of the microbiota in various hosts. A major concern is the potential for transferring virulent and resistant strains of _E. coli_ between animals and humans, a process that can occur via direct contact, exposure to animal waste, or through the food chain.

In addition, _E. coli_ acts as a significant reservoir for antibiotic resistance genes, which can compromise treatment efficacy in both human and veterinary medicine. In recent years, the prevalence of resistant strains has risen, with many acquiring resistance through horizontal gene transfer. Within the enterobacterial gene pool, _E. coli_ functions as both a donor and recipient of these resistance genes, thereby facilitating their spread among different bacterial populations. The challenge posed by antimicrobial resistance in _E. coli_ is recognized as a serious global public health issue affecting both human and animal health.[2,3]

In this study, we examined the raw sequence data of _Escherichia coli_ strain K-12 substrain MG1655 to identify critical genomic SNP.

## **Methods**

Genomic data were acquired from public repositories, including reference sequences (NCBI accession: GCF_000005845.2_ASM584v2) and paired-end shotgun sequencing were download manually. Initial quality control was performed using FastQC(v0.12.1). Trimmomatic(v0.39) was then applied with parameters such as ILLUMINACLIP(for Nextera transposase sequence), SLIDINGWINDOW:4:20, TRAILING:20, and MINLEN:36 to remove adapters and trim poor-quality bases.

Subsequent alignment of filtered reads to the reference genome was conducted using bwa-mem(v0.7.18-r1243-dirty), generating SAM files that were converted to BAM format with samtools(v1.21). The aligned sequences were sorted, indexed, and processed for variant calling. VarScan(v2.4.6) was used with a minimum variant frequency threshold of 0.8 to detect high-confidence SNPs. For functional annotation, the SnpEff(v5.2 ) tool was employed after building a custom database from GenBank files and related protein/CDS sequences. After that we extracted unique missense variants, which were then cross-referenced with protein function data from UniProt.
## **Results**


| Measure                           | Initial Forward Reads (amp_res_1.fastq) | Initial Reverse Reads (amp_res_2.fastq) | Filtered Forward Reads (amp_res_1P.fastq) | Filtered Reverse Reads (amp_res_2P.fastq) |
| --------------------------------- | --------------------------------------- | --------------------------------------- | ----------------------------------------- | ----------------------------------------- |
| Total Sequences                   | 455,876                                 | 455,876                                 | 376,010                                   | 376,010                                   |
| Total Bases                       | 46 Mbp                                  | 46 Mbp                                  | 34.7 Mbp                                  | 34.9 Mbp                                  |
| Sequences flagged as poor quality | 0                                       | 0                                       | 0                                         | 0                                         |
| Sequence length                   | 101                                     | 101                                     | 36-101                                    | 36-101                                    |
| %GC                               | 50%                                     | 50%                                     | 50%                                       | 50%                                       |

Quality control analysis using FastQC revealed poor per-sequence quality scores in both forward and reverse reads in the end, as well as the presence of Nextera transposase sequences. Adapter sequences were identified in both files. To improve the quality of the reads, Trimmomatic was used to remove adapter sequences, trim low-quality bases using a sliding window approach, and discard reads shorter than 36 bp. Post-filtering FastQC analysis confirmed the successful removal of adapters and improvement in quality distribution.

Mapping of reads to the _Escherichia coli_ K12 reference genome (GCF_000005845.2) was performed using BWA-MEM. A total of 751,993 reads (100%) were successfully mapped, with 99.77% of reads properly paired and only 0.00% classified as singletons. Variant calling was conducted using Samtools and VarScan 2.4.6, identifying six single nucleotide polymorphisms (SNPs). The transition/transversion ratio was found to be 0.5, with four transitions and eight transversions. SNP were visualize with IGV browser as it shown below:
![image_IGV](./images/IGV_close.png)

| Gene Name | Gene ID | Position | Ref | Alt | Protein Change |
| --------- | ------- | -------- | --- | --- | -------------- |
| _ftsI_    | b0084   | 93043    | C   | G   | p.Ala544Gly    |
| _acrB_    | b0462   | 482698   | T   | A   | p.Gln569Leu    |
| _mntP_    | b1821   | 1905761  | G   | A   | p.Gly25Asp     |
| _envZ_    | b3404   | 3535147  | A   | C   | p.Val241Gly    |

Annotation of variants using SnpEff v5.2 classified 91.8% of effects as MODIFIER, 6.6% as moderate, and 1.6% as low impact. The majority of identified variants (90.1%) were located in upstream or downstream regulatory regions, while 8.2% were exonic. Among coding effects, 80% were missense mutations(n=4). 

## **Discussion**

The sequencing data processed with multiple quality control and filtering steps. The initial analysis revealed adapter contamination and low-quality bases in the end, which were removed using Trimmomatic. This resulted in a reduction in total reads but improved overall quality, ensuring more reliable further analysis. The high mapping rate of 100% indicates that the sequencing reads aligned well with the reference genome, suggesting minimal contamination or sequencing errors.

The detected mutations  suggest multiple potential mechanisms of antibiotic resistance. The mutation in _ftsI_ (p.Ala544Gly) affects penicillin-binding protein 3 (PBP3), a key enzyme in peptidoglycan cross-linking. Alterations in this protein may reduce binding affinity for β-lactam antibiotics, leading to reduced susceptibility.

The _acrB_ mutation (p.Gln569Leu) affects the AcrAB-TolC efflux pump, which actively exports antibiotics out of the bacterial cell. Alterations in this pump may modify substrate specificity or increase efflux efficiency, potentially reducing the effectiveness of fluoroquinolones, tetracyclines, and chloramphenicol.

The _mntP_ mutation (p.Gly25Asp), involved in manganese efflux, could affect cellular metal homeostasis and indirectly influence antibiotic susceptibility, possibly impacting tetracyclines and fluoroquinolones. 

The _envZ_ mutation (p.Val241Gly) alters a regulatory protein involved in osmoregulation, which may affect porin expression and modify antibiotic uptake, reducing sensitivity to β-lactams and carbapenems.

While these findings suggest possible resistance mechanisms, additional biochemical validation is required to confirm their effects. 
## **References**

1.  World Health Organization. (2024). **Antimicrobial resistance**. Retrieved from [https://www.who.int/news-room/fact-sheets/detail/antimicrobial-resistance](https://www.who.int/news-room/fact-sheets/detail/antimicrobial-resistance)
2. Poirel L.Madec J.Lupo A.Schink A.Kieffer N.Nordmann P.Schwarz S.2018.Antimicrobial Resistance in Escherichia coli. Microbiol Spectr6:10.1128/microbiolspec.arba-0026-2017.https://doi.org/10.1128/microbiolspec.arba-0026-2017
3. Spratt BG. Distinct penicillin binding proteins involved in the division, elongation, and shape of Escherichia coli K12. Proc Natl Acad Sci U S A. 1975 Aug;72(8):2999-3003. doi: 10.1073/pnas.72.8.2999. PMID: 1103132; PMCID: PMC432906.

# Supplementary Materials

All commands and results information could be found here(lab_notebook):
https://github.com/MPHRS/Genomic-data-analysis/blob/main/Project_2_antibiotic_resistance/labnotebook_2.md#install-data
