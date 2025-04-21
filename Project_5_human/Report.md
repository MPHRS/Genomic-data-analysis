**authors: Ivan Mikhniuk, Seraphim Dobrovolskii**

original here: https://github.com/MPHRS/Genomic-data-analysis/blob/main/Project_5_human/Report.md

# H+: Engineering the Perfect Human with CRISPR and Personal Genomics


---

# Abstact



---

# Introduction 

Technologies such as DNA microarrays and CRISPR-Cas9 are not novel and nowadays they have become widely accessible and deeply integrated into both research and consumer applications. Large-scale adoption of genotyping services has led to the creation of vast SNP databases, enabling population-wide association studies and trait predictions. With the growing understanding of how specific genetic variants influence traits, it is now possible to envision the use of gene-editing tools like CRISPR not only for treating monogenic diseases but also for the targeted enhancement of complex organism traits, using data from sources like VCF files to guide precise genetic interventions [1].

DNA microarrays, also known as genotyping chips consist of DNA fixed on a solid surface. They are used to quickly and cost-effectively identify specific genetic variants( single nucleotide polymorphisms - SNPs). The process involves hybridizing a target DNA or RNA sample to these probes under controlled conditions. Fluorescent or other detectable signals indicate which probes have bound to the target, revealing the presence of specific genetic variants. While microarrays do not provide full genome coverage, they focus on clinically and evolutionarily relevant regions, enabling efficient genotyping and disease-associated SNP identification. [3] CRISPR-Cas9, on the other hand, allows for precise modification of these loci and has already demonstrated therapeutic potential in correcting pathogenic mutations in vivo [4].

In this study, we process VCF files from 23andMe, perform comprehensive SNP annotation(including sex determination and eye‑color prediction) and then select the clinically relevant variants for further analysis.


# Methods

