---
layout: default
title: ICU Modeling
parent: BIOS823
nav_order: 6
---

# Modeling ICU Deaths  
## Background & Motivation  
The Intensive Care Unit (ICU) is a specialized treament department within hospitals that provide critical medical care and life support to patients. Deaths in the ICU prove to be a problematic challenge that healthcare reserachers and professionals try preventing and forecasting ( [Zimmerman JE, et. al 2013](https://pubmed.ncbi.nlm.nih.gov/23622086/), [Young MP, et. al 2000](https://pubmed.ncbi.nlm.nih.gov/11151525/), [USF Health Policy Reserach](https://healthpolicy.ucsf.edu/icu-outcomes), [Braber A, et. al 2010](https://pubmed.ncbi.nlm.nih.gov/20386386/),  [Wu AW, et. al 2002](https://pubmed.ncbi.nlm.nih.gov/12096371/) ).   

This project aims to build upon previous literature to 1) *develop an algorithm* that accurately provides risk of death during an ICU visit, and 2) *deploys the algorithm* via a dashboard for healthcare specialists that outputs probability of death during an ICU stay. This project uses the **Medical Information Mart for Intensive Care** ([MIMIC](https://mimic.mit.edu/)) as its data source for analysis and modeling.  
Workflow is organized into four parts: 1) Data Extraction, 2) Feature Engineering, 3) Model Development, and 4) Model/Dashboard Deployment.   

## Table of Contents
1. [Data Extraction](#de)
2. [Feature Engineering](#fe)
3. [Model Development](#md)
4. [Model/Dashboard Deployment](#dash)

## Data Extraction <a name="de"></a>     
All group members first requested permission to the full [MIMIC III](https://mimic.mit.edu/) dataset. We also downloaded and utilized the [demo data](https://physionet.org/content/mimiciii-demo/1.4/) as a proof-of-concept for the project pipeline.  
This analyses utilizes and integrates four datasets to model ICU deaths. These are:  
1. [ICU Stays](https://physionet.org/content/mimiciii-demo/1.4/ICUSTAYS.csv)   
Provides information regarding the ICU stay such as first ICU care unit, ward number, and length of stay during the ICU.  
2. [Admissions](https://physionet.org/content/mimiciii-demo/1.4/ADMISSIONS.csv)   
Provides insurance status, admit status, admission diagnosis, and death flag (used as outcome)  
3. [Prescriptions](https://physionet.org/content/mimiciii-demo/1.4/PRESCRIPTIONS.csv)  
Provides prescription type administered, prescription name, and prescription dosage. 
4. [Procedures](https://physionet.org/content/mimiciii-demo/1.4/PROCEDUREEVENTS_MV.csv)   
Provides procedural information for patients. Procedural groupings such as "Invasive Lines", "General Procedures", "Ventilation", and "Imaging" are used to make counts of procedures for each patient during their ICU stay.   

The below table shows the differences in data sizes between the demo data and the full data.   

| Table Name      | Demo Table      | Full Data      |
| ----------- | ----------- | ----------- |
| ICU Stays      | 13.2 KB       | 1.9MB       |
| Admissions   | 26.2 KB        | 2.4 MB       |
| Prescriptions      | 1.6 MB       | 98.7 MB       |
| Procedures   | 131.1 KB        | 7.5 MB       |
      
Due to the size differences between the demo table and the full data, we began our data downloading, pipeline building, exploratory analysis, and prelimiary model fitting on the demo data. We then utilized GoogleBigQuery, GoogleCloud, and SQL databases to download, store, and use the full data. This allowed the group to create pipelines with the demo data and then migrate the pipeline to the full data.  

## Feature Engineering <a name="fe"></a>        
The *ICU Stays* data table acts as the base dataframe for the model. Each row represents an ICU stay. The *Admissions*, *Prescriptions*, and *Procedures* data tables are then used to append to the ICU Stays data.  
The Admissions table 

The *Prescriptions* demo table was first used to find the top twenty drugs administered. The list of top twenty drugs were then used as columns for the analytic dataset. The below code creates the static list and pickles it for later use.  
```python 
mycounts = drugdf.groupby(['subject_id', 'icustay_id', 'formulary_drug_cd']).size().reset_index(name='counts')
mycounts_long = mycounts.pivot(index = ['subject_id','icustay_id'], 
                               columns = 'formulary_drug_cd', values = 'counts').reset_index()
                               with open("../../crosstables/prescription_list.txt", "wb") as dl: 
    pickle.dump(prescription_list, dl)
```

In a similar fashion, the *Procedures* demo table was used to find the top twenty drugs administered.  


what are they linked by?  

Columns used and columns created  
list columns created from each table  
dtypes and counts  
merges performed     
create final analytic dataset with the demo data    
Do the same thing with the full data using Google big query, etc.  

## Model Development <a name="md"></a>      
Dummy model, other models, output metrics, output plots, etc.  

## Model & Dashboard Deployment <a name="dash"></a>       

  
Streamlit and end user capabilities.  