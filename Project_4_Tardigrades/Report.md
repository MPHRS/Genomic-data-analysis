original here: https://github.com/MPHRS/Genomic-data-analysis/blob/main/Project_4_Tardigrades/Report.md
# Nuclear-Localized Proteins Identified from Chromatid-Derived Peptides in Ramazzottius varieornatus: Insights into DNA Protection Mechanisms



---

# Abstact
Tardigrades are known for their extreme resistance to environmental stress, yet the molecular basis of this resilience remains poorly understood. In this study, we performed tandem mass spectrometry on the chromatin fraction of Ramazzottius varieornatus and identified 64 proteins with 100% identity to predicted sequences. Several were predicted to localize to the nucleus, with two showing domain homology to proteins involved in intracellular transport and development. Notably, seven proteins lacked known homologs or signal peptides, marking them as promising candidates for further study into the unique genome-preserving mechanisms of tardigrades.
  
  
---
# Introduction
Tardigrades (Tardigrada) are microscopic invertebrates, typically measuring between 0.25 and 1 mm in length. They rely on water to maintain their hydrostatic skeleton and absorb oxygen through their cuticle. The phylum includes around 1,300 species inhabiting terrestrial, freshwater, and marine ecosystems. Although their phylogenetic position is debated, tardigrades are commonly associated with arthropods, though some studies suggest a closer relationship to nematodes.

They are best known for their ability to survive in dry and cold environments. The concept of cryptobiosis, which explains this ability, was first introduced in a 1959 review by David Keilin. Research has shown that their tolerance to extreme conditions, including radiation, correlates with desiccation resistance. However, a recent study did not confirm this correlation in organisms with likely similar mechanisms[1].

To investigate the radiation tolerance of tardigrades, mass spectrometry was performed on their chromatin fraction, identifying proteins with short sequence similarities. Among the proteins identified, 64 showed 100% identity. Nine lacked signal peptides, while two demonstrated homology to functional domains involved in endosomal transport and embryonic morphogenesis. The remaining seven proteins had no identifiable homologs in public databases.

These findings highlight several uncharacterized, nucleus-localized proteins as promising candidates for further investigation into the specific mechanisms responsible for maintaining genome integrity in tardigrades.

# Methods 

