# Wesleyan Media Project - Data Post Production 

Welcome! This repo is part of the Cross-platform Election Advertising Transparency initiatIVE (CREATIVE) project. CREATIVE is a joint infrastructure project of WMP and privacy-tech-lab at Wesleyan University. CREATIVE provides cross-platform integration and standardization of political ads collected from Google and Facebook.

This repo is a part of the data storage and processing step.
## Table of Contents
- [Introduction](#introduction)

- [Objective](#objective)

- [Data](#data)

- [Setup](#setup)

## Introduction
This repo contains code that allows for merging variables derived from different processing steps and final data cleaning, with a particular focus on deduplication. 

Specifically it contains two scripts. The first makes sure that the variables particular to specific ad types (text, video and image ads) are added in after loading the metadata masterfile that contains metadata for all ad types. The second drops duplicates using a collection of variables appropriate for research objectives through creating a unique ID for each piece of unique ad content, assigning group indices for all data points and then prefixing these indices and generating unique IDs in text strings. 
## Objective
Each of our repos belongs to one or more of the the following categories:
- Data Collection
- Data Storage & Processing
- Preliminary Data Classification
- Final Data Classification

This repo is part of the Data Storage & Processing step. 
## Data
The data created by both scripts in this repo is in a .csv format. 

An individual record created by `merge results.ipynb` contains the following fields:
'index', 'ad_id', 'ad_url', 'ad_type', 'advertiser_id','advertiser_name', 'date_range_start', 'date_range_end', 'num_of_days','impressions', 'age_targeting', 'gender_targeting','geo_targeting_included', 'geo_targeting_excluded', 'spend_range_min_usd', 'spend_range_max_usd'.

Along with additional fields depending on what kind of ad data is being processed. 

For text ads, the following fields are merged: 
'ad_id', 'ad_title', 'ad_text', 'url', 'all_urls'.  

For video ads, the following fields are merged:
google_asr_text', 'aws_ocr_video_text', 'aws_face_vid'.

For ads that are images, the following fields are merged: 
'aws_ocr_img_text', 'aws_face_vid'. 

`Deduplication.ipynb` returns all of the fields that are input, with deduplication being done on them.  
## Setup
### 1. Install Relevant Software
Before running any of the code in this repo, make sure you have Python installed on your system. You can do so on the [official Python website](https://www.python.org/downloads/). In addition, install Jupyter Notebook by writing the following command in your terminal 'pip install jupyter'. From here, you should be able to run Jupyter Notebook by entering this command in your terminal 'jupyter notebook'.   

### 2. Install Dependencies 
Prior to running the scripts in this repo, please install the following dependency 
'pip install pandas'.  

### 3. Run the Scripts 
In order to run the scrupts, keep in mind that `merge results.ipynb` should be run prior to `Deduplication.ipynb`.

Prior to running `merge results.ipynb`, you will have to change the line of code `df = pd.read_csv(my_metadata_filepath)` to match up with your metadata filepath. Prior to running `Deduplication.ipynb`, you will have the change the line of code `df = pd.read_csv('my-final-table.csv')` to match with your final table filepath. 

After running `merge results.ipynb`, the results will be saved at whatever file is indicated by the line of code `outfilepath = 'outfile.csv'`. 