# Wesleyan Media Project - Data Post Production 

Welcome! This repo is part of the Cross-platform Election Advertising Transparency initiatIVE (CREATIVE) project. CREATIVE is a joint infrastructure project of WMP and privacy-tech-lab at Wesleyan University. CREATIVE provides cross-platform integration and standardization of political ads collected from Google and Facebook.

This repo is a part of the data storage and processing step.
## Table of Contents
- [Introduction](#introduction)

- [Objective](#objective)

- [Data](#data)

- [Setup](#setup)

## Introduction
This repo contains code that allows for merging different data fields and final data cleaning. 


Specifically it contains two key tasks. The first task merges variables derived from different pre-processing and classification steps. There are two components in the "merging variables" task. The first component concatentes text, image and video ads, information extracted from these ads and ad metadata. The merged results then go through various classification tasks located in other repos to produce additional variables. The second component merges these additional variables into the final output. The second task deduplicates ads that share the exact same creative content. 


## Objective
Each of our repos belongs to one or more of the the following categories:
- Data Collection
- Data Storage & Processing
- Preliminary Data Classification
- Final Data Classification

This repo is part of the Data Storage & Processing step. 

## Data
The data created by this repo is in the gzip format (gzip compressed .csv files). 

In the merging results task (specified in the folder `01-merging-results`), three output tables can be created for each platform. 

For Facebook ads, the final output tables are: 

+ fb_2022_adid_text.csv.gz
- fb_2022_adid_var1.csv.gz
+ fb_2022_adid_var.csv.gz

For Google ads, the final output tables are: 
+ g2022_adid_01062021_11082022_text_v20231203.csv
- g2022_adid_01062021_11082022_var1_v20231203.csv
+ g2022_adid_var_121523.csv


All tables are at the ad_id level. The first table for each platform primarily contains text fields, such as sponsor names, ad URLs, speech and text extracted from videos and images. The second table contains non-text fields queried or extracted from the data collection and preprocessing steps, such as ad spending, dates of ads being run, demographic targeting information. These two tables are produced in `01-merging-results/merge_preprocessed_multimedia_results.ipynb`. The input data came from [image-and-video-data-preparation](https://github.com/Wesleyan-Media-Project/image-video-data-preparation) and [aws-rekognition-image-video-processing](https://github.com/Wesleyan-Media-Project/aws-rekognition-image-video-processing). The third table is produced in the folder `merge_final_classification_results`. It adds additional variables into the second table. Specifically, this task merges output from race of focus, ad tone, ad goal, party classifier, entity linking, attack like, aspect-based sentiment analysis, and issue classifiers (INSERT LINKS TO REPOS). All of these repos for data classification take the first two tables as input. 


`Deduplication.ipynb` identifies exact duplicate ads based on text fields and creates unique creative identifiers. This is optional and customizable based on research objectives. 


## Setup
### 1. Install Relevant Software
Before running any of the code in this repo, make sure you have Python installed on your system. You can do so on the [official Python website](https://www.python.org/downloads/). In addition, install Jupyter Notebook by writing the following command in your terminal 'pip install jupyter'. From here, you should be able to run Jupyter Notebook by entering this command in your terminal 'jupyter notebook'.   

### 2. Install Dependencies 
Prior to running the scripts in this repo, please install the following dependency 
`pip install pandas`.  

### 3. Run the Scripts 
In order to run the scripts, keep in mind that `01-merging-results` should be run prior to `02-deduplication`, `01-merging-results/01_merge_preprocessed_results` prior to `01-merging-results/02_merge_final_classification_results`, and that `01-merging-results` requires output from multiple data collection, preprocessing and classification repos being linked above. 

Prior to running these scripts, you will have to change input and output file paths to match up with your local file paths. 
