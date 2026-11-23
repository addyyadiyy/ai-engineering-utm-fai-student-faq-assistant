# Week 3 Data Inventory: UTM FAI Student FAQ Assistant

## Step A1: From Topic 2 requirements to data needs

| Requirement ID | Needed information / feature | Candidate source | Why usable / constraint |
|---|---|---|---|
| FR1, FR4 (NFR2) | Official regulation text (rules, articles, tables, appendices) as passages that answers can be grounded in, including answers that combine an article with an appendix. | S01: UTM Academic Regulations for Full-Time Undergraduate Studies Programmes (2026). | Authoritative, 2026, public PDF. Long, with many cross-references. The copyright page restricts reproduction (see Access / rights). |
| FR1, FR2 | Programme facts a student asks about (overview, objectives, structure, admission, fees). | S02: FAI programme page: Bachelor in Artificial Intelligence with Honours, saved as a PDF from the public page. | Official FAI page. Saved from a web page, so menu and copyright text is mixed into the content. Admission table is dated 2024/2025. |
| FR3 / RAI1 | Source identity kept with every passage so that answers can cite the document. | Both PDFs plus manifest metadata (file name, SHA-256). | The V1 splitter does not keep page numbers (`page` = `None`), so page-level citation is a known gap for later topics. |
| RAI3, NFR4 | A source version or date, and a repeatable way to refresh the data. | Edition or date text in each PDF, SHA-256, and a re-runnable notebook. | Neither version could be confirmed from the extracted text, so manifests record `source_version: Not verified`. |

Source type: unstructured text (PDF). Dataset used: **real project sources, not the synthetic practice dataset.** Pathway A (PDF/TXT) only. Pathways B to F are not used because the project has no tabular, image, JSON, audio or time-series data. The FAI Undergraduate Registration Kit page is not used: it only links to other documents.

## Step A4: Data inventory

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| S01 | `data/raw/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf`; https://ugs.utm.my/wp-content/uploads/sites/8/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf | PDF, unstructured text. Source for FR1, FR3, FR4, RAI1, RAI3. | Published by Penerbit UTM Press for Universiti Teknologi Malaysia, 2026. The file name says "16th edition". The cover and title page (PDF pp. 1 and 4) say “16th Edition”, but the copyright page (p. 5) says “First Edition 2026”. The two disagree and the edition is not confirmed. The title and “16th Edition” are graphics on those pages, so they are not in the extracted text (only www.penerbit.utm.my and 2026 are). | Public URL. The copyright page forbids reproduction without written permission from UTM. Raw PDF kept out of GitHub. Redistribution of prepared chunks: not verified. | 81 pages, 779,259 bytes, 105,132 characters (sum of per-page text; the manifest’s 105,212 counts the same text plus one newline between pages). Pages with almost no text: 1, 2, 3, 11, 13, 15, 81. Checked against the PDF: pp. 1 and 81 are the front and back covers (images), p. 2 is a graphic title page, and pp. 3, 11, 13 and 15 are blank. SHA-256 `d2743dfad87d3be3...` |
| S02 | `data/raw/Bachelor-in-Artificial-Intelligence-With-Honours-FAI-programme-page.pdf`; https://fai.utm.my/?p=2248 | PDF made by saving the public programme page. Source for FR1, FR2, FR3, RAI1. | Owner: Faculty of Artificial Intelligence, UTM. The page shows "Copyright 2026". No page version number. The admission table on pages 7 and 8 is for the 2024/2025 session. | Public web page. Saved as a PDF by the student. Redistribution of prepared chunks: not verified. | 14 pages, 1,372,769 bytes, 22,263 characters. Pages with almost no text: none. SHA-256 `b8527fea695e284e...` |
| S01, subset 1 | Same file, PDF page 5 | Body text page, used to check normal paragraph extraction. | Same as S01. | Same as S01. | Printed in Step A3 for S01. |
| S01, subset 2 | Same file, PDF page 26 | Page with a table caption, used to check table extraction. | Same as S01. | Same as S01. | Extracted as: Table 1  Academic Session * / * 1 week of  mid-semester break for Semester I and II is allocated subject to the academic  / session calendar set by the University . / **Revision period is not allocated for students who will undergo industrial training during  |
| S02, subset 1 | Same file, PDF page 1 | First page: menu, overview text. | Same as S02. | Same as S02. | Printed in Step A3 for S02. |
| S02, subset 2 | Same file, PDF page 7 | Page with the most lines: the admission-requirements table (Malay, 2024/2025). | Same as S02. | Same as S02. | Extracted as: UNIVERSITI TEKNOLOGI MALAYSIA  / SYARAT KHAS KEMASUKAN MENGIKUT PROGRAM  / BAGI LEPASAN DIPLOMA UA, POLITEKNIK, DVM DAN KELAYAKAN SETARAF  / SESI AKADEMIK 2024/2025  |

## Observed ingestion limitations (from Step A3 evidence)

**S01 (regulations)**
- Extraction worked on every page that has text. Characters per page: min 0, median 1553, max 2402. 7 pages returned no text (1, 2, 3, 11, 13, 15, 81). These are covers, a graphic title page and blank pages, so no rule text is missing.
- Repeated lines (digits shown as `#`): `#` (63 pages), `Academic Regulations` (32 pages), `Section #` (22 pages). They stay inside chunks, because the lab's V1 cleaning only collapses spaces and tabs.
- Tables are flattened into plain lines with no grid. 11 table captions were found.
- Only 10 blank-line paragraph breaks exist in the whole document, and 44 lines start with `Article N`. Hyphenated line breaks: 11.

**S02 (programme page)**
- Extraction worked on all 14 pages. Characters per page: min 828, median 1443, max 3107.
- The page menu and the line `Copyright © 2026 Faculty of Artificial Intelligence | All Right Reserved.` were captured on 7 of the 14 pages, because the web page was saved as a PDF. Short repeated lines: `#,#)` (37 pages), `RM#,# (USD` (27 pages), `+#-#` (15 pages), `Undergraduate` (6 pages).
- The admission table (pages 7 and 8) is in Malay and for the 2024/2025 session. This is an older date and a different language from the rest of the sources.
- 53 blank-line paragraph breaks. No `Article N` headings and no table captions.

Neither source keeps page numbers in the prepared records (`page` = `None`).

## Checkpoint A

- **Which Topic 2 requirement needs this data?** FR1 and FR4 (grounded answers from the regulations, S01), FR1 and FR2 (programme facts, S02), supported by FR3 / RAI1 (source identity) and RAI3 / NFR4 (version and repeatable refresh).
- **Loaded sample (not just "file loaded"):** Step A3 prints a body page and a second page from each source, plus layout checks. Step B3 prints two prepared records per source.
