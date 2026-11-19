
# Week 3 Data Inventory

## Project: UTM FAI Student FAQ Assistant

| Source ID | Filename / URL | Type and Purpose | Version / Date | Access / Rights | Inspection Evidence |
|---|---|---|---|---|---|
| S01 | FAI_Programme_Information.pdf / https://fai.utm.my/bachelor-in-artificial-intelligence-with-honours/ | PDF – academic rules | 2026 / 16th edition | Public source; redistribution to be checked | PDF successfully opened; page count recorded; text extraction checked; programme information inspected |
| S02 | UTM_Academic_Regulations_2026.pdf / https://ugs.utm.my/wp-content/uploads/sites/8/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf  | PDF – programme details | Record actual date | Public source; rights to be checked | PDF uploaded and opened successfully in Google Colab. The document contains 81 pages. Text extraction was tested using PyPDF. Most pages produced readable text, while pages 1–3 and 15 returned no extractable text. Page 4 showed possible font encoding issues, and several pages returned very little text. Further inspection is required to confirm whether these pages contain blank content, images, or extraction errors. |

## Data Preparation Decision

Selected pathway: A – PDF/TXT

Reason:

The FAQ assistant uses official documents as its knowledge sources. Extracting and preparing text allows the documents to be processed for later retrieval and source referencing.

Actual project datasets will be used.

No synthetic practice dataset is being treatedas real project evidence.
