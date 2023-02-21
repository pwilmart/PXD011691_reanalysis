# PXD011691_reanalysis

Reanalysis of data from [PXD011691](http://proteomecentral.proteomexchange.org/cgi/GetDataset?ID=PXD011691) - another DIA versus TMT experiment.

The data is from [this paper](https://pubs.acs.org/doi/abs/10.1021/acs.jproteome.8b00898):

> Muntel, J., Kirkpatrick, J., Bruderer, R., Huang, T., Vitek, O., Ori, A. and Reiter, L., 2019. Comparison of protein quantification in a complex background by DIA and TMT workflows with fixed instrument time. Journal of proteome research, 18(3), pp 1340-1351.

The paper compares 10 single shot DIA runs to a 10-fraction TMT-labeling experiment using the same biological replicate samples of a mouse brain background. The samples were run in two labs using similar LC and mass spec systems. Note: [human UPS2](https://www.sigmaaldrich.com/US/en/technical-documents/technical-article/protein-biology/protein-mass-spectrometry/ups1-and-ups2-proteomic) proteins were spiked-in to the mouse background (more on this below).

I would like to thank Michael Steidel for recently reminding me of this paper as a rather fair comparison of DIA and TMT quantitative methods. I had grabbed the TMT RAW files back in October 2019 and processed the data. I am guessing that I lost interest in the reanalysis because of the UPS2 proteins (more on that below). A recent [blog-like post](https://github.com/pwilmart/Human-plasma_DIA-vs-TMT) comparing DIA and TMT sparked interest in a data revisit.

---

## Contents:
- Disclaimers
- TMT Data
  - Proteome depths
  - Missing data
  - Comparison between sites
- DIA Data
  - Same metrics as TMT
- Compare DIA to TMT
  - Still thinking about this
- Conclusions

## Disclaimers

No paper is ever 100% right or 100% wrong. I liked many things about the data from this paper. Ten biological replicates of a mouse brain (cerebellum) background was great. I think the most can be learned from the unchanging background proteins when evaluating data processing and normalizations. Comparing single-shot DIA to a fractionated TMT experiment is a fair way to compare because they use the same total amount of instrument time. These are also the typical use cases for both quant methods. Another interesting aspect was to run the same sample preparations at two lab sites using very similar instruments and instrument methods.

It takes me many days (really) of effort to perform these reanalyses of published data. Being publicly critical of published work without doing a ton of careful work yourself is lazy and petty. The only fair thing is to pour over every detail in the paper and, if there is anything that could have been done better, to demonstrate that better results would have been obtained with the proposed changes. This can seem like a real "Reviewer 2" exercise, but it is not intended to be. The goal is not to single out the paper or its authors in a bad way. The goal is to show (hopefully) better data analysis methods so that the proteomics community can do even better work in the future. This is a tricky thread the needle situation and I apologize in advance if any feelings are hurt.     

---



## Map to files in the repository

File Name|Description
---|---
BGS-quant-table.txt|Prepped quant data table for BGS TMT data
BGS_peptide_results_9.txt|Detailed PSM-level summary table
FLI-quant-table.txt|Prepped quant data table for FLI TMT data
FLI_peptide_results_9.txt|Detailed PSM-level summary table
MSConvert_GUI_log.txt|RAW file conversion log file
PAW_IRS_log.txt|Combining BGS and FLI data with IRS log file
PAW_protein_grouper.log|Extended parsimony protein grouping log file
PAW_results.log|Basic parsimony protein grouping log file
PAW_table_descriptions_9.txt|Descriptions of the tables written by PAW pipeline
PXD011691_BGS_QC_checks.ipynb|Data quality control checks for BGS TMT data
PXD011691_FLI_QC_checks.ipynb|Data quality control checks for BGS TMT data
PXD011691_IRS_QC-checks.html|Data QC for combined BGS and FLI data
PXD011691_IRS_QC-checks.ipynb|Data QC for combined BGS and FLI data
PXD011691_IRS_edgeR-exact.html|Statistical testing of mouse background proteins
PXD011691_IRS_edgeR-exact.ipynb|Statistical testing of mouse background proteins
PXD011691_grouped_protein_summary_TMT_9.xlsx|Sheet for prepping the quantitative data
PXD011691_results_edgeR-exact.txt|Results of the testing of mouse background proteins
PXD11691_BGS_QC_checks.html|Data quality control checks for BGS TMT data
PXD11691_FLI_QC_checks.html|Data quality control checks for BGS TMT data
ThresholdFigures|Folder of delta-mass and conditional score histograms
add_TMT_intensities_log.txt|Protein summarization of reporter ions log file
comet.params|Comet search engine parameters file
grouped_peptide_summary_9.txt|Peptide summary after extended protein grouping
grouped_peptide_summary_TMT_9.txt|Peptide summary after extended protein grouping with reporter ions
grouped_protein_summary_9.txt|Protein summary after extended protein grouping
grouped_protein_summary_TMT_9.txt|Protein summary after extended protein grouping with reporter ions
labeled_grouped_protein_summary_TMT_9.txt|Protein summary after extended protein grouping with sample key
labeled_grouped_protein_summary_TMT_9_IRS_normalized.txt|IRS adjusted TMT data from BGS and FLI data
msn_files_PAW_log.txt|Data filtering (PSM FDR control) log file
peptide_summary_9.txt|Peptide summary after basic protein grouping
protein_summary_9.txt|Peptide summary after basic protein grouping
sqt_conversion_log.txt|Top hit summarization log file

---

Thank you for reading. <br> Phil Wilmarth <br> February 21, 2023