### Genome Acquisition and Repeat Masking  
The **genome assembly of *Ramazzottius varieornatus*** (accession: GCA_001949185.1_Rvar_4.0) was obtained in pre-assembled form from the NCBI database. To identify and mask repetitive elements, **RepeatModeler2 v2.0.3** ([DFam GitHub](https://github.com/Dfam-consortium/RepeatModeler)) was employed with default parameters.

### Gene Prediction  
Genome annotation was performed using **AUGUSTUS v3.5.0** ([AUGUSTUS website](https://bioinf.uni-greifswald.de/augustus/)). Default settings were used.

### Peptide Identification via Mass Spectrometry  
Short peptides were experimentally derived through **tandem mass spectrometry (MS/MS)**. The resulting peptide sequences were curated and are publicly available via [Yandex Disk](https://disk.yandex.ru/d/xJqQMGX77Xueqg).

### Peptide-Protein Mapping  
To map peptides to predicted proteins, a **custom BLAST database** was created from the annotated proteins. The peptides were aligned using **BLAST+ v2.13.0** in `blastp` mode ([NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi)). Matches with **100% identity** were retained for downstream analysis.

### Subcellular Localization Prediction  
Proteins matching MS-derived peptides were analyzed for subcellular localization using two independent tools:
- **TargetP 2.0** ([DTU Bioinformatics](https://services.healthtech.dtu.dk/service.php?TargetP-2.0))
- **WoLF PSORT** ([WoLF PSORT server](https://wolfpsort.hgc.jp/))

Proteins predicted to lack signal peptides and localize to the **nucleus** were selected for further characterization.

### Ortholog Identification  
To determine evolutionary conservation, orthologs of nuclear-localized proteins were searched using **BLAST+** against the **UniProtKB/Swiss-Prot** database ([UniProt](https://www.uniprot.org/)). *R. varieornatus* sequences were **excluded** from the query set to avoid self-matches and ensure ortholog detection from other species.

### Pfam Domain Prediction

In cases where orthologous sequences could not be identified through BLAST searches, protein function was predicted by identifying conserved protein domains and motifs. The HMMER web version (HMMER web server) was used to search the protein sequences against the Pfam database.

### Documentation  
All intermediate files, parameter settings, and custom scripts used for sequence processing, filtering, and analysis are archived and described in the **laboratory notebook** (see Supplementary materials).

---

# Results 

As a result of the search for short peptides derived from chromatid fractions of Ramazzottius varieornatus, 118 peptide sequences were identified in the annotated genome. Among these, 64 showed 100% identity with predicted proteins.

To eliminate random matches and false positives in nuclear localization prediction, the 64 matching proteins were filtered using WoLF PSORT and TargetP 2.0. This resulted in the identification of 4 and 9 candidate proteins, respectively. Besides 4 of them were filtered in a both way.

Among these sequences, two proteins showed homology with annotated proteins in public databases (see Table 1).

The g15484.t1 protein shares 45.03% identity with a gene from Danio rerio and is involved in retrograde transport from early and late endosomes to the late Golgi.

The g3428.t1 protein shares 56.6% identity with a gene from Caenorhabditis elegans and functions in regulating myosin II activity and organization during embryo elongation.

The g10514.t1 and g10513.t1 didn't show any result with blast search, but have nuclear localiztion due to WoLF and TagretP.

> Table 1. Predicted Proteins with 100% Identity to MS/MS Peptides and Homology to Known Proteins

| peptide ID | Best BLAST Hit (Annotation/E-Value)                                                                                                                        | Pfam Domains (E-Value)            | WoLF PSORT Localization (Top Score) | TargetP Prediction |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- | ----------------------------------- | ------------------ |
| g5641.t1   | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1") <br>5e-13    | CBM_14 (9.54e-09)                 | Extracellular (31)                  | SP                 |
| g15153.t1  | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>2e-14     | CBM_14 (1.81e-08)                 | Extracellular (32)                  | SP                 |
| g13530.t1  | -                                                                                                                                                          | -                                 | Extracellular (13)/Nuclear (6.5)    | SP                 |
| g14472.t1  | -         | -                                 | Nuclear (28)                        | OTHER              |
| g4106.t1   | -                                                                                                                                                          | -                                 | ER (14.5)                           | OTHER              |
| g10513.t1  | -                                                                                                                                                          | -                                 | Nuclear (20)                        | OTHER              |
| g3428.t1   | [Q09510.1](https://www.ncbi.nlm.nih.gov/protein/Q09510.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q09510.1")<br>9e-65     | EF-hand_7 (7.27e-06)              | Mitochondrial (18)                  | OTHER              |
| g5616.t1   | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>2e-14     | CBM_14 (8.34e-09)                 | Extracellular (31)                  | SP                 |
| g12562.t1  | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br><br>7e-13 | CBM_14 (3.45e-09)                 | Extracellular (30)                  | SP                 |
| g5443.t1   | -                                                                                                                                                          | -                                 | Extracellular (28)                  | OTHER              |
| g5502.t1   | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>6e-14     | CBM_14 (6.91e-09)                 | Extracellular (31)                  | SP                 |
| g5503.t1   | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>7e-14     | CBM_14 (7.84e-08)                 | Extracellular (29)                  | SP                 |
| g5467.t1   | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>4e-13     | CBM_14 (2.89e-08)                 | Extracellular (27)                  | SP                 |
| g5510.t1   | -                                                                                                                                                          | -                                 | Plasma membrane (23)                | OTHER              |
| g12510.t1  | -                                                                                                                                                          | MARVEL (0.00458)                  | Plasma membrane (29)                | OTHER              |
| g5237.t1   | -                                                                                                                                                          | -                                 | Plasma membrane (24)                | OTHER              |
| g1285.t1   | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>2e-12     | CBM_14 (2.74e-09)                 | Extracellular (25)                  | SP                 |
| g15484.t1  | [Q155U0.1](https://www.ncbi.nlm.nih.gov/protein/Q155U0.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q155U0.1")<br><br>0     | Vps51 (8.55e-22), COG2 (1.59e-10) | Nuclear (17.5)/Cytoplasm (15.3)     | OTHER              |
| g702.t1    | [P0DPW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DPW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DPW4.1")<br>1e-11     | CBM_14 (4.17e-07)                 | Extracellular (29)                  | SP                 |
| g10514.t1  | -                                                                                                                                                          | -                                 | Nuclear (19)                        | OTHER              |
| g3679.t1   | [Q19269.2](https://www.ncbi.nlm.nih.gov/protein/Q19269.2?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q19269.2")<br>7e-22     | Astacin (8.31e-32)                | Extracellular (26)                  | SP                 |

  

---

# Discussion

The initial analysis revealed that the majority of proteins identified through the peptide search are not directly associated with radiation resistance, as most are predicted to be localized outside the nucleus. In our study, only a subset of proteins exhibited nuclear localization signals, suggesting that these might be more likely to be involved in mechanisms of DNA protection or repair.

Interestingly, while several proteins showed high similarity to well-characterized proteins in public databases, a group of seven proteins did not produce any significant BLAST hits. These latter proteins contain peptide fragments detected by mass spectrometry and are predicted to be localized outside the nucleus according to WoLF PSORT and TargetP. It is plausible that these proteins are unique to the studied species and could underlie its remarkable resistance properties.

A detailed analysis of the four most relevant peptides with WoLF and TargetP sorting (summarized in Table 2) provides further insight:

For **g14472.t1**, BLAST analysis didn't show any hit, which may indicate that it is a novel protein without known homologs. Its sequence characteristics and predicted structure suggest a potential functional role in DNA interaction. 
However, when searched specifically against tardigrade sequences, it produced a significant hit with [P0DOW4.1](https://www.ncbi.nlm.nih.gov/protein/P0DOW4.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for P0DOW4.1")  (**e-value of 0**), confirming its presence in tardigrade datasets. This supports our hypothesis that it is a tardigrade-specific protein. Given these findings and supporting literature identifying **g14472.t1** as a tardigrade protein abd  recommended for experimental validation as potential DNA-binding proteins.[[Genomic-data-analysis/Project_4_Tardigrades/Report#Refernces| [1] ]]

For **g15484.t1**, the BLAST hit corresponds to a vacuolar protein sorting-associated protein 51 homolog ([Q155U0.1](https://www.ncbi.nlm.nih.gov/protein/Q155U0.1)) with an E-value of 0.0, signifying a strong match. Although this protein is typically involved in Golgi/endosomal trafficking. [[Genomic-data-analysis/Project_4_Tardigrades/Report#Refernces| [2] ]]

For **g10514.t1**, no significant BLAST hit was observed, and no Pfam domains were detected, which underlines its novelty. Its predicted nuclear localization (WoLF PSORT score of 19) makes it a potential candidate for tardigrade-specific DNA-binding activities. Its unique characteristics needed further investigation.

No, **g3428.t1** is not of primary interest. Although it shows a strong BLAST hit to a myosin regulatory light chain and contains an EF-hand domain, its function is related to calcium signaling and cytoskeletal regulation. Its predicted mitochondrial localization also makes it unlikely to be involved in nuclear DNA protection.

Overall, our findings highlight a small subset of candidate proteins with potential roles in DNA protection and repair, particularly **g14472.t1** and **g10514.t1**, which show nuclear localization and, in the case of **g14472.t1**, direct evidence of involvement in DNA damage suppression. The presence of unidentified proteins with no known homologs suggests the existence of tardigrade-specific mechanisms that may contribute to their extraordinary resilience.



> Table 2. Most relevant peptides

| Peptide ID | Best BLAST Hit (E-Value)                                                                                                                               | Pfam Domains         | WoLF PSORT Localization | TargetP |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------- | ----------------------- | ------- |
| g14472.t1  | -     | -                    | Nuclear (28)            | OTHER   |
| g15484.t1  | [Q155U0.1](https://www.ncbi.nlm.nih.gov/protein/Q155U0.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q155U0.1")<br><br>0 | Vps51 (8.55e-22)     | Nuclear (17.5)          | OTHER   |
| g10514.t1  | -                                                                                                                                                      | -                    | Nuclear (19)            | OTHER   |
| g3428.t1   | [Q09510.1](https://www.ncbi.nlm.nih.gov/protein/Q09510.1?report=genbank&log$=prottop&blast_rank=1&RID=Z89UZCH9016 "Show report for Q09510.1")<br>9e-65 | EF-hand_7 (7.27e-06) | Mitochondrial (18)      | OTHER   |




# Refernces
1) Beblo-Vranesevic K., Bohmeier M., Perras A.K., Schwendner P., Rabbow E., Moissl-Eichinger C., Cockell C.S., Vannier P., Marteinsson V.T., Monaghan E.P., et al. Lack of correlation of desiccation and radiation tolerance in microorganisms from diverse extreme environments tested under anoxic conditions. FEMS Microbiol. Lett. 2018;365 doi: 10.1093/femsle/fny044.
2) Hashimoto T, Horikawa DD, Saito Y, Kuwahara H, Kozuka-Hata H, Shin-I T, Minakuchi Y, Ohishi K, Motoyama A, Aizu T, Enomoto A, Kondo K, Tanaka S, Hara Y, Koshikawa S, Sagara H, Miura T, Yokobori SI, Miyagawa K, Suzuki Y, Kubo T, Oyama M, Kohara Y, Fujiyama A, Arakawa K, Katayama T, Toyoda A, Kunieda T. Extremotolerant tardigrade genome and improved radiotolerance of human cultured cells by tardigrade-unique protein. Nat Commun. 2016 Sep 20;7:12808. doi: 10.1038/ncomms12808. PMID: 27649274; PMCID: PMC5034306.
3) Ho SY, Lorent K, Pack M, Farber SA. Zebrafish fat-free is required for intestinal lipid absorption and Golgi apparatus structure. Cell Metab. 2006 Apr;3(4):289-300. doi: 10.1016/j.cmet.2006.03.001. PMID: 16581006; PMCID: PMC2247414.
4) UniProt Consortium. _Myosin regulatory light chain – Caenorhabditis elegans (Q09510)_. UniProt. Available at: [https://www.uniprot.org/uniprotkb/Q09510/entry](https://www.uniprot.org/uniprotkb/Q09510/entry) (accessed April 9, 2025).

# Supplementary material

lab notebook is available here: https://github.com/MPHRS/Genomic-data-analysis/blob/main/Project_4_Tardigrades/Lab_noteebook.md


