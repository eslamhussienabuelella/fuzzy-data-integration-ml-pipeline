# Fuzzy Data Integration ML Pipeline

A reproducible data-engineering pipeline for integrating Canadian vehicle fuel-consumption records with detailed vehicle specification datasets using token-based fuzzy matching and Jaccard similarity.

This project demonstrates practical **data engineering**, **data quality assurance**, and **fuzzy entity resolution** techniques to construct a machine-learning-ready dataset from heterogeneous public data sources.

This repository supports the data preparation stage of the MSc thesis:

> **Explainable and Constraint-Aware Machine Learning for Robust Vehicle Fuel Consumption Prediction**

---

## Project overview

Machine learning performance depends heavily on data quality and feature completeness. This project integrates two independent public datasets into a unified analytical dataset for downstream machine learning.

The pipeline integrates:

1. **Fuel consumption ratings dataset**  
   Contains fuel-efficiency measurements and CO₂ emission records.

2. **Vehicle specification dataset**  
   Contains vehicle design, geometry, and mass-related technical specifications.

Because vehicle naming conventions differ across sources, direct joins are unreliable. This pipeline resolves those inconsistencies through text standardisation and token-based fuzzy matching.

---

## Why this project matters

Real-world datasets often contain:

- inconsistent naming conventions;
- incomplete identifiers;
- heterogeneous formatting;
- duplicated or weak descriptive tokens.

This project demonstrates practical solutions for:

- multi-source data integration;
- fuzzy entity resolution;
- text normalisation;
- quality-control validation;
- reproducible dataset construction;
- machine-learning data preparation.

These are core data-engineering tasks in production machine learning workflows.

---

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
│   ├── raw/
│   │   ├── README.md
│   │   ├── fuel_consumption_ratings/
│   │   │   ├── my1995-2014-fuel-consumption-ratings-5-cycle.csv
│   │   │   └── my2015-2024-fuel-consumption-ratings.csv
│   │   │
│   │   └── vehicle_specifications/
│   │       ├── 2011_en.csv
│   │       ├── 2012_en.csv
│   │       ├── 2013_en.csv
│   │       ├── 2014_en.csv
│   │       ├── 2015_en.csv
│   │       ├── 2016_en.csv
│   │       ├── 2017_en.csv
│   │       ├── 2018_en.csv
│   │       ├── 2019_en.csv
│   │       ├── 2020_en.csv
│   │       ├── 2021_en.csv
│   │       ├── 2022_en.csv
│   │       └── 2023_en.csv
│   │
│   └── processed/
│       ├── fc_veh_spec_all.csv
│       └── missing_vehicles.csv
│
└── docs/
    └── Data_constuction_logic.docx
```

---

## Workflow architecture

```text
Fuel consumption data + Vehicle specification data
                     ↓
             Text normalisation
                     ↓
               Tokenisation
                     ↓
          Jaccard similarity matching
                     ↓
            Tie-breaking logic
                     ↓
             Quality-control checks
                     ↓
          Final integrated dataset
```

---

## Workflow summary

## 1. Load raw datasets

The notebook loads:

- fuel-consumption rating datasets;
- yearly vehicle specification datasets.

The analysis is restricted to:

```text
Model years: 2011–2023
```

to ensure overlap between both sources.

This repository includes both raw input datasets and processed outputs for full reproducibility.

---

## 2. Standardise text fields

Vehicle names and descriptors are cleaned to reduce formatting inconsistencies.

Standardisation includes:

- lowercase conversion
- punctuation removal
- whitespace normalisation
- synonym harmonisation

Examples:

```text
4 door → 4dr
sport utility vehicle → suv
```

Weak marketing terms are removed:

```text
technology
tech
package
limited
premium
```

---

## 3. Build token keys

Each fuel-consumption record is transformed into token sets using:

- model name
- engine size
- vehicle class
- cylinder count

Vehicle specification records undergo the same tokenisation logic.

---

## 4. Apply Jaccard similarity

Matching is restricted to records with the same:

- manufacturer (make)
- model year

Candidate matches are scored using:

```text
Jaccard similarity = |A ∩ B| / |A ∪ B|
```

Where:

- `A` = fuel-consumption token set
- `B` = vehicle specification token set

Higher overlap indicates stronger similarity.

---

## 5. Best-match selection logic

Candidate matches are resolved using ordered tie-breaking:

1. highest Jaccard similarity  
2. largest token intersection  
3. smallest token union  
4. strongest semantic overlap  

This improves matching robustness.

---

## 6. Quality-control validation

Integrated records are validated using:

- make consistency checks
- empty-intersection checks
- duplicate match inspection
- tie-case review
- manual mismatch filtering

This reduces noisy joins and improves final data quality.

---

## 7. Export final dataset

Final outputs:

**Integrated dataset**

```text
data/processed/fc_veh_spec_all.csv
```

**Unmatched records**

```text
data/processed/missing_vehicles.csv
```

---

## Final dataset summary

Processed integrated dataset:

| Metric | Value |
|---|---:|
| Rows | 12,895 |
| Columns | 33 |
| Model years | 2011–2023 |
| Unmatched records | 706 |

---

## Main feature groups

### Fuel-consumption features

- city fuel consumption
- highway fuel consumption
- combined fuel consumption
- CO₂ emissions

### Mechanical features

- engine size
- cylinders
- transmission
- fuel type

### Geometry features

- overall length
- overall width
- overall height
- wheelbase
- overhangs
- track widths

### Mass features

- curb weight
- weight distribution

---

## Key files

| File | Description |
|---|---|
| `notebooks/Data_Construction_Logic.ipynb` | Full implementation of the integration workflow |
| `docs/Data_constuction_logic.docx` | Detailed workflow documentation |
| `data/processed/fc_veh_spec_all.csv` | Final integrated dataset |
| `data/processed/missing_vehicles.csv` | Unmatched vehicle records |

---

## Installation

Install dependencies:

```bash
pip install -r requirements.txt
```

Core packages:

- pandas
- numpy
- matplotlib
- seaborn
- jupyter

---

## Reproducibility

Clone the repository:

```bash
git clone https://github.com/eslamhussienabuelella/fuzzy-data-integration-ml-pipeline.git
cd fuzzy-data-integration-ml-pipeline
```

Create environment:

### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

### Linux/macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Launch notebook:

```bash
jupyter notebook notebooks/Data_Construction_Logic.ipynb
```

---

## Skills demonstrated

- Data integration
- Fuzzy matching
- Entity resolution
- Data cleaning
- Feature engineering
- Quality assurance (QA/QC)
- Reproducible pipelines
- Machine-learning data preparation

---

## Recommended GitHub topics

```text
data-integration
fuzzy-matching
jaccard-similarity
entity-resolution
data-cleaning
data-engineering
machine-learning
feature-engineering
vehicle-data
python
```

---

## Repository description

Token-based fuzzy data integration pipeline linking Canadian fuel-consumption and vehicle specification datasets using text normalisation, Jaccard similarity, and quality-control validation.

---

## Author

**Eslam H. M. Abuelella**  
MSc Data Science — Coventry University  
MSc Geology — Cairo University  

Machine Learning | Data Engineering | Geospatial Analytics | Earth Systems Modelling
