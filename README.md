# Test data provenance

This directory holds the small, self-contained datasets committed to the repo
so the test profiles run offline without downloading anything. Two datasets:

| Dataset | Directory | Used by | Purpose |
|---------|-----------|---------|---------|
| Native direct-RNA | `directrna/` | `test_direct_rna` profile · `tests/pipeline_direct_rna.nf.test` | End-to-end poly(A) + DTU on real FAST5 signal |
| Coding potential | `coding_potential/` | coding-potential branch (any library: cDNA **or** direct-RNA) | CPAT/FEELnc noncoding vs. coding classification |

---

## `directrna/` — native direct-RNA

### Overview

Real HEK293T native direct-RNA reads with raw FAST5 signal, subset to a
mini-genome so the true poly(A) path runs end to end: genome alignment. Two conditions (WT vs. METTL3-KO) give DTU the ≥2 conditions it needs.

### Reference used

Mini-genome of **3 rebased slices** of the *Homo sapiens* Ensembl GRCh38
assembly (`ensembl_havana` annotation), one gene per contig. Original genomic
coordinates were rebased so the reads map to exactly these three genes:

| Contig | Gene | Ensembl gene ID | Source chromosome |
|--------|------|-----------------|-------------------|
| `3`  | RNF7   | ENSG00000114125 | chr3  |
| `9`  | NANS   | —               | chr9  |
| `17` | MRPL10 | —               | chr17 |

- `genome/drna_genome.fa` — the 3 rebased slices.
- `genome/drna.gtf` — matching rebased annotation: 20 transcripts / 72 exons /
  39 CDS, all `protein_coding`.
- `genome/drna_transcriptome.fa` — 20 spliced transcripts, needed because the
  profile sets `quantification_tool = 'both'` (exercises
  `MINIMAP2_TRANSCRIPTOME` + salmon/oarfish alongside featureCounts).

### FASTQ / FAST5 source

Native direct-RNA reads (with signal-level FAST5) from the HEK293T
**WT vs. METTL3-KO** m6A dataset, distributed through `nf-core/test-datasets`
(`modification_fast5_fastq`). Reads were subset to the three genes above.


### Samples

| Sample | Condition | Reads (FASTQ) | FAST5 |
|--------|-----------|---------------|-------|
| `HEK293T-WT-rep1` | `wt` | 162 | 162 |
| `HEK293T-METTL3-KO-rep1` | `ko` | 302 | 302 |

```
directrna/
├── HEK293T-WT-rep1/
│   ├── HEK293T-WT-rep1.fastq.gz
│   └── fast5/                     # 162 single-read .fast5
├── HEK293T-METTL3-KO-rep1/
│   ├── HEK293T-METTL3-KO-rep1.fastq.gz
│   └── fast5/                     # 302 single-read .fast5
└── genome/
    ├── drna_genome.fa
    ├── drna.gtf
    └── drna_transcriptome.fa
```

### Profile used

`conf/test_direct_rna.config`:

```text
fasta:                genome/drna_genome.fa
gtf:                  genome/drna.gtf
transcript_fasta:     genome/drna_transcriptome.fa
direct_rna:           true
run_polya:            true          # the point of this dataset
quantification_tool:  both          # featureCounts + salmon/oarfish
run_coding_potential: false         # too few reads → no meaningful novels
run_sqanti:           false         # same
# DE skipped: no deseq2_formula (covered by the cDNA test)
```

> The nf-test
> (`tests/pipeline_direct_rna.nf.test`) writes its own two-row samplesheet
> (`wt` + `ko`). 

---

## `coding_potential/` — noncoding vs. coding training set

### Overview

**Fully synthetic** set (520 transcripts) for the novel-transcript
coding-potential branch: CPAT positive/negative training, FEELnc coding
reference, PLEK. No external accession — sequences are generated
deterministically.

These are static **training/reference** FASTAs. The coding-potential demo data is
gated only by `run_coding_potential`. It classifies the `NOVEL_TRANSCRIPTS` output from **any** library
type, cDNA **or** direct-RNA. So the same training set feeds both paths. It is
turned off in `test_direct_rna` only because that mini-genome + few reads yield
no meaningful novel transcripts, not because direct-RNA is unsupported.

Produces 520 records each of:

| File | Seqs | Role |
|------|------|------|
| `cds.fa` | 520 | CPAT positive (coding) training |
| `noncoding.fa` | 520 | CPAT negative (noncoding) training |
| `mrna.fa` | 520 | FEELnc coding reference |
| `novel_transcripts.{fa,gtf,tmap}` | 520 | candidate novel transcripts |
| `genome.fa`, `reference.gtf` | — | matched synthetic reference for extraction modules |

### Profile used

Committed wiring is in the cDNA `test` profile (`conf/test.config`) via
`cpat_training_coding_fasta`, `cpat_training_noncoding_fasta`, and
`feelnc_mrna_fasta` (with `run_coding_potential = true`). The same three params
enable the branch for a direct-RNA run — pair them with `run_coding_potential =
true` on a dataset that produces novel transcripts. 

---

## Notes

- These files exist for workflow testing and CI; reduced references keep
  runtime and output size small and reproducible.
- `directrna/` uses **real** native RNA signal (required — poly(A) needs FAST5);
  `coding_potential/` is **synthetic** and deterministic.
