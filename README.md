# Cassini Probe Generation

This repository provides three workflows for generating **Cassini probes**:

1. **Canonical transcript probes**
2. **All transcript probes**
3. **Synthetic sequence probes**

The three workflows are provided as Jupyter notebooks. Each notebook uses a collection of supporting scripts contained in the `cassini_code_set` directory.

---

## Repository Structure

The repository contains the three main running notebooks and the supporting Cassini code set:

```text
.
├── generate_canonical_probes.ipynb
├── generate_all_transcript_probes.ipynb
├── generate_synthetic_probes.ipynb
│
└── cassini_code_set/
    ├── ...
    ├── ...
    └── ...
```

The `cassini_code_set` directory contains the **hundreds of supporting scripts required to run the three workflows**.

The notebooks are the entry points for the analysis. The supporting scripts should not be run individually.

---

# 1. Download the Repository

Clone or download the repository:

```bash
git clone <REPOSITORY_URL>
cd <REPOSITORY_NAME>
```

After downloading, make sure that both the notebooks and the `cassini_code_set` directory are present:

```text
<REPOSITORY_NAME>/
├── generate_canonical_probes.ipynb
├── generate_all_transcript_probes.ipynb
├── generate_synthetic_probes.ipynb
└── cassini_code_set/
```

---

# 2. Configure the Supporting Scripts

The three notebooks depend on the scripts contained in `cassini_code_set`.

The location of these supporting scripts is specified in each notebook using the `script_dir` parameter.

For example:

```python
script_dir = "<PATH_TO_CASSINI_CODE_SET>"
```

The `script_dir` should point to the directory containing the downloaded `cassini_code_set`.

For example, if the repository has been downloaded to:

```text
<PROJECT_DIRECTORY>/
```

then:

```python
script_dir = "<PROJECT_DIRECTORY>/cassini_code_set/"
```

The exact path depends on where the repository has been downloaded.

> **Important:** The complete `cassini_code_set` directory is required. Do not download only individual scripts from this directory, as the notebooks may depend on multiple supporting scripts.

---

# 3. Canonical Transcript Probe Generation

The canonical transcript workflow is provided in:

```text
generate_canonical_probes.ipynb
```

This notebook generates probes for **canonical transcripts** using a reference genome and gene annotation.

## Parameters

The following parameters are defined in the notebook:

```python
gtf_file = "<PATH_TO_GTF_FILE>"

genome_file = "<PATH_TO_GENOME_FASTA>"

output_folder = "<OUTPUT_DIRECTORY>"

Python_interpreter = "<PATH_TO_PYTHON>"

script_dir = "<PATH_TO_CASSINI_CODE_SET>"

suffix = "mo"

parallel = "yes"

species = "mouse"

generate_genome = "yes"

genome_name = "<GENOME_ASSEMBLY>"

genome_anno = "<GENOME_ANNOTATION>"

probe_type = {
    "exon_probe": "yes",
    "intron_probe": "yes",
    "junction_probe": "yes"
}
```

## Parameter Description

| Parameter            | Description                                                                 |
| -------------------- | --------------------------------------------------------------------------- |
| `gtf_file`           | Path to the reference GTF annotation file.                                  |
| `genome_file`        | Path to the reference genome FASTA file.                                    |
| `output_folder`      | Directory where generated probes and intermediate files will be stored.     |
| `Python_interpreter` | Path to the Python interpreter/environment used by the workflow.            |
| `script_dir`         | Path to the `cassini_code_set` directory containing the supporting scripts. |
| `suffix`             | Suffix used to identify the generated probe set.                            |
| `parallel`           | Controls whether processing is performed in parallel.                       |
| `species`            | Species being analyzed.                                                     |
| `generate_genome`    | Controls genome-related file generation.                                    |
| `genome_name`        | Name of the reference genome assembly.                                      |
| `genome_anno`        | Name/version of the reference annotation.                                   |
| `probe_type`         | Defines which types of probes should be generated.                          |

