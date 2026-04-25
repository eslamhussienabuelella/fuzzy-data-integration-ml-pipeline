# Fuzzy Data Integration ML Pipeline

A reproducible data-construction pipeline for integrating Canadian vehicle fuel-consumption records with vehicle specification data using token-based fuzzy matching.

This repository supports the data preparation stage of the project:

> **Explainable and Constraint-Aware Machine Learning for Robust Vehicle Fuel Consumption Prediction**

## Project overview

The pipeline integrates two independent data sources:

1. **Fuel consumption ratings** containing vehicle efficiency metrics and basic descriptors.
2. **Vehicle specification files** containing detailed design, geometry, and mass measurements.

Because vehicle names and descriptions are not formatted consistently across the two sources, the workflow applies text normalisation, tokenisation, fuzzy matching, quality control, and final dataset standardisation.

## Repository structure

```text
fuzzy-data-integration-ml-pipeline/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   └── Data_Construction_Logic.ipynb
│
├── data/
│   ├── processed/
│   │   ├── fc_veh_spec_all.csv
│   │   └── missing_vehicles.csv
│   └── raw/
│       └── README.md
│
└── docs/
    └── Data_constuction_logic.docx
```

## Workflow summary

### 1. Load raw datasets

The notebook loads two fuel-consumption rating datasets, filters them to model years **2011–2023**, and dynamically reads yearly vehicle specification CSV files for the same period.

### 2. Standardise text fields

Vehicle names are cleaned to reduce formatting differences between sources:

- lower-case conversion;
- punctuation and special-character handling;
- category harmonisation, such as `4 door` → `4dr` and `sport utility vehicle` → `suv`;
- removal of weak marketing tokens, such as `technology`, `tech`, and `package`.

### 3. Build token keys

Each fuel-consumption record is represented using tokens derived from:

- model name;
- engine size;
- vehicle class;
- cylinder configuration.

Vehicle specification records are similarly tokenised after cleaning their model descriptions.

### 4. Apply Jaccard similarity

For each fuel-consumption vehicle, candidate specification records are restricted to the same **make** and **model year**. Token overlap is then scored using Jaccard similarity:

```text
Jaccard similarity = |A ∩ B| / |A ∪ B|
```

where `A` is the fuel-consumption token set and `B` is the vehicle-specification token set.

### 5. Select the best match

The best candidate is selected using the following ordered rules:

1. highest Jaccard similarity;
2. strongest shared tokens;
3. largest token intersection;
4. smallest token union.

### 6. Quality control

The pipeline validates the joined dataset through:

- vehicle make consistency checks;
- non-empty token-intersection checks;
- tie-case review;
- removal of mismatched manufacturer records.

### 7. Export final dataset

The final integrated dataset is exported as:

```text
data/processed/fc_veh_spec_all.csv
```

A separate file of unmatched records is retained as:

```text
data/processed/missing_vehicles.csv
```

## Final dataset

The processed dataset contains integrated fuel-consumption and vehicle-specification attributes.

Current processed output from the attached data:

- **Rows:** 12,895
- **Columns:** 33
- **Model years:** 2011–2023
- **Unmatched vehicle records:** 706

Main feature groups include:

- fuel-consumption variables: city, highway, combined fuel consumption, CO₂ emissions;
- mechanical variables: engine size, cylinders, transmission, fuel type;
- geometry variables: overall length, width, height, wheelbase, overhangs, track widths;
- mass variables: curb weight and weight distribution.

## Key files

| File | Description |
|---|---|
| `notebooks/Data_Construction_Logic.ipynb` | Main notebook implementing the full data construction workflow |
| `docs/Data_constuction_logic.docx` | Written explanation of the data construction procedure |
| `data/processed/fc_veh_spec_all.csv` | Final integrated dataset |
| `data/processed/missing_vehicles.csv` | Fuel-consumption records without reliable specification matches |

## Requirements

Install the required Python packages with:

```bash
pip install -r requirements.txt
```

Core packages:

- pandas
- numpy
- matplotlib
- seaborn
- jupyter

## How to run

Clone the repository:

```bash
git clone https://github.com/eslamhussienabuelella/fuzzy-data-integration-ml-pipeline.git
cd fuzzy-data-integration-ml-pipeline
```

Create and activate an environment:

```bash
python -m venv .venv
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Open the notebook:

```bash
jupyter notebook notebooks/Data_Construction_Logic.ipynb
```

## Recommended GitHub topics

```text
data-integration
fuzzy-matching
jaccard-similarity
data-cleaning
pandas
vehicle-data
fuel-consumption
machine-learning-pipeline
data-quality
python
```

## Suggested repository description

> Token-based fuzzy data integration pipeline linking Canadian fuel-consumption records with vehicle specification data using text normalisation, Jaccard similarity, and quality-control checks.

## Author

**Eslam H. M. Abuelella**  
MSc Data Science, Coventry University  
Geospatial Data Scientist / GIS & ML practitioner
