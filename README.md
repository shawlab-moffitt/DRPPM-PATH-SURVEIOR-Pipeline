# DRPPM-PATH-SURVIOER-Pipeline

# Introduction

# Installation

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
  * This file is provided but can be replaced for a file of the users choice.
  * An .RData list is the preferred format which is a named list of gene sets and genes. A script to generate this list is provided here: [GeneSetRDataListGen.R](https://github.com/shawlab-moffitt/DRPPM-SURVIVE/blob/main/GeneSet_Data/GeneSetRDataListGen.R)
    * The app also accepts gene sets in .gmt format or two-column tab-delimited .tsv/.txt format with the first column being the gene set name repeating for every gene symbol that would be placed in the second column. If either of these three formats are given athe app with automatically convert them to an RData list.
    
* **Gene Set .lst File (.lst) (OPTIONAL):**
  * In the case the user would like to run the pipeline with multiple gene set files, the user can provide a two column file containing the gene set name in the first column and the path to the gene set file in the second column.
  * The gene set files listed should follow the format described above.
    
* **Parameter File (.txt/.tsv):**
  * This contains the file paths of the files described above, as well as run parameters.
  * This file is described in detail in the Set-Up section below.

# Set-Up

* This file will contain all the necessary information for the pipeline to run.
* Some parameters are optional.
* Do not include a header in this file.
* The order does not matter, but the parameter names do matter
* The survival time and ID label should be what is in your data that you want to analyze. For advanced use, you may input a comma seperated list of survival time and ID labels, but you must have a time and ID column for each survival data type and they must be in the same order for both Time and ID.
* The "Rank_Genes" parameter is an optional function that will run the analysis based on each gene in the expression data. This will produce a table of genes ranked by hazard ratio as well as the gene set(s) being run.
* The Covariate parameters allow the user to perform an interactive Coxh survival analysis between the pathway High/Low score and a specified categorical column from the meta/clinical data. The user input would be the column name of the covariate.
  * If performing this analysis, it is requested the user provide a reference variable for the Coxh analysis, this would go in the "Covariate_Reference" parameter below. If this is not specified the function will use the first level from the factored covariate column.

| Parameter | User Input |
| --- | --- |
| Project_Name | User_Defined |
| Expression_Matrix_file | ~/Path/To/Expression.txt |
| Meta_Data_File | ~/Path/To/Meta.txt |
| Gene_Set_File | ~/Path/To/GeneSetFile.[txt/tsv/gmt/RData] or ~/Path/To/GeneSetFiles.lst |
| Output_File_Path | ~/Path/To/Output/Folder/ |
| Survival_Time_Label | OS.time |
| Survival_ID_Label | OS |
| Rank_Genes | TRUE/FALSE |
| Covariate_Column_Label | [optional] |
| Covariate_Reference | [optional] |

# Pipeline Steps

# Pipeline Output

# Quesions and Comments

Please email Alyssa Obermayer at alyssa.obermayer@moffitt.org if you have any further comments or questions.