## Probe Types

The notebook can generate three types of probes:

### Exon probes

Probes targeting exonic regions.

### Intron probes

Probes targeting intronic regions.

### Junction probes

Probes targeting exon-exon junctions.

Probe types can be enabled or disabled using:

```python
probe_type = {
    "exon_probe": "yes",
    "intron_probe": "yes",
    "junction_probe": "yes"
}
```

For example:

```python
probe_type = {
    "exon_probe": "yes",
    "intron_probe": "no",
    "junction_probe": "yes"
}
```

will generate exon and junction probes but not intron probes.

## Running the Workflow

Open:

```text
generate_canonical_probes.ipynb
```

in JupyterLab or another compatible Jupyter environment.

Configure the parameters and run the notebook cells in order.

The notebook calls the supporting scripts from the directory specified by:

```python
script_dir
```

---

# 4. Probe Generation for All Transcripts

The all-transcripts workflow is provided in:

```text
generate_all_transcript_probes.ipynb
```

This notebook generates probes for **all transcripts** present in the supplied reference annotation.

Unlike the canonical transcript workflow, this workflow considers all annotated transcript isoforms.

This can be useful for:

* Alternative transcript isoforms
* Multiple transcript variants
* Comprehensive transcript-level probe sets
* Genes with multiple annotated transcripts

## Configuration

The notebook uses a reference genome and annotation:

```python
gtf_file = "<PATH_TO_GTF_FILE>"

genome_file = "<PATH_TO_GENOME_FASTA>"

output_folder = "<OUTPUT_DIRECTORY>"

Python_interpreter = "<PATH_TO_PYTHON>"

script_dir = "<PATH_TO_CASSINI_CODE_SET>"
```

Additional parameters required by the notebook should be configured in the corresponding parameter section.

## Input Files

### Reference GTF

The GTF provides gene and transcript annotation:

```text
<PATH_TO_GTF_FILE>
```

### Reference Genome

The genome FASTA provides the reference genomic sequence:

```text
<PATH_TO_GENOME_FASTA>
```

## Running the Workflow

Open:

```text
generate_all_transcript_probes.ipynb
```

in JupyterLab or another compatible Jupyter environment.

Configure the required parameters and run the notebook cells in order.

The notebook uses the supporting scripts located in:

```text
<PATH_TO_CASSINI_CODE_SET>
```

as specified by `script_dir`.

---

# 5. Probe Generation for Synthetic Sequences

The synthetic-sequence workflow is provided in:

```text
generate_synthetic_probes.ipynb
```

This workflow generates probes for a **synthetic transcript or sequence**.

The synthetic sequence is provided as an input file using `sequence_file`.

The reference **genome and GTF annotation are used to identify homology to genes expressed in the selected species**.

This makes it possible to generate probes for a synthetic sequence while determining its relationship to genes in the reference species.

---

## Workflow Overview

```text
                   Synthetic sequence
                     sequence_file
                          |
                          v
                  Sequence analysis
                          |
               +----------+----------+
               |                     |
               v                     v
        Reference genome       Reference GTF
          genome_file             gtf_file
               |                     |
               +----------+----------+
                          |
                          v
                Identify homologous
                 genes / regions
                          |
                          v
                    Probe design
                          |
                          v
                     Probe set
```

---

## Configuration

The synthetic-sequence notebook uses the following parameters:

```python
gtf_file = "<PATH_TO_GTF_FILE>"

genome_file = "<PATH_TO_GENOME_FASTA>"

sequence_file = "<PATH_TO_SYNTHETIC_SEQUENCE>"

input_dir = "<REFERENCE_INPUT_DIRECTORY>"

output_folder = "<OUTPUT_DIRECTORY>"

template_gtf = "<PATH_TO_CUSTOM_GTF>"

name_custom = "<CUSTOM_SEQUENCE_NAME>"

Python_interpreter = "<PATH_TO_PYTHON>"

script_dir = "<PATH_TO_CASSINI_CODE_SET>"
```

