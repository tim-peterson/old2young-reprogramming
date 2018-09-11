
## Import data

Find list of genes that are differentially expressed. They are often in supplemental tables, such as Excel files. R Studio can work on many types of files including Excel files, CSV, TSV files.

[Mouse liver, PMID: 26541291](https://www.ncbi.nlm.nih.gov/pubmed/26541291)

	aging_mouse_liver <- read_excel("~/Downloads/12864_2015_2061_MOESM2_ESM.xlsx")

[Rat kidney, PMID: 27153548](https://www.ncbi.nlm.nih.gov/pubmed/27153548)

	aging_rat_kidney <- read_excel("~/Downloads/oncotarget-07-30037-s003.xls")



## Get intersection of multiple data frame columns (lists of genes)

	gene_intersection <- Reduce(intersect, list(tolower(aging_mouse_liver$gene_name),tolower(aging_rat_kidney$X__1)))

`list()` can take more than two gene lists.

`tolower()` puts all gene names as lowercase to ensure that the intersection doesn't fail due to capitalization.

## Write this data to file

	write.csv(gene_intersection, file = "/Users/timrpeterson/OneDrive - Washington University in St. Louis/Data/Aging/intersect_liver_kidney.csv")


## Convert mouse Rik names to human names

`aging_test` is a dataframe of mouse gene expression data. `mouse2human` is a dataframe of mouse to human names converted by Biomart. `mergedname` is a composite data frame "joined" on the mouse names in each dataframe. The `by.x` and `by.y` are needed because the mouse gene names aren't the same in the two data frames.

	mergedname <- merge(mouse2human,aging_test,by.x = "Mouse gene name", by.y="gene_name”)

## Count occurrences of gene names across multiple lists

Remove duplicates `in data->remove duplicates` and make lowercase using `=LOWER() combined with fill down` in Excel.

Import data, e.g., as an Excel file.

	library(readxl)

	hu_skel_musc <- read_excel("~/OneDrive - Washington University in St. Louis/Data/Aging/Standardized/Up in young/hu_skel_musc.xlsx")

Just checking to see if you've imported the data you want:
	
	View(hu_skel_musc) 

`count_occurences` this will contain your gene counts across your columns.

	count_occurences <- table(unlist(hu_skel_musc)))


## To-do	

1. Get gene homolog tables (from BioMart) to convert non-mammalian species gene names to their mammalian homologs. This will enable us to leverage experiments done in worms, yeast, fish, etc.

2. Identify a list of transcription factors that control aging-associated genes common to multiple datasets.