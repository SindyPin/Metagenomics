# Metagenomics: MAG (Metagenome-Assembled Genome)

# Basic Protocol 1: MAG Data Processing Workflow

This README outlines the steps to set up and run a basic protocol for MAG data processing using `sra-tools`, `fastp`, and `FastQC`. These tools are critical for fetching, cleaning, and validating the quality of sequencing data. Each tool/package plays a specific role in ensuring the integrity and usability of the sequence data for downstream analysis.

## Requirements
- macOS
- Conda (Miniconda or Anaconda)
- Command-line (Terminal)

## Environment Setup

### Step 1: Create and Activate Conda Environment
Conda is a widely-used environment management system, essential for managing dependencies, isolating project environments, and ensuring reproducibility.

```bash
conda create -n Basic_protocol_1
conda activate Basic_protocol_1
```

### Step 2: Configure Conda Channels
Adding the right Conda channels is important because some bioinformatics tools are hosted on specific repositories (like `bioconda` and `conda-forge`). These repositories are curated to ensure compatibility and updates for bioinformatics tools.

```bash
conda config --add channels bioconda
conda config --add channels conda-forge
```

### Step 3: Install Required Tools

#### `sra-tools`
- **Purpose**: `sra-tools` is essential for retrieving sequence data from the Sequence Read Archive (SRA), a large repository of publicly available next-generation sequencing data.
- **Why it's important**: It provides easy access to raw sequencing data (in `.sra` format), and its tools like `prefetch` and `fasterq-dump` are indispensable for converting `.sra` files into usable FASTQ files.
```bash
conda install -c bioconda sra-tools==3.0.8
```

#### `fastp`
- **Purpose**: `fastp` is a highly efficient tool for quality control and preprocessing of FASTQ files. It performs functions like adapter trimming, filtering by quality, and basic data analysis.
- **Why it's important**: Ensuring high-quality sequence data is crucial before downstream analyses such as assembly or mapping. `fastp` automates the trimming and filtering process, which improves the reliability of the data.
```bash
conda install -c bioconda fastp==0.23.4
```

### Step 4: Prepare Workspace
Create a directory to store sequence data and quality check results:
```bash
mkdir MAG
cd MAG
```

### Step 5: Download Sequence Data
Using `sra-tools` to download data directly from the SRA repository:

1. **`prefetch`**: Downloads the raw `.sra` files from the SRA repository.
2. **`fasterq-dump`**: Converts `.sra` files into FASTQ format, which is the standard input format for most sequence processing tools. The `--split-files` flag ensures that paired-end reads are split into two separate files, and `--skip-technical` ignores technical reads that do not contribute to biological information.

```bash
prefetch SRR23604271 SRR23604268

fasterq-dump SRR23604271 --split-files --skip-technical
fasterq-dump SRR23604268 --split-files --skip-technical
```

## FastQC Installation

### What is FastQC and Why it's Important
- **Purpose**: FastQC is a tool for quality control of raw sequence data. It generates comprehensive reports with metrics like sequence quality scores, GC content, overrepresented sequences, and adapter content.
- **Why it's important**: Assessing the quality of sequence data is critical before any further analysis. FastQC provides a quick overview to identify any issues such as low-quality reads or contamination, ensuring the reliability of the dataset for downstream processes.

#### Installation Instructions
FastQC is not available directly via Conda for macOS, so it needs to be downloaded manually:

1. Visit the [FastQC download page](https://www.bioinformatics.babraham.ac.uk/projects/download.html#fastqc) and download **FastQC v0.12.1 (Mac DMG image)**.
2. Mount the `.dmg` file and drag the FastQC application to the `Applications` folder.
3. Unmount the `.dmg` after installation.

### Add FastQC to Your PATH
To run FastQC from the command line in your conda environment or system-wide, you need to add it to your PATH variable.

1. Open Terminal and add FastQC to your PATH by adding this line to your `~/.bash_profile` or `~/.zshrc` file:
   ```bash
   export PATH=$PATH:/Applications/FastQC.app/Contents/MacOS/
   ```
2. Reload your shell configuration:
   ```bash
   source ~/.bash_profile
   ```
   Or, if you use Zsh:
   ```bash
   source ~/.zshrc
   ```

### Verify FastQC Installation
Verify that FastQC has been added to your PATH:
```bash
which fastqc
```
Expected output:
```
/Applications/anaconda3/envs/Basic_protocol_1/bin/fastqc
```

Check the version of FastQC:
```bash
fastqc --version
```
Expected output:
```
FastQC v0.12.1
```

### Fix Permissions (if necessary)
If you encounter issues running FastQC, you may need to make the application executable:
```bash
chmod +x /Applications/FastQC.app/Contents/MacOS/fastqc
```

Run FastQC from any directory by simply typing:
```bash
fastqc
```

## Resources

- [Conda Documentation](https://docs.conda.io/projects/conda/en/latest/)
- [SRA Toolkit Documentation](https://github.com/ncbi/sra-tools/wiki)
- [fastp Documentation](https://github.com/OpenGene/fastp)
- [FastQC Download Page](https://www.bioinformatics.babraham.ac.uk/projects/download.html#fastqc)

## Expected Outputs
- **Prefetch output**: `.sra` files downloaded from the SRA.
- **Fasterq-dump output**: Split FASTQ files (e.g., `SRR23604271_1.fastq`, `SRR23604271_2.fastq`).
- **Fastp output**: Cleaned FASTQ files (e.g., `SRR23604271_1_clean.fastq`, `SRR23604271_2_clean.fastq`).
- **FastQC output**: Quality control reports (`.html` and `.zip` files) summarizing sequence quality metrics.

## Notes
- Ensure that Conda is correctly installed on your system before proceeding.
- Always make sure your Conda environment is activated (`conda activate Basic_protocol_1`) when running commands.
- If FastQC is not recognized in your PATH, revisit the steps for adding it to your PATH.

### Key Explanations:
- **Conda**: Manages environments and dependencies to ensure tools don't conflict with each other.
- **sra-tools**: Essential for fetching publicly available sequence data from SRA.
- **fastp**: Critical for cleaning sequence data, ensuring the highest quality input for downstream analysis.
- **FastQC**: Ensures the quality of sequence data, allowing you to spot issues early on.
