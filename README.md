# CREATIVE --- Data Post Production

Welcome! This repo contains scripts for processing content from textual and audiovisual ads, specifically, with regards to merging data fields and performing final data cleaning.

This repo is a part of the [Cross-platform Election Advertising Transparency Initiative (CREATIVE)](https://www.creativewmp.com/). CREATIVE is an academic research project that has the goal of providing the public with analysis tools for more transparency of political ads across online platforms. In particular, CREATIVE provides cross-platform integration and standardization of political ads collected from Google and Facebook. CREATIVE is a joint project of the [Wesleyan Media Project (WMP)](https://mediaproject.wesleyan.edu/) and the [privacy-tech-lab](https://privacytechlab.org/) at [Wesleyan University](https://www.wesleyan.edu).

To analyze the different dimensions of political ad transparency we have developed an analysis pipeline. The scripts in this repo are part of the Data Processing step in our pipeline.

![A picture of the pipeline diagram](CREATIVE_step2_032524.png)

## Table of Contents

[1. Introduction](#1-introduction)  
[2. Data](#2-data)  
[3. Setup](#3-setup)  
[4. Thank You!](#4-thank-you)

## 1. Introduction


This repo contains scripts for merging different data fields and final data cleaning.

Specifically, the scripts in this repo perform two key tasks:

1. `01-merging_results`: Merge variables derived from different pre-processing and classification steps. There are two components:

* (1). `01_merge_preprocessed_results`: This step corresponds to "Step 2a: Merge Ad Content and Metadata" in the pipeline diagram. This is performed after you extracted textual content fields from audiovisual ads in the following repos: 
- [image-and-video-data-preparation](https://github.com/Wesleyan-Media-Project/image-video-data-preparation)
- [automatic-speech-recognition](https://github.com/Wesleyan-Media-Project/automatic-speech-recognition)
- [aws-rekognition-image-video-processing](https://github.com/Wesleyan-Media-Project/aws-rekognition-image-video-processing)

* This step concatenates text, image and video ads, textual fields extracted from these ads, ad metadata and produces two final output tables (The first two output tables for Facebook and Google respectively listed in the next section). 



(2). `02_merge_final_classification_results`: This step correponds to "Step 3a: Merge Classification Results" in the pipeline diagram. The two final output tables from the above step will first go through data classification tasks -- Step 3 in our pipeline diagram -- to produce additional variables. Then these new variables are merged into the third and last final output table for Google and Facebook here. The data classification tasks for pipeline Step 3 are located in [entity_linking_2022](https://github.com/Wesleyan-Media-Project/entity_linking_2022), [attack_like](https://github.com/Wesleyan-Media-Project/attack_like), [ABSA](https://github.com/Wesleyan-Media-Project/ABSA), [race_of_focus](https://github.com/Wesleyan-Media-Project/race_of_focus), [party_classifier](https://github.com/Wesleyan-Media-Project/party_classifier), [ad_tone](https://github.com/Wesleyan-Media-Project/ad_tone), [ad_goal_classifier](https://github.com/Wesleyan-Media-Project/ad_goal_classifier), [party_classifier_pdid](https://github.com/Wesleyan-Media-Project/party_classifier_pdid), and [issue_classifier](https://github.com/Wesleyan-Media-Project/issue_classifier)).  

2. `02-deduplication`: The second task deduplicates ads that share the exact same creative content and outputs a mapping between ad identifiers (`ad_id`) and unique creative identifiers (`cid` or `wmp_creative_id`). This step is performed after the above step of merging final results, described in 1(2). The resulting id mapping tables also belong to the final data output.  

See Wiki pages for an explanation of variables in var tables for [Facebook](https://github.com/Wesleyan-Media-Project/data-post-production/wiki/Variables-in-the-Facebook-Dataset) and [Google](https://github.com/Wesleyan-Media-Project/data-post-production/wiki/Variables-in-the-Google-Dataset)

## 2. Data

The data created by this repo is in the gzip format (gzip compressed .csv files) or the csv format.

### 2.1 Merging Preprocessed and Final Classification Results

In the merging results task (specified in the folder `01-merging-results`), three output tables can be created for each platform.

For Facebook 2022 ads, the final output tables are:

- fb_2022_adid_text.csv.gz
- fb_2022_adid_var1.csv.gz
- fb_2022_adid_var.csv.gz

For Google 2022 ads, the final output tables are:

- g2022_adid_01062021_11082022_text.csv
- g2022_adid_01062021_11082022_var1.csv
- g2022_adid_var.csv

All tables are at the ad_id level. The first table for each platform primarily contains text fields, such as sponsor names, ad URLs, speech and text extracted from videos and images. The second table contains non-text fields queried or extracted from the data collection and preprocessing steps, such as ad spending, dates of ads being run, demographic targeting information. These two tables are produced in `01-merging-results/01_merge_preprocessed_results`. The input data comes from [image-and-video-data-preparation](https://github.com/Wesleyan-Media-Project/image-video-data-preparation), [automatic-speech-recognition](https://github.com/Wesleyan-Media-Project/automatic-speech-recognition) and [aws-rekognition-image-video-processing](https://github.com/Wesleyan-Media-Project/aws-rekognition-image-video-processing). The third table is produced in the folder `01-merging-results/02_merge_final_classification_results`. It adds additional variables into the second table. Specifically, this task merges output from [entity_linking_2022](https://github.com/Wesleyan-Media-Project/entity_linking_2022), [attack_like](https://github.com/Wesleyan-Media-Project/attack_like), [ABSA](https://github.com/Wesleyan-Media-Project/ABSA), [race_of_focus](https://github.com/Wesleyan-Media-Project/race_of_focus), [party_classifier](https://github.com/Wesleyan-Media-Project/party_classifier), [ad_tone](https://github.com/Wesleyan-Media-Project/ad_tone), [ad_goal_classifier](https://github.com/Wesleyan-Media-Project/ad_goal_classifier), [party_classifier_pdid](https://github.com/Wesleyan-Media-Project/party_classifier_pdid), and [issue_classifier](https://github.com/Wesleyan-Media-Project/issue_classifier). All of these repos for data classification take the first two tables (of Facebook and Google ads respectively) as input.

### 2.2 Identifying Duplicate Content

`02-deduplication/Deduplication.ipynb` identifies exact duplicate ads based on text fields and creates unique creative identifiers. This is optional and customizable based on research objectives.

The output tables for this task are the mapping between ad_id and the new unique creative identifiers (referred to as "cid" or "wmp_creative_id"). Ads that share the exact same creative content have the same unique creative identifiers.

Output table for Facebook 2022 ads:

- cid_fb2022.csv

Output table for Google 2022 ads:

- cid_google2022.csv

## 3. Setup

### 3.1 Install Relevant Software

Before running any of the code in this repo, make sure you have Python installed on your system. You can do so on the [official Python website](https://www.python.org/downloads/). In addition, install Jupyter Notebook with the following command in your terminal

```bash
pip install jupyter
```

Now, you can run Jupyter Notebook with:

```bash
jupyter notebook
```

### 3.2 Install Necessary Dependencies and Repo Output

Prior to running the scripts in this repo, please install the following dependency:

```bash
pip install pandas
```

This repo primarily merges and deduplicates variables. Python's pandas and numpy libraries are sufficient for setup.
 
`01-merging-results/01_merge_preprocessed_results` requires output from [image-and-video-data-preparation](https://github.com/Wesleyan-Media-Project/image-video-data-preparation), [automatic-speech-recognition](https://github.com/Wesleyan-Media-Project/automatic-speech-recognition) and [aws-rekognition-image-video-processing](https://github.com/Wesleyan-Media-Project/aws-rekognition-image-video-processing).

`01-merging-results/02_merge_final_classification_results` requires output from [entity_linking_2022](https://github.com/Wesleyan-Media-Project/entity_linking_2022), [attack_like](https://github.com/Wesleyan-Media-Project/attack_like), [ABSA](https://github.com/Wesleyan-Media-Project/ABSA), [race_of_focus](https://github.com/Wesleyan-Media-Project/race_of_focus), [party_classifier](https://github.com/Wesleyan-Media-Project/party_classifier), [ad_tone](https://github.com/Wesleyan-Media-Project/ad_tone), [ad_goal_classifier](https://github.com/Wesleyan-Media-Project/ad_goal_classifier), [party_classifier_pdid](https://github.com/Wesleyan-Media-Project/party_classifier_pdid), and [issue_classifier](https://github.com/Wesleyan-Media-Project/issue_classifier).

### 3.3 Run the Scripts

In order to run the scripts, note that `01-merging-results` should be run prior to `02-deduplication`, `01-merging-results/01_merge_preprocessed_results` prior to `01-merging-results/02_merge_final_classification_results`, and that `01-merging-results` requires output from multiple data collection, preprocessing and classification repos which are linked above.

Prior to running these scripts, you will have to change input and output file paths to match up with your local file paths.

## 4. Thank You

<p align="center"><strong>We would like to thank our supporters!</strong></p><br>

<p align="center">This material is based upon work supported by the National Science Foundation under Grant Numbers 2235006, 2235007, and 2235008.</p>

<p align="center" style="display: flex; justify-content: center; align-items: center;">
  <a href="https://www.nsf.gov/awardsearch/showAward?AWD_ID=2235006">
    <img class="img-fluid" src="nsf.png" height="150px" alt="National Science Foundation Logo">
  </a>
</p>

<p align="center">The Cross-Platform Election Advertising Transparency Initiative (CREATIVE) is a joint infrastructure project of the Wesleyan Media Project and privacy-tech-lab at Wesleyan University in Connecticut.

<p align="center" style="display: flex; justify-content: center; align-items: center;">
  <a href="https://www.creativewmp.com/">
    <img class="img-fluid" src="CREATIVE_logo.png"  width="220px" alt="CREATIVE Logo">
  </a>
</p>

<p align="center" style="display: flex; justify-content: center; align-items: center;">
  <a href="https://mediaproject.wesleyan.edu/">
    <img src="wmp-logo.png" width="218px" height="100px" alt="Wesleyan Media Project logo">
  </a>
</p>
