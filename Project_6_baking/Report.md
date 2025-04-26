# authors:
Seraph Dobrovolskii,
Ivan Mikhniuk

---

original here:
https://github.com/MPHRS/Genomic-data-analysis/blob/main/Project_6_baking/Report.md

---

# **Abstract**


---

# **Introduction**



# **Methods**
The analysis utilized publicly available RNA-seq data from Saccharomyces cerevisiae fermentation experiments (SRA accessions SRR941816-SRR941819), which represent two biological replicates at 0-minute and 30-minute fermentation time points. The reference genome for strain S288C (assembly GCF_000146045.2_R64) and its annotation GFF file were obtained from NCBI, with additional genomic context provided by the Saccharomyces Genome Database (SGD).

Read alignment was performed using HISAT2 (v2.2.1) with default parameters after genome indexing. Resulting BAM files were sorted via SAMtools (v1.15). Gene expression quantification was conducted using featureCounts from the Subread package (v2.0.8) with the -g gene_id parameter. Prior to analysis, GFF annotations were converted to GTF format using gffread (v0.12.7), followed by attribute corrections based on guidelines from a public Colab notebook.

Differential expression analysis was implemented in R (v4.3.1) using the DESeq2 package (v1.38.3), including normalization and statistical processing stages. Functional annotation of the top 50 differentially expressed genes was performed via the GO Slim Mapper online tool with default settings. Results were visualized using custom R scripts provided in the supplementary materials.

The complete analysis scripts and intermediate files are available in the laboratory journal.


# **Results**
The total number of reads per sample ranged from 1.7 to 9.9 million, with the proportion of successfully aligned reads ranging from 94.3% to 96.2%. In all samples, more than 87% of reads were mapped to a single genomic location, while the proportion of reads not aligned to any position did not exceed 6%.

During the work, 50 genes with significant differential expression were selected and ranked by p-value. Functional annotation allowed for their distribution into Gene Ontology (GO) categories. For each category, both the number of genes present among the selected 50 and their total representation in the annotated genome were calculated. Summary data are presented in Table 1.

Table 1. Distribution of differentially expressed genes by GO categories.
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

The analysis showed that a significant portion of the differentially expressed genes are related to fundamental cellular metabolic processes, including ribosome biogenesis, transcription, and substance transport.
Genes involved in carbohydrate metabolism are of particular interest. Specifically:

Carbohydrate metabolic process (GO:0005975)

    YBR105C — PGI1 — Phosphoglucose isomerase, a key enzyme in glycolysis

    YER062C — HXK2 — Hexokinase 2, catalyzes the first step in glycolysis

    YKR097W — PTS1 — Phosphotriose isomerase, an intermediate step in glycolysis

    YOL136C — RGT2 — Glucose sensor on the membrane, involved in regulating the shift to fermentation

Carbohydrate transport (GO:0008643)

    YDR536W — STL1 — Sugar transporter

    YHR094C — HXT1 — Low-affinity glucose transporter (important at high sugar concentrations, typical for fermentation)

Generation of precursor metabolites and energy (GO:0006091)

    YOL136C — (repeated) — RGT2, as mentioned above; indirectly influences through the regulation of HXT group expression.

Monocarboxylic acid metabolic process (GO:0032787)

    YOL136C — (again) — RGT2, indirectly related through the metabolism of glycolysis products.

Five out of the six analyzed genes, some of which are simultaneously involved in multiple processes, demonstrated a statistically significant increase in expression (log2FoldChange > 2.5, padj < 0.001), while the YKR097W gene showed the opposite trend with a marked decrease in expression levels (Fig. 1).

![image](https://github.com/user-attachments/assets/dd05fba3-1cd9-4df3-8258-5d2c712b9048)


  
# **Discussion**
Анализ ключевых генов, связанных с углеводным метаболизмом и транспортом, выявил их взаимосвязь в контексте адаптации клетки к изменению доступности глюкозы.

PGI1 (YBR105C) – фосфоглюкоизомераза, играет центральную роль в гликолизе и пентозофосфатном пути. Повышенная экспрессия (log2FoldChange > 2.5) указывает на усиление катаболизма глюкозы

HXK2 (YER062C) – гексокиназа 2, выполняет двойную функцию: катализирует фосфорилирование глюкозы и регулирует транскрипцию генов через взаимодействие с репрессором Mig1
. Высокая экспрессия согласуется с активацией гликолиза и подавлением альтернативных метаболических путей (например, дыхания) при избытке глюкозы


PTS1 (YKR097W) – фосфотриозоизомераза, участвует в промежуточных этапах гликолиза. Снижение экспрессии (log2FoldChange < -2.5) может отражать компенсаторный механизм, направленный на балансировку потока метаболитов или переключение на альтернативные пути (например, пентозофосфатный)


RGT2 (YOL136C) – мембранный сенсор глюкозы, активирующий экспрессию низкоаффинных транспортеров (например, HXT1) при высоких концентрациях сахара
. Его повышенная экспрессия согласуется с переходом клетки к ферментативному метаболизму


STL1 (YDR536W) и HXT1 (YHR094C) – транспортеры сахаров. STL1, предположительно, участвует в импорте альтернативных субстратов
, тогда как HXT1 обеспечивает массовый транспорт глюкозы в условиях её избытка. Их активация (log2FoldChange > 2.5) поддерживает гипотезу о переключении на брожение.

# **Supplementary materials**


# References

