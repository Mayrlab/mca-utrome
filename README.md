[![DOI](https://zenodo.org/badge/135358784.svg)](https://zenodo.org/badge/latestdoi/135358784)

## 3' UTR cleavage site identification from the Mouse Cell Atlas
This reposistory hosts a Snakemake pipeline used to call 3' UTR cleavage sites *de novo* from [the Mouse Cell Atlas dataset](http://bis.zju.edu.cn/MCA/). The final output of the pipeline is the Mouse UTRome used in [the scUTRquant pipeline](https://github.com/Mayrlab/scUTRquant) for quantifying 3' UTR isoforms from scRNA-seq data.

The accompanying manuscript is openly available at:

> Fansler, M.M., Mitschka, S. & Mayr, C. Quantifying 3′UTR length from scRNA-seq data reveals changes independent of gene expression. *Nat Commun* **15**, 4050 (2024). [https://doi.org/10.1038/s41467-024-48254-9](https://doi.org/10.1038/s41467-024-48254-9)

## Requirements
The original pipeline was run with 

- Snakemake 6.8
- Conda 4.11.0
- Mamba 0.21.2

and assumes use of `--use-conda` to source all other software.

Rule `resources:` values are compatible with [Snakemake profiles](https://github.com/Snakemake-Profiles), which we recommend using. The included `lsf.yaml` provides some adjustments to default resource allocations specific to our cluster, mostly providing for jobs with longer run times.

## Running

> ⚠️ This is not a trivial pipeline! If you require scUTRquant (kallisto) indices with specific settings, please [file an Issue](https://github.com/Mayrlab/mca-utrome/issues) and we may be able to help generating this. For example, we may be able to provide intermediate files to "bootstrap" the pipeline and avoid downloading and aligning the raw data.

After cloning this repository and adjusting the `config.yaml` to point at a local HISAT index (`hisatIndex`), all outputs for the UTRome can be generated with

```bash
snakemake --use-conda
```

Parameters are adjustable in the `config.yaml`:

 - **epsilon (e):** distance for initial merging of cleavage sites
 - **threshold (t):** minimum TPM for cutoff (per cell type)
 - **gencodeVersion (gc):** version of GENCODE to use (default 25)
 - **polyASiteTPM (pas):** minimum TPM for polyASite database entries
 - **non-internal priming likelihood (f):** minimum posterior likelihood for not being an internal priming site
 - **truncation width (w):** maximum transcript distance to cleavage site
 - **mergeDistance (m):** distance within which to merge isoforms when quantifying in scUTRquant (kallisto)

Alternatively, one can generate a specific file directly, such as a kallisto index with a direct command:

```bash
snakemake --use-conda data/kdx/utrome.e30.t5.gc25.pas3.f0.9999.w500.kdx
```
