# Project4

Melika & Sid

Below is an overview of the Steps in order to replicate the results of the project:
---

## 1. Installation:

- Install GInPipe: https://github.com/KleistLab/GInPipe
- Install covsonar: https://github.com/rki-mf1/covsonar


## 2. Data Download:

- Download to `Germany` folder:
  - https://osf.io/hxk5m (Germany sequences and daily cases)
    
- Download to `France` folder:
  - https://osf.io/uchtg (covSonar Database with France and Spain sequences)
  - https://osf.io/jptkw (France weekly cases)

- Download to `India` folder:
  - https://drive.google.com/file/d/18UCfhpLJQQiDHWg2Eyv-HUTWsQ4p7SQj/view?usp=sharing (India sequences)
  - https://drive.google.com/file/d/1zVoPcCPlshpmHDyxSzkIrWjQpt5Reelr/view?usp=sharing (Metadata for India sequences)

## 3. Data Analyses:

### Germany
1. Store dates of sequences 
```
cd Germany
cut -d ',' -f1 sequences_Germany_220101_220630.csv > dates_sequences_Germany_220101_220630.csv
```
2. Run `Germany Daily Sequences Histogram` section in DataProcessing.ipynb
3. Run GInPipe
```
mamba activate ../envs/GInPipe3/
snakemake --snakefile ../GInPipe/GInPipe --configfile config_Germany.yaml -j
```

### France
1. Run covSonar and store dates of sequences
```
cd France
mamba activate ../envs/sonar/
python ../covsonar/sonar.py match --db project4.db --date 2022-01-01:2022-06-30 --collection FRANCE > sequences_France_220101_220630.csv
cut -d ',' -f16 sequences_France_220101_220630.csv > dates_sequences_France_220101_220630.csv
```
2. Run `France Daily Sequences Histogram` section in DataProcessing.ipynb
3. Run `France Daily Estimation` section in DataProcessing.ipynb
4. Run GInPipe
```
mamba activate ../envs/GInPipe3/
snakemake --snakefile ../GInPipe/GInPipe --configfile config_France.yaml -j
```

### Spain
1. Run covSonar and store dates of sequences
```
cd Spain
mamba activate ../envs/sonar/
python ../covsonar/sonar.py match --db ../France/project4.db --date 2022-01-01:2022-06-30 --collection SPAIN > sequences_Spain_220101_220630.csv
cut -d ',' -f16 sequences_Spain_220101_220630.csv > dates_sequences_Spain_220101_220630.csv
```
2. Run `Spain Daily Sequences Histogram` section in DataProcessing.ipynb
3. Run `Spain Daily Estimate` section in DataProcessing.ipynb
4. Run GInPipe
```
mamba activate ../envs/GInPipe3/
snakemake --snakefile ../GInPipe/GInPipe --configfile config_Spain.yaml -j
```
  
### India
1. Run covSonar (Recommended to run on HPC, `add` took ~25 minutes with 32 cpus) and store dates of sequences
```
cd India
mamba activate ../envs/sonar/
python ../covsonar/sonar.py add -f sequences_India_220101_220630.fasta --db India2022 --cpus 32
python ../covsonar/sonar.py update --tsv metadata_India_220101_220630.tsv --fields accession=strain date=date --db India2022
python ../covsonar/sonar.py match --db India2022 --date 2022-01-01:2022-06-30  > sequences_India_220101_220630.csv
cut -d ',' -f16 sequences_India_220101_220630.csv > dates_sequences_India_220101_220630.csv
```
2. Run `India Daily Sequences Histogram` section in DataProcessing.ipynb
3. Run `India Daily Estimate` section in DataProcessing.ipynb
4. Run GInPipe
```
mamba activate ../envs/GInPipe3/
snakemake --snakefile ../GInPipe/GInPipe --configfile config_India.yaml -j
```



    
