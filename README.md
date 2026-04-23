# Test CRAM provenance

This directory contains test `CRAM` files generated from public `Saccharomyces` short-read datasets using `nf-core/sarek`.

## Overview

The purpose of these files is to provide a small cross-species yeast test dataset for mapping-based workflow development. Reads from multiple *Saccharomyces* species were aligned against a reduced reference consisting of **chromosome I only** from the *Saccharomyces cerevisiae* Ensembl assembly, distributed through the `nf-core/test-datasets` repository.

## Reference used

The alignment reference was:

- `https://raw.githubusercontent.com/nf-core/test-datasets/626c8fab639062eade4b10747e919341cbf9b41a/reference/genome.fasta`

This FASTA contains only **chromosome I** from the *S. cerevisiae* Ensembl assembly, as documented in the `nf-core/test-datasets` repository:

- `https://github.com/nf-core/test-datasets/blob/rnaseq/README.md`

## Pipeline used

The `CRAM` files were generated with `nf-core/sarek` using the following parameters:

```text
input: samplesheet.csv
outdir: output
split_fastq: 0
no_intervals: true
skip_tools: baserecalibrator
aligner: bwa-mem2
fasta: https://raw.githubusercontent.com/nf-core/test-datasets/626c8fab639062eade4b10747e919341cbf9b41a/reference/genome.fasta
genome: null
save_mapped: true
```

In practice, this means:

- input data were paired-end FASTQ files listed in a Sarek samplesheet
- reads were aligned with `bwa-mem2`
- no interval splitting was used
- base quality score recalibration was skipped
- mapped alignment files were retained with `--save_mapped`
- the reference was supplied explicitly with `--fasta`, with `--genome null`

## Input samplesheet

The following samplesheet was used:

```csv
patient,sample,lane,fastq_1,fastq_2
scer_SA1,scer_SA1,L001,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR845/004/SRR8455574/SRR8455574_1.fastq.gz,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR845/004/SRR8455574/SRR8455574_2.fastq.gz
scer_E59,scer_E59,L001,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR173/006/ERR1730036/ERR1730036_1.fastq.gz,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR173/006/ERR1730036/ERR1730036_2.fastq.gz
scer_BY4742,scer_BY4742,L001,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR156/005/SRR1569895/SRR1569895_1.fastq.gz,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR156/005/SRR1569895/SRR1569895_2.fastq.gz
spar_yHKS224,spar_yHKS224,L001,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR186/000/SRR1868810/SRR1868810_1.fastq.gz,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR186/000/SRR1868810/SRR1868810_2.fastq.gz
spar_R43,spar_R43,L001,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR445/004/SRR4453484/SRR4453484_1.fastq.gz,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR445/004/SRR4453484/SRR4453484_2.fastq.gz
spar_337,spar_337,L001,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR152/054/SRR15215154/SRR15215154_1.fastq.gz,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR152/054/SRR15215154/SRR15215154_2.fastq.gz
seub_UCD646,seub_UCD646,L001,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR100/071/ERR10084971/ERR10084971_1.fastq.gz,ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR100/071/ERR10084971/ERR10084971_2.fastq.gz
```

## FASTQ sources

All FASTQ inputs were obtained from public archives hosted through the ENA/EBI FTP service.

### *Saccharomyces cerevisiae*

- `scer_SA1`
  - run accession: `SRR8455574`
  - FASTQ source:
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR845/004/SRR8455574/SRR8455574_1.fastq.gz`
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR845/004/SRR8455574/SRR8455574_2.fastq.gz`

- `scer_E59`
  - run accession: `ERR1730036`
  - FASTQ source:
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR173/006/ERR1730036/ERR1730036_1.fastq.gz`
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR173/006/ERR1730036/ERR1730036_2.fastq.gz`

- `scer_BY4742`
  - run accession: `SRR1569895`
  - FASTQ source:
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR156/005/SRR1569895/SRR1569895_1.fastq.gz`
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR156/005/SRR1569895/SRR1569895_2.fastq.gz`

### *Saccharomyces paradoxus*

- `spar_yHKS224`
  - run accession: `SRR1868810`
  - FASTQ source:
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR186/000/SRR1868810/SRR1868810_1.fastq.gz`
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR186/000/SRR1868810/SRR1868810_2.fastq.gz`

- `spar_R43`
  - run accession: `SRR4453484`
  - FASTQ source:
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR445/004/SRR4453484/SRR4453484_1.fastq.gz`
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR445/004/SRR4453484/SRR4453484_2.fastq.gz`

- `spar_337`
  - run accession: `SRR15215154`
  - FASTQ source:
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR152/054/SRR15215154/SRR15215154_1.fastq.gz`
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR152/054/SRR15215154/SRR15215154_2.fastq.gz`

### *Saccharomyces eubayanus*

- `seub_UCD646`
  - run accession: `ERR10084971`
  - FASTQ source:
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR100/071/ERR10084971/ERR10084971_1.fastq.gz`
    - `ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR100/071/ERR10084971/ERR10084971_2.fastq.gz`

## Notes

- These files were created for workflow testing and development.
- The sample set intentionally includes multiple *Saccharomyces* species mapped to a single *S. cerevisiae* reference.
- The reduced reference keeps runtime and output size small, which is useful for CI and reproducible test fixtures.

