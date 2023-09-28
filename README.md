# Project4

Melika & Sid

Below is an overview of the Steps in order to replicate the results of the project. 
---

### 1. Installation:

- Install GInPipe: https://github.com/KleistLab/GInPipe
- Install covsonar: https://github.com/rki-mf1/covsonar


### 2. Data Download:

- Download to Germany folder:
  - https://osf.io/hxk5m (Germany sequences and daily cases)
    
- Download to France folder:
  - https://osf.io/uchtg (covSonar Database with France and Spain sequences)
  - https://osf.io/jptkw (France weekly cases)

- Download to India folder
  - (India sequences)

### 3. Data Analyses:

Germany
- Run "Germany Daily Sequences Histogram" in DataExploration.ipynb
- cd Germany
- Change config_Germany.yaml to match paths (if necessary)
- mamba activate ../envs/GInPipe3/
- snakemake --snakefile ../GInPipe/GInPipe --configfile config_Germany.yaml -j

France
- cd France
- mamba activate ../envs/sonar/
- python ../covsonar/sonar.py match --db project4.db --date 2022-01-01:2022-06-30 --collection FRANCE > sequences_France_220101_220630.csv
- cut -d ',' -f16 sequences_France_220101_220630.csv > dates_sequences_France_220101_220630.csv
- Run "France Daily Sequences Histogram" in DataExploration.ipynb
- Run "France Daily Estimation" in DataExploration.ipynb
- mamba activate ../envs/GInPipe3/
- snakemake --snakefile ../GInPipe/GInPipe --configfile config_France.yaml -j

Spain
- cd Spain
- mamba activate ../envs/sonar/
- python ../covsonar/sonar.py match --db ../France/project4.db --date 2022-01-01:2022-06-30 --collection SPAIN > sequences_Spain_220101_220630.csv
- cut -d ',' -f16 sequences_Spain_220101_220630.csv > dates_sequences_Spain_220101_220630.csv
- Run "Spain Daily Sequences Histogram" in DataExploration.ipynb
- Run "Spain Daily Estimate" in DataExploration.ipynb
- mamba activate ../envs/GInPipe3/
- snakemake --snakefile ../GInPipe/GInPipe --configfile config_Spain.yaml -j
  
India
- cd India
- mamba activate ../envs/sonar/
- Recommended to run on HPC: python ../covsonar/sonar.py add -f sequences_India_220101_220630.fasta --db India2022 --cpus 32
- python ../covsonar/sonar.py match --db India2022 --date 2022-01-01:2022-06-30  > sequences_India_220101_220630.csv
- cut -d ',' -f16 sequences_India_220101_220630.csv > dates_sequences_India_220101_220630.csv
- Run "India Daily Sequences Histogram" in DataExploration.ipynb
- Run "India Daily Estimate" in DataExploration.ipynb
- mamba activate ../envs/GInPipe3/
- snakemake --snakefile ../GInPipe/GInPipe --configfile config_India.yaml -j



    
