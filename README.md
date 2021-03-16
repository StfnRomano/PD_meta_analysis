# Meta-analysis of the Parkinson’s disease gut microbiome suggests alterations linked to intestinal inflammation

**Please note**, these scripts are purely intended as a descriptive extension of the methods used in the publication: 

Romano, Savva, Bedarf, Charles, Hildebrand, Narbad, 2020, *Meta-analysis of the Parkinson’s disease gut microbiome suggests alterations linked to intestinal inflammation* npj Parkinsons Dis. 7, 27 (2021). https://doi.org/10.1038/s41531-021-00156-z

If you wish to use any material in this repository please be aware that this code was tailored to the analyses conducted in the above manuscript and it is presented here to foster reproducibility. **The code is not intended to be used in other contexts**. Hence, use it at your own risk.
Only the code related to the analyses performed at the Species level is reported, as it is identical to the one used for analyzing Genus, Family, and 16S-based functional predictions. Specific changes made to accommodate the data structure of the other taxonomic ranks will be reported in the description below.
The work-flow consists of 10 R scripts that need to be executed one after the other. In addition, there are two R scripts (AA and AB) that were used to combine graphs and generate the final figures and tables included in the manuscript.

**If you find this code useful or use any of the approaches below in your work, please cite**:

Romano, Savva, Bedarf, Charles, Hildebrand, Narbad, 2020, *Meta-analysis of the Parkinson’s disease gut microbiome suggests alterations linked to intestinal inflammation* npj Parkinsons Dis. 7, 27 (2021). https://doi.org/10.1038/s41531-021-00156-z

## Work-flow

### 01 - Load and format data and create phyloseq objects.

This script will load the count tables and the mapping files used in Lotus/picrust2 and create phyloseq objects. The objects are then saved as rds files. 

### 02 - Rarefy data and calculate alpha-diversity indices.

Each dataset is rarefied without replacements and selected alpha-diversity indices are calculated. Agresti’s generalized odd ratios are used to estimate the differences between PD and controls across the studies. Estimates are then pooled using a random-effect meta-analysis approach.

**NOTE**: This script was run only for the data referring to species. It was not run for the other taxonomic ranks and the functional predictions, for which we rarefied the count tables in script 03.

### 03 - Total sum scaling and WMW differential abundance testing

After calculating relative abundances (total sum scaling, TSS) for the data of each study, they were pooled into a “Combined” dataset. Beta-diversity analyses were performed on the individual dataset and on the “Combined” data. The “Combined” dataset was then used to perform dbRDAs and identify the taxa that most influence the separation between PD and control samples. Differential abundance analysis was performed using Wilcoxon-Mann-Whitney non-parametric tests.

**NOTE**: in the dbRDA analysis performed at the family level we selected only families with a variation along CAP1 ≥ |0.07|. All the beta-diversity analyses were not performed for the 16S-based functional predictions.

### 04 - Variance stabilizing transformation and DESeq2

Non-rarefied count data were normalized using variance stabilizing transformation (VST) and used to perform beta-diversity analyses. DESeq2 was then used to infer differential abundances.

### 05a/b - Centered log-ratios and ANCOM

Raw count data were transformed using the centered log-ratios (CLR) and used to perform beta-diversity analyses. ANCOM was then used to infer differential abundances.

### 06 - Meta-analysis approaches

Data were transformed using CLR and differences in abundance between PD and controls were calculated using Agresti’s generalized odd ratios for each species. Results were then pooled using a random-effect meta-analysis approach.

**NOTE**: Linear models were instead used for Genus, Family and 16S-based predicted functionalities. 

### 07 - Combining DA results

Differential abundance results were combined and data were formatted to run generalized linear mixed models (GLMM) and create the final figures.

### 08 - GLMM to assess the effect of gender and age on taxa abundances

The meta-data made available by 5 studies were used to assess the effect of gender and age on each taxon. We used the raw counts and rarefied them at 10,000. We then created GLMMs with and without the variable disease status. We then compared all models and selected the ones within 2 delta AIC. If amongst them there was one model that did not contain the variable disease status, we assumed that the current data do not allow to establish whether the disease status is essential to describe the distribution of taxa abundances. Hence, we removed these taxa from further analyses. If all models contained the variable disease status, we concluded that this variable is essential to describe the abundance of the taxa.

**NOTE**: These analyses were not run for the 16S-based predicted functionalities. 

### 09 - Create final DA figures

The combined DA results were then plotted after counting the number of times each taxon was detected DA across studies and methods. Taxa potentially confounded by gender and/or age were removed.

### 10 - Estimating the effect of technical factors on bacterial community structures

Additional distance-based RDAs were created to assess the effect of study-specific technical factors on the structure of the gut microbiome.

------------------------------------------------------------------------------------------------------------------

### AA - Combine graphs

This code was used to combine the graphs obtained along the work-flow and generate the figures used in the manuscript. 
We performed a sensitivity analysis to verify the strength of our findings. This was done by repeating the above work-flow omitting the baseline of a longitudinal Finnish study. The differences observed in the differential abundant taxa and 16S-based predicted functionalities were then plotted.

### AB - Combine tables

This code was used to combine the results obtained along the work-flow and generate the tables used in the manuscript. Please note, tables were then formatted manually.

