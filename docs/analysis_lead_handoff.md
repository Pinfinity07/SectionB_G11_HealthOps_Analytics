# Analysis Lead Handoff Notes

This file is for the Analysis Lead to understand exactly what was completed, why it matters, and how to explain the work during presentation or viva.

## Role Responsibility

From the Capstone 2 PDFs, the Analysis Lead owns:

- `notebooks/03_eda.ipynb`: exploratory data analysis, KPI exploration, trends, distributions, outliers, and insight candidates.
- `notebooks/04_statistical_analysis.ipynb`: statistical validation using correlation, hypothesis testing, and regression if available.
- Support for `notebooks/05_final_load_prep.ipynb`: KPI computation and final analysis-ready data handoff for Tableau.

The grading rubric gives 25 marks to analysis depth, so this part must show more than charts. It needs business questions, statistical reasoning, decision language, and evidence-backed recommendations.

## What Was Done

### 1. Read the capstone PDFs

Files reviewed:

- `DVA_Capstone_2_Introduction.pdf`
- `DVA_Capstone_2_StudentHandbook.pdf`

Important requirements identified:

- Python must be used for ETL, EDA, KPI computation, and statistical analysis.
- Tableau must be used for dashboards.
- Final work must be committed to GitHub through visible contributions.
- Analysis should support business recommendations, not just describe charts.
- High-scoring work needs strong statistical analysis, decision-ready insights, and clear storytelling.

### 2. Created `notebooks/03_eda.ipynb`

Purpose:

- Explore the cleaned hospital dataset.
- Build KPI tables for hospital, diagnosis, region, age group, economic status, and insurance.
- Identify cost, outcome, and operational patterns.
- Create reusable CSV outputs for Tableau and report writing.

Why this was needed:

- The repository already had extraction and cleaning notebooks, but the required EDA notebook was missing.
- EDA is the bridge between cleaned data and business insight.
- Tableau and the final report need reliable KPI tables, not manually calculated numbers.

Main outputs generated:

- `data/processed/analysis/hospital_kpis.csv`
- `data/processed/analysis/diagnosis_kpis.csv`
- `data/processed/analysis/region_kpis.csv`
- `data/processed/analysis/age_group_kpis.csv`
- `data/processed/analysis/economic_status_kpis.csv`
- `data/processed/analysis/insurance_kpis.csv`
- `data/processed/analysis/outcome_mix_by_hospital.csv`
- `data/processed/analysis/outcome_mix_by_diagnosis.csv`
- `data/processed/analysis/spearman_correlation_matrix.csv`
- `data/processed/analysis/insight_candidates.csv`

### 3. Created `notebooks/04_statistical_analysis.ipynb`

Purpose:

- Validate whether the EDA patterns are statistically meaningful.
- Test treatment cost differences across diagnosis, hospital, hospital type, age group, economic status, and insurance.
- Test association between patient outcome and diagnosis, age group, economic status, insurance, hospital, and region.
- Test operational correlations with treatment cost.
- Include optional regression support if `statsmodels` is installed.

Why this was needed:

- The rubric specifically rewards statistical analysis depth.
- Statistical testing prevents weak claims like "this looks higher" and replaces them with evidence like "this difference is significant at p < 0.001".
- It gives the Strategy Lead and PPT Lead stronger evidence for recommendations.

Main outputs generated:

- `data/processed/analysis/statistical_tests_summary.csv`
- `data/processed/analysis/statistical_decision_readout.csv`
- `data/processed/analysis/pairwise_diagnosis_cost_tests.csv`

### 4. Created `notebooks/05_final_load_prep.ipynb`

Purpose:

- Prepare a single Tableau-ready fact table from the cleaned hospital datasets.
- Add useful analysis flags and calculated fields.
- Create summary tables for Tableau dashboard pages.

Why this was needed:

- Tableau work becomes cleaner if the final dataset already has outcome flags, insurance flags, hospital type flags, cost difference fields, and an operational access index.
- It reduces manual work and avoids hard-coded dashboard numbers.

Main outputs generated:

- `data/processed/hospital_tableau_ready.csv`
- `data/processed/analysis/tableau_hospital_summary.csv`
- `data/processed/analysis/tableau_diagnosis_summary.csv`
- `data/processed/analysis/tableau_region_summary.csv`
- `data/processed/analysis/tableau_age_group_summary.csv`
- `data/processed/analysis/tableau_economic_status_summary.csv`
- `data/processed/analysis/tableau_insurance_summary.csv`

## Dataset Used

The analysis uses the cleaned datasets:

- `data/processed/cleaned_hospital_1.csv`
- `data/processed/cleaned_hospital_2.csv`
- `data/processed/cleaned_hospital_3.csv`

Combined dataset size:

- 24,000 rows
- 17 cleaned source columns
- 25 columns after final Tableau load preparation
- 0 missing values in the cleaned data
- 3 hospitals: Medilife, Sunrise, Carepoint
- 5 diagnoses: Asthma, Covid, Diabetes, Flu, Hypertension

## Key Findings

### 1. Diagnosis is the strongest treatment cost driver

Evidence:

- Treatment cost differs significantly by diagnosis.
- Kruskal-Wallis statistic: 18356.4312
- p-value: <0.001
- Effect size: 0.7287

Business meaning:

- Diagnosis explains a large part of treatment cost variation.
- Covid has the highest average treatment cost.
- Flu has the lowest average treatment cost.
- The dashboard and report should treat diagnosis as a primary cost driver.

