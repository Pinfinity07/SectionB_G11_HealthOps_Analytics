# Data Dictionary

This document provides a comprehensive overview of all variables used in the **HealthOps Analytics** project. The dataset consists of 24,000 patient records across three hospitals, enriched with derived KPIs and operational indices.

## Core Features (Source Data)

| Column | Type | Description |
|:---|:---|:---|
| `Patient_ID` | String | Unique patient identifier |
| `Age` | Float | Patient age in years |
| `Gender` | String | Patient gender (Male / Female) |
| `Hospital` | String | Hospital name (Medilife, Sunrise, Carepoint) |
| `Hospital_Type` | String | Hospital management type (Government or Private) |
| `Diagnosis` | String | Primary diagnosis category |
| `Treatment_Cost` | Float | Total treatment cost in ₹ INR |
| `Doctor_Experience_Years` | Float | Treating doctor's years of experience |
| `Doctor_Availability` | Integer | Availability score (higher = more available) |
| `Cleanliness_Score` | Integer | Facility cleanliness rating (1-5) |
| `Economic_Status` | String | Patient economic status (Low / Medium / High) |
| `Outcome` | String | Treatment outcome (Recovered / Deceased / Under Treatment) |
| `Insurance` | String | Whether patient is insured (Yes / No) |
| `Region` | String | Geographic region (North / South / East / West) |
| `Hospital_ID` | String | Unique hospital identifier |

## Derived Features (ETL & Cleaning)

| Column | Type | Description |
|:---|:---|:---|
| `Age_Group` | String | Categorical age band (Youth, Adult, Senior) |
| `Cost_Tier` | String | Categorical cost band (Low, Mid, High, Ultra) |

## Analytical Indices & KPI Flags (Final Load Prep)

| Column | Type | Description |
|:---|:---|:---|
| `Recovered_Flag` | Integer | 1 if Outcome is 'Recovered', else 0 |
| `Deceased_Flag` | Integer | 1 if Outcome is 'Deceased', else 0 |
| `Under_Treatment_Flag` | Integer | 1 if Outcome is 'Under Treatment', else 0 |
| `Insured_Flag` | Integer | 1 if Insurance is 'Yes', else 0 |
| `Private_Hospital_Flag` | Integer | 1 if Hospital_Type is 'Private', else 0 |
| `Male_Flag` | Integer | 1 if Gender is 'Male', else 0 |
| `Female_Flag` | Integer | 1 if Gender is 'Female', else 0 |
| `Vulnerable_Patient_Flag` | Integer | 1 if patient is a Senior with Low income and No Insurance |
| `High_Cost_Flag` | Integer | 1 if Treatment_Cost ≥ 90th percentile globally |
| `Financial_Distress_Flag` | Integer | 1 if patient is Uninsured AND cost > overall mean |
| `Operational_Access_Index` | Float | Weighted composite (0–1): 50% Dr. Avail + 30% Cleanliness + 20% Dr. Exp |
| `Quality_Tier` | Category | Performance band based on Access Index (Low/Medium/High/Premium) |

## Statistical Benchmarks

| Column | Type | Description |
|:---|:---|:---|
| `Cost_vs_Overall_Avg` | Float | Deviation from global mean cost (₹) |
| `Cost_Pct_vs_Overall_Avg` | Float | Percentage deviation from global mean cost |
| `Cost_vs_Diagnosis_Avg` | Float | Deviation from mean cost within the same diagnosis (₹) |
| `Cost_vs_Hospital_Avg` | Float | Deviation from mean cost within the same hospital (₹) |
| `Cost_Percentile_Rank` | Float | Global percentile rank of cost (0–100) |
| `Hospital_Patient_Volume` | Integer | Total patient count for the respective hospital in this dataset |
