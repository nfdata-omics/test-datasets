The GTF file was downloaded from Ensembl, release 115. The information about chromosome 21 was extracted to make it a toy dataset with the following command:

`awk '$1=="21" && $4>=1 && $5<=15000000 || /^#/' chr21.gtf > chr21_subset.gtf`
