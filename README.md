# foodb_v1.0_cleanup

## Description

`foodb_v1.0_cleanup` is designed to clean the `FooDB v1.0` database up.
`FooDB` is the world's largest and most comprehensive resource on food constituents, chemistry, and biology. It contains information on both macronutrients and micronutrients, including many of the constituents that give foods their flavor, color, taste, texture, and aroma. It covers ca. 70,000 compounds found in ca. 800 food stuffs, linked through a content table that reports compound concentrations in individual foods.
More information is available at https://foodb.ca/.

## What does the script do

Main cleanup steps:

1. Load the Content, Food, and Compound CSV files (with encoding detection and fallback encodings if needed).

2. Clean the Food table:

- Rename columns to follow other similar databases, e.g., public_id to public_food_id.
- Drop unnecessary columns (e.g., picture data, timestamps, creator/updater IDs, export flags, etc.).
- Create and populate a harmonized food group column (food_group | food_subgroup | food_type), similar to the CPDat product use category.

3. Clean the Compound table:

- Drop unnecessary columns (e.g., state, annotation_quality, kingdom).
- Detect broken compound names (mojibake) and replace them with existing IUPAC names, PubChem IUPAC names, or PubChem names.
- Remove compounds with no InChIKey values.
- Rename and reorder PubChem columns.

4. Remove compounds:

- With specific InChIKey values (e.g., mixtures, inorganic compounds & inorganic mixtures, isotopically labeled compounds, ions, etc.).
- With unwanted keywords (e.g., mixtures, minerals, metals, inorganic compounds & inorganic mixtures, extracts, etc.), except for protected compound names.

5. Clean the Content table:

- Rename columns to follow other similar databases, e.g., source_id to cpd_id and orig_source_name to orig_feature_name.
- Drop unnecessary columns (e.g., citations, timestamps, creator/updater IDs, methods, etc.).
- Remove nutrients (source_type == 'Nutrient') and summary entries (e.g., 'Carbohydrates, total', 'Fat, total (Lipids)', etc.).

6. Populate missing values in the Content table:

- Fill orig_food_common_name within each food_id, then from the Food table.
- Create food_name from the Food table using food_id.
- Fill orig_feature_name from the Compound table using cpd_id.

7. Remove Content table entries:

- With no food_name or orig_feature_name values.
- With cpd_id not present in the filtered Compound table.
- With empty or zero orig_content values.

8. Show stats (data types, unique values, and missing values per column) for the filtered Food, Compound, and Content tables.

9. Save filtered Food, Compound, and Content tables as CSV files.

## Prerequisites

1. The script is written in Python 3; https://www.python.org/downloads/windows/.
2. The script is run in JupyterLab Notebook; https://jupyter.org/.
3. FooDB CSV files are available at https://foodb.ca/downloads:

- The Compound table was updated with PubChem names and other identifiers using PubChem_Retriever (available at https://github.com/r3bryk/PubChem_Retriever).

## How to use the script

Run the script cell by cell in JupyterLab Notebook.

## License

[![MIT License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/license/mit)

Intended for academic and research use.
