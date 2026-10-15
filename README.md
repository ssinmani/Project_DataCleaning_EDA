# Hospital Readmission EDA: Diabetes 130-US Hospitals

## Overview
This data covers hospital visits of diabetic patients from 130 US hospitals, 1999–2008.
I am exploring the dataset to find out which factors are linked to a patient coming back within 30 days.
This can help hospitals see which patients, such as certain age groups, come back often and need special care, and reduce the cost of repeat visits
This project covers cleaning the data and exploring it with charts and preparing a clean dataset for a prediction model.
## Data
- Source: Diabetes 130-US Hospitals (1999–2008), UCI Machine Learning Repository
- Files: diabetic_data.csv, IDS_mapping.csv
- 101,766 hospital visits, 50 columns, 71,518 unique patients
- Citation: Clore, J., Cios, K., DeShazo, J., & Strack, B. (2014). UCI ML Repository. https://doi.org/10.24432/C5230J (CC BY 4.0)

## Cleaning decisions

Column	Missing	Decision	Reason
weight	96.9%	Drop column	Almost no patient has a recorded weight.
payer_code	39.6%	Drop column	How a patient pays does not describe their health, and 40% is unknown.
medical_specialty	49.1%	Keep, fill "?" with "Unknown"	The type of doctor (for example surgery vs. general medicine) may be linked to how sick patients are, and so to readmission.
race	2.2%	Drop rows with "?"	Very few rows are missing, so removing them loses little data.
diag_1, diag_2, diag_3	0.02–1.4%	Keep, fill "?" with "Unknown"	Diagnoses can help explain readmission, and very few values are missing.
gender	3 rows "Unknown/Invalid"	Keep	Only 3 rows, too few to affect results.
encounter_id	0%	Keep for now, drop after keeping one visit per patient	It is unique, so I use it to sort visits and find each patient's first visit. It does not explain readmission.
max_glu_serum, A1Cresult	NaN = test not done	Fill NaN with "None"	NaN here is real information, not missing data.
add Codeadd Markdown

**Other decisions:**
- Missing values were marked as "?", not blank. Replaced with "Unknown" or dropped.
- "None" in max_glu_serum / A1Cresult means the test was not done, so it was kept as its own category.
- Removed patients who died or went to hospice (discharge codes 11, 13, 14, 19, 20, 21): they could never be readmitted.
- Kept one visit per patient (the first):due to some patients had many visits, so they would be counted many times and skew the charts.
**Data flow:**
101,766 → 99,493 (unknown race) → 97,109 (died/hospice) → 68,167 (one visit per patient)

## Target
readmitted_30: 1 = readmitted within 30 days, 0 = not. After cleaning: 9.04% (it was 11.16% before cleaning).

## Charts
1. Are older patients readmitted more often?
2. Do patients with more past hospital stays come back more?
3. Does a longer hospital stay mean more readmissions?
4. Does the A1C test result relate to readmission?
5. Are patients whose diabetes medicine was changed readmitted more?

## Key insights
Key insights
Past hospital stays are the strongest sign. Patients with no hospital stays in the previous year were readmitted 8.2% of the time, but patients with 3 or more stays were readmitted 26.5% of the time, more than 3 times higher.

Older patients come back more often. The readmission rate is about 7% for ages 20–60 and rises to 10.8% for ages 80–90.

Longer stays are linked to more readmissions. The rate rises from 6.5% for a 1-day stay to about 12.4% for an 8-day stay, then levels off. A longer stay probably means the patient was sicker, not that the stay itself causes readmission.

The A1C result made little difference. All groups were between 8.3% and 9.2%. Surprisingly, patients who were never tested had the highest rate (9.2%), and patients with a high result (>8) had the lowest (8.3%), possibly because a high result leads doctors to adjust treatment.

A medication change shows only a small difference. 9.5% of patients whose diabetes medicine was changed were readmitted, compared with 8.7% with no change.

30-day readmission is rare. After cleaning, only 9.04% of 68,167 patients came back within 30 days, so the data is imbalanced. This matters when building a model in Project 4.

Cleaning changed the result. Removing repeat visits and patients who died or went to hospice lowered the readmission rate from 11.16% to 9.04%. Without cleaning, readmission would look more common than it is.

Summary: Patients with several recent hospital stays, older patients, and patients with longer stays are linked to higher 30-day readmission. These groups may benefit from extra follow-up after discharge. These charts show links, not causes.

## Limitations
- The data is from 1999–2008, so hospital care and patterns may be different today.
- Patients readmitted to a different hospital may not be counted, so the true readmission rate could be higher than 9.04%.
- I kept only each patient's first visit, so the results describe first visits, not all visits.
- Half of medical_specialty is unknown, so conclusions about doctor type are limited.
- The charts show links, not causes. For example, a longer stay may simply mean the patient was sicker.

## Files
- hospital-readmission-eda.ipynb: notebook
- diabetes_clean.csv: cleaned data (68,167 rows, 48 columns)

## Tools
Python, pandas, matplotlib, seaborn, Kaggle

## Note
I used an AI assistant as a tutor while learning pandas. It explained concepts and helped with code syntax; the cleaning decisions and interpretations are my own.
