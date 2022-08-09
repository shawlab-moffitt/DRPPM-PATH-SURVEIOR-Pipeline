# DRPPM-PATH-SURVIOER-Pipeline

# Introduction

# Installation

## Via Download

1. Download the [Zip File](https://github.com/shawlab-moffitt/DRPPM-PATH-SURVIOER-Pipeline/archive/refs/heads/main.zip) from this GitHub repository: https://github.com/shawlab-moffitt/DRPPM-PATH-SURVIOER-Pipeline
2. Unzip the downloaded file into the folder of your choice.

## Via Git Clone

1. Clone the [GitHub Repository](https://github.com/shawlab-moffitt/DRPPM-SURVIVE.git) into the destination of your choice.
   * Can be done in R Studio Terminal or a terminal of your choice
```bash
git clone https://github.com/shawlab-moffitt/DRPPM-PATH-SURVIOER-Pipeline.git
```

# Requirments

* `R` - https://cran.r-project.org/src/base/R-4/
* `R Studio` - https://www.rstudio.com/products/rstudio/download/

# R Dependencies

|  |  |  |  |  |
| --- | --- | --- | --- | --- |
| DT_0.23 | tibble_3.1.7 | readr_2.1.2 | dplyr_1.0.9 | tidyr_1.1.3 |
| GSVA_1.40.1 | clusterProfiler_4.0.5 | gtsummary_1.6.0 | survival_3.2-11 | survminer_0.4.9 |

# Required Files

* **Expression Matrix (.tsv/.txt):**
  * Must be tab delimited with gene names as symbols located in the first column with subsequent columns consiting of the sample name as the header and expression data down the column.
  *  The App expects lowly expressed genes filtered out and normalized data either to FPKM or TMM.
     * Larger files might inflict memory issues for you local computer.

* **Meta/Clincal Data (.tsv/.txt):**
  * This should be a tab delimited file with each row depicting a sample by the same name as in the expression matrix followed by informative columns containing survival data and other features to analyze the samples by.
  * Required columns:
    * A sample name column
    * A survival time and ID column (can be more than one type of survival time)

* **Gene Set File (.gmt/.txt/.tsv/.RData):**
  * This is the file that contains the gene set names and genes for each gene set.
  * I provide example gene sets from publicly available sources here: https://github.com/shawlab-moffitt/DRPPM-PATH-SURVIOER-Pipeline/tree/main/Example_GeneSets
    * These include the Molecular Signatures Database, LINCS L1000 small molecule perturbations, and ER Stress Signatures.
  * An .RData list is the preferred format which is a named list of gene sets and genes. A script to generate this list is provided here: [GeneSetRDataListGen.R](https://github.com/shawlab-moffitt/DRPPM-SURVIVE/blob/main/GeneSet_Data/GeneSetRDataListGen.R)
    * The app also accepts gene sets in .gmt format or two-column tab-delimited .tsv/.txt format with the first column being the gene set name repeating for every gene symbol that would be placed in the second column. If either of these three formats are given athe app with automatically convert them to an RData list.
    * If no Gene Set File is provided, the analysis can still run if tyhe user only plans to rank on a gene level
  * I have provided duiplicate gene sets in both RData and txt file types
    
* **Gene Set .lst File (.lst) (OPTIONAL):**
  * In the case the user would like to run the pipeline with multiple gene set files, the user can provide a two column file containing the gene set name in the first column and the path to the gene set file in the second column.
  * The gene set files listed should follow the format described above.
    
* **Parameter File (.txt/.tsv):**
  * This contains the file paths of the files described above, as well as run parameters.
  * This file is described in detail in the Set-Up section below.

# Set-Up

## Parameter File

* This file will contain all the necessary information for the pipeline to run.
* Some parameters are optional.
* Do not include a header in this file.
* The order does not matter, but the parameter names do matter
* The Survival Time and ID Label should be the column names of the respective survival data you want to analyze.
  * For the simplified version only input one survival type. For example, only run through with OS data and your column names may be OS.time and OS.ID (make sure your column names match what you put in the parameter file.
  * For advanced use (in the advanced folder), you may input a comma seperated list of survival time and ID labels, but you must have a time and ID column for each survival data type and they must be in the same order for both Time and ID.
* The "Rank_Genes" parameter is an optional function that will run the analysis based on each gene in the expression data. This will produce a table of genes ranked by hazard ratio as well as the gene set(s) being run.
  * The user may choose to only rank genes and leave the Gene_Set_File parameter blank if they choose.
  * This analysis may take an extended period of time.
* The Covariate parameters allow the user to perform an interactive Coxh survival analysis between the pathway High/Low score and a specified categorical column from the meta/clinical data. The user input would be the column name of the covariate.
  * If performing this analysis, it is requested the user provide a reference variable for the Coxh analysis, this would go in the "Covariate_Reference" parameter below. If this is not specified the function will use the first level from the factored covariate column.

| Parameter | User Input |
| --- | --- |
| Project_Name | [User_Defined] |
| Expression_Matrix_file | [~/Path/To/Expression.txt] |
| Meta_Data_File | [~/Path/To/Meta.txt] |
| Gene_Set_File | [~/Path/To/GeneSetFile.[txt/tsv/gmt/RData] or ~/Path/To/GeneSetFiles.lst] |
| Gene_Set_Name | [Desired Name for gene set (optional)]
| Output_File_Path | [~/Path/To/Output/Folder/] |
| Survival_Time_Label | [OS.time/EFS.time/PFI.time/ect] |
| Survival_ID_Label | [OS/EFS/PFI/ect] |
| Rank_Genes | [TRUE/FALSE] |
| Covariate_Column_Label | [optional] |
| Covariate_Reference | [optional] |

## Running the script

Please keep in mind, depending on how large the gene set files are and if the user chooses to perform the individual gene analysis, this script can take multiple hours to run.

### R Studio

* The user can input the path to the parameter file at the top of the [RStudio version](https://github.com/shawlab-moffitt/DRPPM-PATH-SURVIOER-Pipeline/blob/main/ssGSEA_Coxh_Ranking_RStudio.R) of the script
* it is recommended to run the script as a local job in R Studio.
  * In the bottom console of R Studio select the "Jobs" tab and select "Start a Local Job" then choose the script wherever you saved it.
* Or the user can select all of the contents and run the script
  * Running the script this way will keep the R studio session busy and you will be able to run anything else while the script is running.

### Command Line
* The user can run this script in a command line environment as long as the requirments are met in the environment you are using.
* When the script and parameter file are in the desired directory run the command below:
```{linux}
Rscript ssGSEA_Coxh_Ranking_CommandLine.R [parameter_file]
```

# Pipeline Steps and Output

## Prepping Data

## ssGSEA Scoring

## Above and Below Median Calculation

## Cox Propotional Hazard Analysis

## Ranking

# Further Applications

The comprehensive table that is output from the analysis will elucidate some of the top pathways or genes that contribute to high-risk patients. If the [DRPPM-PATH-SURVIOER Shiny App](https://github.com/shawlab-moffitt/DRPPM-PATH-SURVIOER) was generated, these top pathways can be visualized within the app for validation. 

Additionally, to investigate the top pathways the user may take the top user specified number of pathways or a grouping of pathways the user wishes to explore further and input this list of pathways to the DRPPM-Jaccard-Pathway-Connectivity Shiny App and visualize the connectedness of the different pathways based on Jaccard Distance. The app set-up and function is explained in detail on the [GitHub page](https://github.com/shawlab-moffitt/DRPPM-Jaccard-Pathway-Connectivity).

Furthermore, the hazard ratio ranked gene list output from the script can be used as input for the Pre-Ranked GSEA Shiny App which allows users to perform Gene Set Enrichment Analysis with a pre-ranked set of genes on various different gene sets and provides enriched signature tables and enrichment plots for visualization. More information on set-up and funciton on out [GitHub page](https://github.com/shawlab-moffitt/DRPPM-Pre-Rank-GSEA).


# Quesions and Comments

Please email Alyssa Obermayer at alyssa.obermayer@moffitt.org if you have any further comments or questions.
