

This directory includes the bash and PBS scripts for downloading and preprocessing the sequencing samples for analysis throughout the rest of this repo.


The following scripts should be executed in numerical order. The user should update the working directory (`$WRK`) filepath variable within every script before executing.

### Download ScriptManager
Make sure you download ScriptManager and move it to `/path/to/2023-Breugel_Fpt1/bin/`.
```
wget https://github.com/CEGRcode/scriptmanager/releases/download/v0.14/ScriptManager-v0.14.jar
mv ScriptManager-v0.14.jar ../bin/
```

### Download Master NoTag from YEP (yeast)
Download and index the control NoTag sample used in Rossi et al, 2021 (Nature).
```
wget https://www.datacommons.psu.edu/download/eberly/pughlab/yeast-epigenome-project/masterNoTag_20180928.zip
unzip masterNoTag_20180928.zip
mv masterNoTag_20180928.bam data/BAM/
samtools index data/BAM/masterNoTag_20180928.bam
rm masterNoTag_20180928.zip
```

### Download YEP Blacklist (yeast)
Download the normalization blacklist used in Rossi et al, 2021 (Nature).
```
wget https://raw.githubusercontent.com/CEGRcode/2021-Rossi_Nature/master/02_References_and_Features_Files/ChExMix_Peak_Filter_List_190612.bed
mv ChExMix_Peak_Filter_List_190612.bed ../data/
```

### 1_download_data.sh
- the ScriptManager binary executable, saved to `2023-Breugel_Fpt1/bin/`
- the Master NoTag control sample (Rossi, 2021), saved to `2023-Breugel_Fpt1/data/BAM`
- All ChIP-exo samples (`sample_ids.txt`), `*.fastq.gz` files saved to `2023-Breugel_Fpt1/data/FASTQ` while `*.bam` files saved to `2023-Breugel_Fpt1/data/BAM`

### 2_align_samples.pbs
This PBS submission script uses the FASTQ files downloaded in `1_download_files.sh` to create BAM alignments using Hisat2. BAM alignments are sorted and indexed using samtools.

This submission script is compatible with systems running the PBS scheduler.

Update file path and allocation name using your favorite text editor, then run:
```
qsub 2_align_rna_samples.pbs

### 3_normalize_samples.sh
Calculate scaling factor for every ChIP-exo sample and save the files to `data/NormalizationFactors/XXXX_ScalingFactors.out`.
- ChIP-exo samples: use ScriptManager's implementation of the NCIS (Liang & Keles 2012, *BMC Bioinformatics*))
