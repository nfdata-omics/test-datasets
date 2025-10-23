# chr21 reference, cellranger compatible

This genome+annotation files were obtained from the 
reference provided by 10x for cellranger, version 2024-A (refdata-gex-GRCh38-2024-A)
and then subsampled to the chr21 with the commands:

```
singularity run https://depot.galaxyproject.org/singularity/seqkit:2.9.0--h9ee0642_0 \
    seqkit grep -r -p '^chr21$' refdata-gex-GRCh38-2024-A/fasta/genome.fa  > chr21.fa
```

```
awk '($1 == "chr21")' refdata-gex-GRCh38-2024-A/genes/genes.gtf  > chr21.gtf
```