### 2. Covid is the highest-cost and highest-risk diagnosis group

Evidence:

- Covid average treatment cost: 40841.61
- Covid median treatment cost: 39270.97
- Covid recovery rate: 79.49%
- Covid mortality rate: 11.94%

Business meaning:

- Covid should be monitored as a high-cost, high-risk treatment category.
- Recommendations can focus on capacity planning, early triage, protocol standardisation, and resource allocation for Covid cases.

### 3. Hospital-level treatment cost differs significantly

Evidence:

- Treatment cost differs significantly by hospital.
- Kruskal-Wallis statistic: 1048.2809
- p-value: <0.001
- Effect size: 0.0532

Hospital cost pattern:

- Sunrise average cost: 28503.81
- Carepoint average cost: 24118.61
- Medilife average cost: 21926.01

Business meaning:

- Sunrise is the most expensive hospital in this dataset.
- The team should investigate whether this is due to hospital type, cost structure, case mix, or operational differences.

### 4. Outcome is associated with diagnosis and age group

Evidence:

- Outcome vs diagnosis: chi-square p-value <0.001, Cramer's V 0.1228
- Outcome vs age group: chi-square p-value <0.001, Cramer's V 0.1223

Business meaning:

- Patient outcomes are not random across diagnosis and age groups.
- Senior patients have higher mortality than younger groups.
- Covid has lower recovery and higher mortality compared with other diagnoses.

### 5. Insurance does not materially change cost or outcome in this data

Evidence:

- Treatment cost by insurance: p-value 0.2784, not significant.
- Outcome by insurance: p-value 0.5500, not significant.

Business meaning:

- Insurance should not be presented as a major driver unless future data shows otherwise.
- It can stay as a dashboard filter, but it should not be a main recommendation driver.

### 6. Operational indicators are statistically related to treatment cost, but effect sizes are small

Evidence:

- Doctor availability vs treatment cost: Spearman rho 0.1696, p <0.001
- Cleanliness score vs treatment cost: Spearman rho 0.1584, p <0.001
- Age vs treatment cost: Spearman rho 0.1588, p <0.001

Business meaning:

- These variables are statistically significant because the dataset is large, but the effect size is modest.
- Do not overclaim that operational scores cause higher cost.
- Use them as monitoring metrics, not as the main causal explanation.

## How To Explain This In Viva

Use this structure:

1. "My role was Analysis Lead, so I owned EDA and statistical analysis."
2. "I used the cleaned hospital datasets from the ETL stage and combined them into a 24,000-row analysis dataset."
3. "I first created KPI tables by hospital, diagnosis, region, age group, economic status, and insurance."
4. "Then I validated the patterns statistically using Kruskal-Wallis tests, chi-square tests, pairwise Mann-Whitney tests, and Spearman correlations."
5. "The strongest finding is that diagnosis is the main treatment cost driver, with Covid being the highest-cost and highest-risk diagnosis."
6. "Insurance was tested but was not statistically significant for cost or outcome, so we avoided making weak recommendations around it."
7. "The outputs were saved as CSVs so Tableau and the final report use reproducible numbers instead of manual calculations."

## Recommended Dashboard Focus

The Tableau dashboard should prioritize:

- Hospital comparison: volume, average cost, recovery rate, mortality rate, under-treatment rate.
- Diagnosis cost and risk: Covid, Diabetes, Hypertension, Asthma, Flu.
- Outcome segmentation: by diagnosis, age group, economic status, and region.
- Operational overview: doctor availability, cleanliness score, doctor experience, operational access index.
- Filters: hospital, diagnosis, region, age group, economic status, insurance.

## Recommended Business Recommendations

These should be refined by the Strategy Lead, but the analysis supports these directions:

1. Prioritize Covid treatment pathways because Covid has the highest cost and mortality rate.
2. Review Sunrise cost structure because it has the highest average treatment cost among hospitals.
3. Monitor senior patients as a higher-risk group because mortality is materially higher for seniors.
4. Reduce Flu under-treatment backlog because Flu has the highest under-treatment rate.
5. Use diagnosis and age group as primary dashboard filters because they are statistically associated with cost and outcome.

## Important Limitations

Mention these honestly:

- The dataset does not prove causation. It shows statistical relationships and patterns.
- Hospital outcome rates are identical in the cleaned data, so hospital is not useful for outcome comparison in this dataset.
- `statsmodels` was not installed locally, so optional regression was added but skipped in the local smoke test. It can run in Colab after installing `statsmodels`.
- Visual charts in the notebooks require `matplotlib` and `seaborn`. The local environment did not have them, but the table-based analysis and statistical tests ran successfully.

## Files You Should Know

Primary files for your role:

- `notebooks/03_eda.ipynb`
- `notebooks/04_statistical_analysis.ipynb`
- `notebooks/05_final_load_prep.ipynb`
- `data/processed/hospital_tableau_ready.csv`
- `data/processed/analysis/statistical_tests_summary.csv`
- `data/processed/analysis/insight_candidates.csv`
- `data/processed/analysis/statistical_decision_readout.csv`

If someone asks what you contributed, say:

> I created the EDA notebook, statistical analysis notebook, final Tableau load preparation notebook, KPI outputs, statistical test summaries, and decision-ready analysis handoff files for the hospital operations capstone.
