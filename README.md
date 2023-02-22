# PXD011691_reanalysis

Reanalysis of data from [PXD011691](http://proteomecentral.proteomexchange.org/cgi/GetDataset?ID=PXD011691) - another DIA versus TMT experiment.

The data is from [this paper](https://pubs.acs.org/doi/abs/10.1021/acs.jproteome.8b00898):

> Muntel, J., Kirkpatrick, J., Bruderer, R., Huang, T., Vitek, O., Ori, A. and Reiter, L., 2019. Comparison of protein quantification in a complex background by DIA and TMT workflows with fixed instrument time. Journal of proteome research, 18(3), pp 1340-1351.

The paper compares 10 single shot DIA runs to a 10-fraction TMT-labeling experiment using the same biological replicate samples of a mouse brain background. The samples were run in two labs using similar LC and mass spec systems. Note: [human UPS2](https://www.sigmaaldrich.com/US/en/technical-documents/technical-article/protein-biology/protein-mass-spectrometry/ups1-and-ups2-proteomic) proteins were spiked-in to the mouse background (more on this below).

I would like to thank Michael Steidel for recently reminding me of this paper as a rather fair comparison of DIA and TMT quantitative methods. I had grabbed the TMT RAW files back in October 2019 and processed the data. I am guessing that I lost interest in the reanalysis because of the UPS2 proteins (more on that below). A recent [blog-like post](https://github.com/pwilmart/Human-plasma_DIA-vs-TMT) comparing DIA and TMT sparked interest in a data revisit.

---

## Contents:
- Disclaimers
- Paper review
- TMT Data
  - Processing overview
  - Proteome depths/overlap
  - Missing data
  - Data quality
  - Comparison between sites
- DIA Data
  - Same metrics as TMT
- Compare DIA to TMT
  - Still thinking about this
- Conclusions

---

## Disclaimers

No paper is ever 100% right or 100% wrong. I liked many things about the data from this paper. Ten biological replicates of a mouse brain (cerebellum) background was great. I think the most can be learned from the unchanging background proteins when evaluating data processing and normalizations. Comparing single-shot DIA to a fractionated TMT experiment is a fair way to compare because they use the same total amount of instrument time. These are also the typical use cases for both quant methods. Another interesting aspect was to run the same sample preparations at two lab sites using very similar instruments and instrument methods.

It takes me many days (really) of effort to perform these reanalyses of published data. Being publicly critical of published work without doing a ton of careful work yourself is lazy and petty. The only fair thing is to pour over every detail in the paper and, if there is anything that could have been done better, to demonstrate that better results would have been obtained with the proposed changes. This can seem like a real "Reviewer 2" exercise, but it is not intended to be. The goal is not to single out the paper or its authors in a bad way. The goal is to show (hopefully) better data analysis methods so that the proteomics community can do even better work in the future. This is a tricky thread the needle situation and I apologize in advance if any feelings are hurt.

Everything below are my personal opinions.

A comma is used as the thousands separator in numbers below (USA convention).    

---

## Paper Review

I will summarize important positive and negative things from the publication that are directly relevant to the data reanalysis. At this point in time I have done just the TMT data, so I will focus on those parts of the paper now and add more on DIA later. The sample prep seems good. The samples were processed in one lab and the final samples were run on LC-MS platforms at two lab sites (Biognosys (BGS) and Fritz Lipmann Institute (FLI)).  For the TMT samples, the common sample processing involved trypsin digestion, TMT 10-plex labeling, mixing labeled samples, and first dimension fractionation into 10 fractions.

The samples were 10 commercial mouse cerebellum samples (biological replicates). Human UPS2 proteins were spiked in at low levels. There were 5 UPS2 dilutions done in duplicate. The analytical platform had different (but similar) LC systems and the same model mass specs (Thermo Orbitrap Fusion Lumos Tribrids). The same instrument method files were used at both sites.

DIA data was generated in 10 single shot runs. A 6-fraction DDA analysis of a pooled sample was used for library creation with MaxQuant. DIA data was collected in a 2-hour run with 40 variable width MS2 windows. DIA data was analyzed with Spectronaut Pulsar X in both library and library-free modes. A common mouse Swiss-Prot FASTA file was used for library building or the library-free direct DIA analysis.

The TMT data was generated using the SPS-MS3 method on the Lumos Tribrids. Instrument acquisition settings were the same. The second dimension low-pH reverse runs were 2 hours long. The TMT data was analyzed with Proteome Discoverer 2.2 using Mascot and the mouse Swiss-Prot FASTA file (about 17K sequences from 2016). Mascot settings were typical narrow (10 ppm) precursor tolerance and 0.5 Da for the ion trap MS2 fragments. Percolator post processing was used for PSM error control. Reporter ions were PD default of signal-to-noise ratios. Minimum peptide length was probably 6 (a PD default). Unique peptides only were used for quant. Some protein inference and error control option in PD (or maybe is was IDPicker post-processing the PSM export from PD?) must have been used but was not detailed.

The sample to TMT channel key was not provided. The main summary tables in the manuscript have reporter ion median values as the protein summary values. I do not think this is an option in PD, so post-processing of the PD lower level data must have been done (details not provided). The PXD011691 repository had the RAW files and the PD MSF files. The MSF files are SQLite3 relational databases and meet the repository requirements. However, MSF files are intermediate files in PD since 2.x and cannot be viewed by newer PD viewers. No details about the PD analysis beyond what was described in the paper could be extracted.

Much of the paper presents an analysis of the UPS2 proteins and mostly ignores the mouse background proteins. The UPS spike-in is not a great choice. The UPS proteins are human and they have a lot of sequence homology to their respective mouse orthologs. This creates additional shared peptide complications. UPS2 has 48 proteins in groups of 8 spanning 5 decades of concentration difference. On top of the built-in dilutions of UPS2 proteins, the UPS2 proteins were added to the mouse background in 5 different dilutions (just 2 replicates of each dilution). The dilution squared effect and overall low levels of UPS2 spike-in resulted in detection of only 12-19 UPS proteins across the DIA and TMT data. The low levels of UPS2 proteins and insufficient replicates for proper analysis make the UPS2 spike-in data of extremely limited utility. It is better to avoid this distraction and focus on the characteristics of the mouse background proteins.

## TMT data Reanalysis

### Processing Overview

Many aspects of the TMT processing in the publication are problematic. Signal-to-noise ratios are not a proper unit of measurement, mouse Swiss-Prot FASTA file is incomplete, Mascot is not great for ion trap MS2 spectra, and how the data was summarized for protein level quant. It made sense to process the data through [my pipeline](https://github.com/pwilmart/PAW_pipeline) where the wrinkles of TMT data processing have been ironed out in 7+ years of use.

Here are some of the parameter choices that make improvements:
- more complete FASTA file (21K sequences vs 17K)
- minimum peptide length of 7 amino acids
- [wide precursor tolerance](https://pwilmart.github.io/blog/2021/04/22/Parent-ion-tolerance)
- Comet search engine is more sensitive
- accurate/sensitive PSM FDR control
- two peptide per protein per plex rule
- extended parsimony protein grouping
- reporter ion peak heights (intensities)
- reporter ion summing for protein quant
- missing value handling

What|BGS|FLI|PAW BGS|PAW FLI|Gain
---|---|---|---|---|---
MS2 scans|384,156|470,691|384,156|470,691
1% FDR Scans|114,371|125,425|143,285|151,071|23%
PSM ID rate|29.8%|26.6%|37.3%|32.1%
Peptides|65,837|65,633|67,206|68,212|3%
Proteins<br>2 peptides/protein|5,938|5,837|6,319|6,271|7%

I am not sure that peptide sequence counting was done the same way (I don't usually count peptides). It is clear that the PAW processing is better.

The PAW pipeline breaks the FDR analysis into peptide subclasses (deltamass windows, charge state, modification state) and it can be fun to see what fractions of the PSMs fall into the different subclasses.

Charge|Fraction
---|---
2+|46.6%
3+|45.1%
4+|8.3%

TMT labels shift peptide charge state distribution to higher charges.

Delta Mass Region|Fraction
---|---
0-Da narrow window|88.1%
1-Da narrow window|9.5%
Outside narrow windows|2.4%

Most peptides have very good agreement between measured and predicted masses. There are quite a few PSMs with deamidated Asn and with C13 triggers. There are also many PSMs with inaccurate masses.

Charge|Fraction with M+16
---|---
2+|18.3%
3+|21.5%
4+|23.2%
All|20.1%

Oxidized Met is an important variable PTM to add. Note that PSM counts inflate how much sample seems oxidized. Intensity-based estimates would be smaller.

### Proteome Depth and Overlap Between Sites

How you count things when you compare things is of critical importance. If we were to look at the data from each site separately, the PAW pipeline would require two peptides per protein. That would give the protein ID numbers above. When data from multiple TMT plexes are processed in the PAW pipeline, you want to take all of the 1% FDR filtered files and do a collective protein inference. The more PSMs we use, the more information we can bring to the inference logic. This also gives peptide and protein summary files with all of the data in one place (we do not have to merge tables). The protein ID criteria for the combined BGS and FLI data is two peptides per protein per plex. Proteins do not have to have 2 peptide in **both** plexes. This relaxes things slightly. The PAW pipeline also removes (zeroes out) some very low intensity reporter ion on a scan basis. We end up with a few proteins that meet the ID criteria but have no associated reporter ion data. We have a few more proteins that we can ID than we can quantify.

When we want to talk about proteome depth and overlap between sites, it makes more sense to count quantifiable proteins (we are doing quantitative proteomics, right?). Wait a minute. Since we have quantities for each protein, should be count proteins or count total intensities? Yes. The union of all protein identifications from both sites was 6,796.

Site|Quantifiable Proteins|Average Intensity per Sample
---|---|---
BGS|6,539|19,328,018,191
FLI|6,504|42,620,258,349

We have a few more proteins in the BGS data even though the intensities are less than half compared to the FLI data. Now we can ask of the proteins seen at each site, how many were unique to that site and how many were seen at the other site.

Site|Quantity|Seen at both sites|Unique to site
---|---|---|---
BGS|Protein Count|6,270|269
BGS|Count Percentages|95.9%|4.1%
BGS|Protein Intensity|19,308,751,470|19,266,721
BGS|Intensity Percentages|99.9%|0.1%
FLI|Protein Count|6,504|234
FLI|Count Percentages|96.4%|3.6%
FLI|Protein Intensity|42,584,284,686|35,973,663
FLI|Intensity Percentages|99.9%|0.1%

If we count by number of proteins identified rather than number of proteins that are quantifiable, the percentage in the overlap is lower (about 92%). Restricting the counts to quantifiable proteins gets the overlap up to 96%, but plain counts inflate low abundance things. When we use quantitative values in the counting, we see that the proteins unique to each site are extremely low abundance (only 0.1% of the intensity total). The vast majority of the signal at each site comes from proteins seen (and quantifiable) at both sites. DIA is supposed to remedy the poor reproducibility of identifications and quantifications in DDA experiments. Well, I don't see a problem that needs fixing with this data.

### Missing Data

The BGS data was 6,539 proteins with 10 TMT values per protein for a total of 65,390 values. Of those, there were 31 missing values (0.05%). There were 6,504 proteins with 65,040 total values and 35 missing values (0.05%) for the FLI data. Low missing data is clearly one of the advantages of TMT labeling. There is no missing data problem to fix either.

### Data Quality

#### Introduction

Global dataset stats and tables of numbers are exciting to me, but others might prefer plots and figures. I completely decouple quantitative data **generation** from quantitative data analysis. The data generation in the PAW pipeline is all Python scripts because an actual programming language is better for that kind of data processing. Once properly summarized quantitative data is packaged in well-formed tables, R and Bioconductor tools can be used. Statistical tools and data visualizations are more mature in R compared to Python. Notebooks are an excellent way to bring transparency, reproducibility, and story telling to data analyses. Jupyter notebooks are what I use (RMarkdown is also an alternative).

During the five years I have been analyzing data with notebooks, I have learned a few things. Every experiment is different and flexibility is key. You have to so some homework and understand some of the biology and the experimental design. You need some domain knowledge to inform the data analysis steps. I am not talking about a high level of biological expertise for each experiment you analyze, just enough biology background to understand why the study was designed as such, and to see if there are any positive or negative controls you can leverage in evaluating the data analysis steps. The data generation steps are much simpler, with far fewer choices, than the data analysis steps. Software that tries to combine data generation and data analysis is usually brought to its knees by the trade offs taken to add some lowest common denominator data analysis framework. You end up with hard to use software that performs poorly.

I have settled on a two notebook data analysis strategy. One notebook looks at dataset quality control metrics to make sure data normalizations worked as intended, that biological groups have the expected characteristics (that biological replicates within groups are similar and that there are some differences between groups), that samples are generally consistent in global properties (there can be many issues that can lead to sample outliers), and the degree of variance in the data (kind of a heads up on what to expect from statistical testing). A series of QC metrics and checks can find issues in the data that would not be obvious in the typical statistical testing steps. These QC steps could be incorporated into a longer statistical testing notebook but that hinders good data story telling (the story gets too long and complicated). The second notebook can focus on the statistical testing and visualization of differential abundance candidates.    

#### QC metrics

We will first look at the TMT data from each site independently. I created a combined grouped protein summary with all of the runs from both sites to get the best protein inference and to make sure that the quantitative protein data from each site was aligned to the same list of proteins. That summary table was opened in Excel for additional data filtering. There are always additional keratins identified that are not automatically mapped to the keratins in the common contaminants. Those get flagged as additional contaminants. There can be other classes of proteins that may or may not be contaminants depending on the samples, such as, hemoglobins and serum albumin. I always double check what proteins should or should not be flagged as contaminants. Excel's column filters make this easy. As I mentioned above, we identify more proteins than we can quantify. Some proteins do not have any quantitative data and need to be excluded. After these decisions about what proteins to quantify and which to exclude are made, the data from each site was packaged into simple tables (saved as tab-delimited text files) for loading into the notebooks. These tables have a column to denote the protein (the accessions) and 10 columns of TMT intensity values. The column labels were labeled by sample names instead of TMT tags to make the notebooks easier to understand.  

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
