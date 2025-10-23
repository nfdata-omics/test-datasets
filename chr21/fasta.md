The FASTA file was downloaded from Ensembl, release 115. The file is composed of the primary human genome assembly. It was modified with the following commands in order to follow the pipeline requirements and make it a toy dataset (from position 1 to 15000000):

`samtools faidx chr21.fa 21:1-15000000 > chr21_subset.fa`

`sed -i '1s/.*/>21/' chr21_subset.fa`
