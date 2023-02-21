# PXD011691_reanalysis

Reanalysis of data from [PXD011691](http://proteomecentral.proteomexchange.org/cgi/GetDataset?ID=PXD011691) - another DIA versus TMT experiment.

The data is from [this paper](https://pubs.acs.org/doi/abs/10.1021/acs.jproteome.8b00898):

> Muntel, J., Kirkpatrick, J., Bruderer, R., Huang, T., Vitek, O., Ori, A. and Reiter, L., 2019. Comparison of protein quantification in a complex background by DIA and TMT workflows with fixed instrument time. Journal of proteome research, 18(3), pp 1340-1351.

The paper compares 10 single shot DIA runs to a 10-fraction TMT-labeling experiment using the same biological replicate samples of a mouse brain background. The samples were run in two labs using similar LC and mass spec systems. Note: [human UPS2](https://www.sigmaaldrich.com/US/en/technical-documents/technical-article/protein-biology/protein-mass-spectrometry/ups1-and-ups2-proteomic) proteins were spiked-in to the mouse background (more on this below).

## Disclaimers 

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