For this study, we used VCF-format genotype data provided by our teacher made by Genotek and 23andMe. The reference genome assembly used for initial alignment was hg19. To ensure compatibility with current genomic databases and tools, the Genotek dataset was lifted over to the latest human genome assembly (hg38) using the web-based Assembly Converter tool provided by Ensembl ([https://useast.ensembl.org/Homo_sapiens/Tools/AssemblyConverter](https://useast.ensembl.org/Homo_sapiens/Tools/AssemblyConverter)).

Mitochondrial haplogroups were determined using the online tool mthap (v0.19c) ([https://dna.jameslick.com/mthap/](https://dna.jameslick.com/mthap/)). Y-chromosome haplogroups were inferred using CladeFinder (v1.0) ([https://cladefinder.yseq.net/](https://cladefinder.yseq.net/)) in combination with Y-linked SNP data from the 23andMe dataset. 

Eye color prediction was based on SNPs identified in a publication [1], which reports variants known to be associated with pigmentation. In addition to that the "HIrisPlex-S Eye, Hair and Skin Colour DNA Phenotyping Webtool" were used to double-check eye color [2]. Additional SNPs related to traits of interest were extracted from the original genotype files and annotated using publicly available databases, including SNPedia and dbSNP.

  

# Results

During the analysis of single nucleotide polymorphisms (SNPs), the sample showed the highest similarity with mitochondrial haplogroup H(T152C). A total of 3,270 markers were identified at 3,268 positions, covering 19.7% of the mitochondrial DNA (mtDNA). However, it is important to note that this list of markers is almost certainly incomplete due to the limitations of genotyping technologies, and thus, it is not comparable to full mtDNA sequencing results. The sex of the individual was determined based on the presence of a Y chromosome. The most probable Y-DNA haplogroup is R-M417.

Out of the five SNPs commonly analyzed in literature to predict eye color, only three were present in the dataset. Based on these, we can confidently exclude blue eye color as a possibility( so far we have "rs12913832  A  G  .  .  .  GT  0/1"), but a more precise prediction is not possible without the remaining markers. In addition to that the "HIrisPlex-S Eye, Hair and Skin Colour DNA Phenotyping Webtool" model strongly predicts brown eyes (p = 0.812, AUC loss = 0.006), with intermediate (p = 0.122) and blue (p = 0.065) eye colors being less likely.

A selection of the most interesting SNPs (six in total) were identified and annotated using SNPedia and dbSNP(see Table 1). 4 out of 6 phenotypic traits can be potentially improved by CRISPR-Cas9.

Additionally, the genotype suggests a high likelihood of lactose intolerance in adulthood. The individual carries the SNP, which is associated with normal sleep needs — typically requiring around 8 hours of sleep to feel well-rested.

The individual also carries the reference allele for a polymorphism that influences earwax type and body odor, indicating a wet-type earwax and a perceivable body odor.

Furthermore, there is a low genetic predisposition to alcohol and opioid dependence, as well as to obesity and type 2 diabetes.

Table 1. Teachers SNPs and Their Phenotypic Associations

|   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|
|SNV ID (rsID)|Chromosome:Position|Genotype|EUR Frequency|Gene(s)|Protein Consequence|Phenotypic Effect|Additional Information|
|rs4988235|chr2:135851076|G/G|0.456934|MCM6|Intron Variant|Likely lactose intolerant in adulthood|Can digest milk if genotype is (T;T)|
|rs121912617|chr12:26122364|G/G|0.99993|BHLHE41, SSPN|Missense, Intron Variant|Needs full 8h of sleep|(G;G) typical; (A;A) → short sleeper phenotype|
|rs17822931|chr16:48224287|C/C|0.877269|ABCC11|Missense Variant|Wet earwax, normal body odour, normal colostrum|(T;T) → dry earwax, no body odour; common in East Asians|
|rs4680|chr22:19963748|G/A|0.491334|COMT, MIR4761|Missense, Upstream Variant|Intermediate dopamine levels|(A;A) → higher dopamine, more exploratory, lower pain threshold|
|rs1799971|chr6:154039662|A/A|0.867641|OPRM1|Missense Variant|Normal response to alcohol|May influence opioid response (e.g., heroin, morphine)|
|rs9939609|chr16:53786615|T/T|0.58975|FTO|Intron Variant|Lower risk of obesity and type 2 diabetes|(A;A) associated with increased obesity risk|

  

# Discussion

  

Individuals homozygous for the ancestral G allele (G/G, equivalent to C/C in alternative strand notation) at the rs4988235 locus typically exhibit a post-weaning decline in LCT gene expression. This phenotype results from the inability of a cis-regulatory element located in intron 13 of the MCM6 gene to sustain LCT transcriptional activity beyond early childhood. The regulatory variant disrupts enhancer function necessary for lactase persistence, leading to a gradual developmental downregulation of lactase production. 

# References

1. Hart KL, Kimura SL, Mushailov V, Budimlija ZM, Prinz M, Wurmbach E. Improved eye- and skin-color prediction based on 8 SNPs. Croat Med J. 2013 Jun;54(3):248-56. doi: 10.3325/cmj.2013.54.248. PMID: 23771755; PMCID: PMC3694299.
2. https://hirisplex.erasmusmc.nl/
3.  Xu, J., Chun, H., Wang, L., Mei, H., Chen, S., & Huang, X. (2024). DNA microarray chips: Fabrication and cutting-edge applications. _Chemical Engineering Journal, 499_, 155937. [https://doi.org/10.1016/j.cej.2024.155937](https://doi.org/10.1016/j.cej.2024.155937)
4. Tyumentseva, M.; Tyumentsev, A.; Akimkin, V. CRISPR/Cas9 Landscape: Current State and Future Perspectives. _Int. J. Mol. Sci._ **2023**, _24_, 16077. https://doi.org/10.3390/ijms242216077
    

# Supplementary material

Lab notebook could be found here: https://github.com/MPHRS/Genomic-data-analysis/blob/main/Project_5_human/Lab_notebook.md

Table below is a draft.  
  

|   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|
|SNV ID (rsID)|Chromosome:Position|Ref/Alt Genotype|Population Frequency (EUR)|Gene|Variant Type|Protein Consequence|Phenotypic Effect|Annotation Source|Additional Information|
|rs4988235|chr2:135851076|G/G|0.456934|MCM6|SNV|Intron Variant|Likely lactose intolerant in adulthood|SNPedia / dbSNP|Can digest milk if genotype is (T;T)|
|rs121912617|chr12:26122364|G/G|0.99993|BHLHE41, SSPN|SNV|Missense Variant, Intron Variant|Normal sleep pattern; needs 8h sleep|SNPedia / dbSNP|(G;G) is typical, (A;A) short sleeper mutation|
|rs17822931|chr16:48224287|C/C|0.877269|ABCC11|SNV|Missense Variant|Wet earwax, normal body odour, normal colostrum|SNPedia / dbSNP|(T;T) leads to dry earwax, no body odour; common in East Asians|
|rs4680|chr22:19963748|G/A|0.491334|COMT, MIR4761|SNV|Missense Variant, 2KB Upstream Variant|Intermediate dopamine levels|SNPedia / dbSNP|(A;A) — lower COMT activity, higher dopamine, more exploratory, lower pain threshold|
|rs1799971|chr6:154039662|A/A|0.867641|OPRM1|SNV|Missense Variant|Typical response to alcohol|SNPedia / dbSNP|May influence opioid response (heroin, codeine, morphine)|
|rs9939609|chr16:53786615|T/T|0.58975|FTO|SNV|Intron Variant|Lower risk of obesity and type 2 diabetes|SNPedia / dbSNP|(A;A) is associated with higher obesity risk|

  
  
  
  
  

1. Hart KL, Kimura SL, Mushailov V, Budimlija ZM, Prinz M, Wurmbach E. Improved eye- and skin-color prediction based on 8 SNPs. Croat Med J. 2013 Jun;54(3):248-56. doi: 10.3325/cmj.2013.54.248. PMID: 23771755; PMCID: PMC3694299.
    

  
  

|   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|
|SNV ID (rsID)|Хромосома:Позиция|Реф/Альт|Частота в популяции (EUR)|Ген|Тип варианта|Последствия на белок|эффект|Источник аннотации|Доп. информация|
|rs4988235|chr2 135851076|G/G|0.456934|[MCM6](https://www.snpedia.com/index.php/MCM6)|SNV|Intron Variant|likely to be lactose intolerant as an adult|snpedia/dbSNP|can digest milk [(T;T)](https://www.snpedia.com/index.php/Rs4988235\(T;T\))|
|rs121912617|chr12 26122364|G/G|0.99993|[BHLHE41](https://www.snpedia.com/index.php/BHLHE41), [SSPN](https://www.snpedia.com/index.php/Special:FormEdit/Gene/SSPN)|SNV|Missense Variant,<br><br>Intron Variant|normal. Need a full 8h of sleep|snpedia/dbSNP|[(G;G)](https://www.snpedia.com/index.php/Rs121912617\(G;G\)) Short sleeper|
|rs17822931|chr16 48224287|C/C|0.877269|ABCC11|SNV|Missense Variant|Wet earwax. Normal body odour. Normal colostrum.|snpedia/dbSNP|Dry earwax. No body odour. Likely Asian ancestry. Reduced colostrum. [(T;T)](https://www.snpedia.com/index.php/Rs17822931\(T;T\))|
|rs4680|chr22 19963748|G/A|0.491334|[COMT](https://www.snpedia.com/index.php/COMT), MIR4761|SNV|Missense Variant, 2KB Upstream Variant|Intermediate dopamine levels, other effects|snpedia/dbSNP|[(A;A)](https://www.snpedia.com/index.php/Rs4680\(A;A\))more exploratory, lower COMT enzymatic activity, therefore higher dopamine levels; lower pain threshold, enhanced vulnerability to stress, yet also more efficient at processing information under most conditions|
|rs1799<br><br>971|chr6<br><br>154039662|A/A|0.867641|[OPRM1](https://www.snpedia.com/index.php/OPRM1)|SNV|Missense Variant|normal(alcohol)|snpedia/dbSNP|This SNP may also influence the response to opioids such as [heroin](https://www.snpedia.com/index.php?title=Heroin&action=edit&redlink=1), [codeine](https://www.snpedia.com/index.php/Codeine) or [morphine](https://www.snpedia.com/index.php/Morphine).|
|rs9939609|chr16 53786615|T/T|0.58975|[FTO](https://www.snpedia.com/index.php/FTO)|SNV|Intron Variant|lower risk of obesity and Type-2 diabetes|snpedia/dbSNP||

  