## Parameter Description

| Parameter            | Description                                                                 |
| -------------------- | --------------------------------------------------------------------------- |
| `gtf_file`           | Reference GTF annotation for the species being analyzed.                    |
| `genome_file`        | Reference genome FASTA used for the homology analysis.                      |
| `sequence_file`      | File containing the synthetic sequence.                                     |
| `input_dir`          | Directory containing reference input files.                                 |
| `output_folder`      | Directory where generated probes and intermediate files will be stored.     |
| `template_gtf`       | GTF file used as a template for the custom/synthetic transcript.            |
| `name_custom`        | Name assigned to the synthetic sequence/custom transcript.                  |
| `Python_interpreter` | Path to the Python interpreter/environment used by the workflow.            |
| `script_dir`         | Path to the `cassini_code_set` directory containing the supporting scripts. |

---

## Synthetic Sequence

The synthetic sequence is provided through:

```python
sequence_file = "<PATH_TO_SYNTHETIC_SEQUENCE>"
```

The file contains the sequence for which probes will be generated.

The sequence is assigned a custom name using:

```python
name_custom = "<CUSTOM_SEQUENCE_NAME>"
```

---

## Reference Genome and GTF

The reference genome and GTF should correspond to the species being analyzed.

```text
Reference genome:
    <PATH_TO_GENOME_FASTA>

Reference annotation:
    <PATH_TO_GTF_FILE>
```

The reference genome provides the genomic sequence, while the GTF provides the annotated genes and transcripts.

These references are used to identify genes and genomic regions homologous to the synthetic sequence.

---

## Custom GTF

The synthetic-sequence workflow also uses a custom GTF:

```python
template_gtf = "<PATH_TO_CUSTOM_GTF>"
```

This allows the synthetic sequence to be represented as a custom transcript/feature during the probe-generation workflow.

---

## Running the Workflow

Open:

```text
generate_synthetic_probes.ipynb
```

in JupyterLab or another compatible Jupyter environment.

Configure the input and output parameters and run the notebook cells in order.

The notebook uses the supporting scripts contained in the `cassini_code_set` directory specified by:

```python
script_dir
```

---

# 6. Python Environment

The notebooks require a Python environment containing all dependencies required by the Cassini workflow.

The Python interpreter is specified using:

```python
Python_interpreter = "<PATH_TO_PYTHON>"
```

Verify that the selected Python environment is available before running the notebooks.

For example:

```bash
<PYTHON_INTERPRETER> --version
```

The required dependencies should be installed in this environment.

---

# 7. Reference Files

The reference genome and annotation must be appropriate for the species and genome assembly being analyzed.

A typical reference directory may contain:

```text
reference/
├── genome.fa
├── annotation.gtf
└── custom.gtf
```

## Reference Genome

The genome FASTA provides the reference genomic sequence.

```text
genome.fa
```

## Reference GTF

The GTF provides the gene and transcript annotation.

```text
annotation.gtf
```

## Custom GTF

The synthetic-sequence workflow can use a custom GTF:

```text
custom.gtf
```

---

# 8. Genome and Annotation Compatibility

The reference genome and GTF annotation should correspond to the **same species and genome assembly**.

For example:

```text
Species:    <SPECIES>
Genome:     <GENOME_ASSEMBLY>
Annotation: <GENOME_ANNOTATION>
```

Using an annotation from an incompatible genome assembly may result in incorrect coordinates, failed mappings, or incorrect probe generation.

---

# 9. Output

The output directory is defined using:

```python
output_folder = "<OUTPUT_DIRECTORY>"
```

It is recommended to use separate output directories for different workflows.

For example:

```text
output/
├── canonical/
├── all_transcripts/
└── synthetic/
```

This prevents results from different workflows from being mixed or overwritten.

---

# 10. Workflow Comparison

