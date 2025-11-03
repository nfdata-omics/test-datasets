# Chromosome-specific FASTQ subsampling of 10x Genomics Visium colorectal cancer dataset

**Dataset source:**
10x Genomics Visium colorectal cancer dataset - sample P2CRC
[https://www.10xgenomics.com/platforms/visium/product-family/dataset-human-crc](https://www.10xgenomics.com/platforms/visium/product-family/dataset-human-crc)

## Processing

Filter FASTQ reads to only those mapping to a specified chromosome (e.g. `chr21`) and subsample a fixed number of reads, for downstream testing.

## Commands

```bash
# Load modules / Singularity (optional depending on environment)
module load singularity

CHR_NAME="chr21"
NREADS=40000

# Input BAM
BAM_FILE="possorted_genome_bam.bam"

# tool wrappers (example using Galaxy images)
SAMTOOLS="singularity run samtools_image.sif samtools"
SEQKIT="singularity run seqkit_image.sif seqkit"

# Step 1: extract read names aligned to the chromosome
$SAMTOOLS view $BAM_FILE $CHR_NAME | cut -f1 > reads_name_chr21.txt

# Step 2: concatenate original FASTQ lanes
cat sample_L00*_R1_001.fastq.gz > R1_all.fastq.gz
cat sample_L00*_R2_001.fastq.gz > R2_all.fastq.gz

# Step 3: extract reads matching the chromosome-specific list
$SEQKIT grep -f reads_name_chr21.txt R1_all.fastq.gz -o sample_chr21_R1.fastq.gz
$SEQKIT grep -f reads_name_chr21.txt R2_all.fastq.gz -o sample_chr21_R2.fastq.gz

# Step 4: subsample first NREADS*lines* (fast)
zcat sample_chr21_R1.fastq.gz | head -n $NREADS > sample_chr21_sub${NREADS}_R1.fastq
gzip sample_chr21_sub${NREADS}_R1.fastq

zcat sample_chr21_R2.fastq.gz | head -n $NREADS > sample_chr21_sub${NREADS}_R2.fastq
gzip sample_chr21_sub${NREADS}_R2.fastq
```