| Workflow             | Notebook                               | Main sequence source | Reference genome | Reference GTF |
| -------------------- | -------------------------------------- | -------------------- | ---------------- | ------------- |
| Canonical transcript | `generate_canonical_probes.ipynb`      | Reference annotation | Required         | Required      |
| All transcripts      | `generate_all_transcript_probes.ipynb` | Reference annotation | Required         | Required      |
| Synthetic sequence   | `generate_synthetic_probes.ipynb`      | `sequence_file`      | Required         | Required      |

---

# 11. Supporting Scripts

The three notebooks rely on a large collection of supporting scripts.

These scripts are distributed in:

```text
cassini_code_set/
```

The directory contains **hundreds of scripts required by the workflows**.

The supporting scripts should be downloaded together with the repository.

The location of the directory is provided to the notebooks through:

```python
script_dir = "<PATH_TO_CASSINI_CODE_SET>"
```

### Important

Do not remove individual files from `cassini_code_set`.

Even if a script does not appear to be directly used by a notebook, it may be required by another supporting script during execution.

The recommended approach is to keep the complete directory structure unchanged after downloading the repository.

---

# 12. Recommended Repository Structure

The repository should have the following general structure:

```text
cassini/
│
├── README.md
│
├── generate_canonical_probes.ipynb
├── generate_all_transcript_probes.ipynb
├── generate_synthetic_probes.ipynb
│
└── cassini_code_set/
    ├── script_1
    ├── script_2
    ├── script_3
    ├── ...
    └── many additional supporting scripts
```

Reference genomes, annotations, synthetic sequences, and generated probe files do not need to be included in the Git repository.

---

# 13. General Workflow

The overall workflow is:

```text
                         Repository
                             |
                             v
                    Download notebooks
                             |
                             v
                    Download cassini_code_set
                             |
                             v
                  Configure script_dir
                             |
                             v
                    Select a workflow
                             |
            +----------------+----------------+
            |                |                |
            v                v                v
       Canonical        All transcripts    Synthetic
       transcript                         sequence
            |                |                |
            v                v                v
        GTF + genome     GTF + genome    Sequence + GTF
                                        + genome
            |                |                |
            +----------------+----------------+
                             |
                             v
                   Run Jupyter notebook
                             |
                             v
                  Supporting scripts
                    from cassini_code_set
                             |
                             v
                      Probe generation
                             |
                             v
                       Probe output
```

---

# 14. Troubleshooting

## Supporting scripts cannot be found

Check that `script_dir` points to the complete `cassini_code_set` directory:

```python
script_dir = "<PATH_TO_CASSINI_CODE_SET>"
```

Make sure the directory exists and contains the supporting scripts.

## Reference file not found

Check the paths provided to:

```python
gtf_file
genome_file
```

For the synthetic workflow, also check:

```python
sequence_file
template_gtf
```

## Python environment not found

Check:

```python
Python_interpreter = "<PATH_TO_PYTHON>"
```

and verify that the interpreter is available.

## Incorrect genome or annotation

Verify that the genome FASTA and GTF correspond to the same species and genome assembly.

---

# 15. Summary

Cassini probe generation is performed through three Jupyter notebooks:

```text
generate_canonical_probes.ipynb
generate_all_transcript_probes.ipynb
generate_synthetic_probes.ipynb
```

The notebooks are the **three running workflows**.

They rely on a large collection of supporting scripts located in:

```text
cassini_code_set/
```

The complete `cassini_code_set` must be downloaded and its location specified through:

```python
script_dir = "<PATH_TO_CASSINI_CODE_SET>"
```

The three workflows are:

* **Canonical transcript:** generates probes for canonical transcripts.
* **All transcripts:** generates probes for all annotated transcripts.
* **Synthetic sequence:** generates probes for a synthetic sequence. The synthetic sequence is provided through `sequence_file`, while the reference genome and GTF are used to identify homology to genes expressed in the selected species.

All paths in this README are intentionally generic and should be replaced with paths appropriate for the local installation.
