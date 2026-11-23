# Week 3 Pipeline Decisions: UTM FAI Student FAQ Assistant

## Topic 2 requirements this pipeline supports

| Requirement | Why the data pipeline matters |
|---|---|
| FR1, FR4, NFR2 | Answers must come from official regulation text, so the text has to be extracted and split into usable passages (S01). |
| FR1, FR2 | Programme facts (structure, admission, fees) come from the FAI programme page (S02). |
| FR3, RAI1 | Every passage in data/processed/ carries the source file name and SHA-256 so it can be traced back to the official document. The pipeline_v1 output files keep only source and id; their SHA-256 is recorded in each pipeline_manifest.json. |
| RAI3 | Manifests record the source version as `Not verified`, so version claims are not made without evidence. |
| NFR4 | The pipeline is a notebook that can be re-run when a source changes, and Cell 9 checks that a re-run gives the same results. |

## Selected sources

- S01: UTM Academic Regulations for Full-Time Undergraduate Studies Programmes (2026), `FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf` (https://ugs.utm.my/wp-content/uploads/sites/8/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf).
- S02: FAI programme page: Bachelor in Artificial Intelligence with Honours, `Bachelor-in-Artificial-Intelligence-With-Honours-FAI-programme-page.pdf` (https://fai.utm.my/?p=2248).

Pathway A (PDF/TXT) only. See `docs/week3_data_inventory.md`.

## Part B comparison and evidence (same cleaned text per source, 500-character target)

### S01: Academic Regulations

| Measure | Approach A: fixed-size | Approach B: paragraph-aware |
|---|---|---|
| Chunks produced | 210 | 213 |
| Mean / min / max length (characters) | 498.5 / 182 / 500 | 491.2 / 74 / 500 |
| Chunks ending at a sentence or clause end | 4.8% | 5.6% |
| Chunks starting mid-sentence (lowercase start) | 84.8% | 82.2% |
| Chunks starting at an `Article N` or `SECTION N` heading | 0 | 2 |

Blank-line paragraph segments found: 14. Of these, 5 are longer than 500 characters, so the paragraph-aware splitter falls back to fixed splitting inside them.

### S02: Programme page

| Measure | Approach A: fixed-size | Approach B: paragraph-aware |
|---|---|---|
| Chunks produced | 45 | 57 |
| Mean / min / max length (characters) | 493.9 / 227 / 500 | 386.6 / 8 / 500 |
| Chunks ending at a sentence or clause end | 4.4% | 14.0% |
| Chunks starting mid-sentence (lowercase start) | 62.2% | 35.1% |
| Chunks starting at an `Article N` or `SECTION N` heading | 0 | 0 |

Blank-line paragraph segments found: 64. Of these, 11 are longer than 500 characters, so the paragraph-aware splitter falls back to fixed splitting inside them.

| Source | Approach / configuration A | Approach / configuration B | Observed evidence | Selected approach and trade-off |
|---|---|---|---|---|
| S01 | Fixed-size, 500 characters | Paragraph-aware, 500 characters | See the S01 table and Step B3 of the notebook. | **paragraph-aware** (`paragraph-aware-char-500`). Paragraph-aware gives only a small gain on S01 because the extracted text has very few blank lines, so most of the text is still split by the fixed 500-character rule. |
| S02 | Fixed-size, 500 characters | Paragraph-aware, 500 characters | See the S02 table and Step B3 of the notebook. | **paragraph-aware** (`paragraph-aware-char-500`). Paragraph-aware keeps more passages starting and ending at natural boundaries on S02, at the cost of a few very short chunks. |

### Why this choice
- Both sources use the same method, so one configuration is documented and the pipeline is simple to rerun (NFR4).
- On S02 the evidence clearly favours paragraph-aware: chunks starting mid-sentence drop from 62.2% to 35.1% and chunks ending at a sentence or clause end rise from 4.4% to 14.0%.
- On S01 the two methods are almost the same (82.2% against 84.8% mid-sentence starts, and 2 chunks that start at a heading against 0). Paragraph-aware is never worse on the measured values, so it is kept, but the evidence does not show a large benefit for S01.
- Fixed-size 500-character splitting cuts through the middle of articles and table rows. For FR1 and FR4 (answers that combine an article with its appendix) this is a real risk, so a splitter that respects paragraph breaks where they exist is preferred.

## Chosen preparation configuration

- Text extraction with `pypdf` (all pages, joined with newlines).
- Cleaning: collapse repeated spaces and tabs, then strip the ends. Nothing else is removed or rewritten.
- Chunking: **paragraph-aware**, 500 characters, for both sources.
- Each record keeps `record_id`, `text`, `source`, `source_sha256`, `processing` and `page` (= `None`).

## Accepted limitations

- S01: only 14 paragraph segments exist in 105,132 characters, and 5 of them are longer than 500 characters, so the paragraph-aware method behaves almost like fixed-size splitting. About 82% of S01 chunks still start in the middle of a sentence. Better splitting (by Article heading) is left for a later topic.
- S01: 7 of 81 pages (1, 2, 3, 11, 13, 15, 81) returned no text. They are probably cover, divider or image pages, but this has not been checked against the PDF. The edition number cannot be read from the extracted text, so the version is recorded as `Not verified`.
- S01: the running header `Academic Regulations`, the `Section N` lines and page numbers stay inside chunks, because the lab's V1 cleaning only collapses spaces and tabs.
- S01: tables are flattened into lines of text without a grid, so a table row can be separated from its heading.
- S02: the web page was saved as a PDF, so the site header (`Copyright © 2026 Faculty of Artificial Intelligence ...`) and menu text were extracted with the content. The copyright line appears on 7 of the 14 pages and is inside 7 of the 57 chunks.
- S02: pages 7 and 8 are an admission-requirements table written in Malay for the 2024/2025 academic session. It is older than the 2026 regulations and is in a different language from the rest of the page. Answers that use it need a date caveat.
- S02: paragraph-aware splitting produces 4 very short chunks (under 50 characters), such as ` Contact`. They add noise but are kept in V1.
- Neither source keeps page numbers in the prepared records (`page` = `None`), so page-level citation is not possible yet.

## ETL or ELT, and batch or streaming

- **ETL.** The text is extracted and cleaned before it is stored. The raw PDFs stay unchanged in `data/raw/`, and only the prepared JSONL is written to the output folders.
- **Batch.** The regulations and the programme page change by edition or by semester, not by the minute, so the pipeline is run on demand (for example when a new edition is published). Streaming adds complexity that is not needed.

## Output location

- Part B prepared outputs: `data/processed/prepared_text_S01.jsonl` (213 records) and `data/processed/prepared_text_S02.jsonl` (57 records)
- Part B lineage manifest: `data/week3_manifest.json` (2 records)
- Part C pipeline outputs and run manifests: `data/pipeline_v1/documents_S01_regulations/` and `data/pipeline_v1/documents_S02_programme/` (each has `prepared_text.jsonl` and `pipeline_manifest.json`)
- Notebook: `notebooks/week3_pipeline.ipynb`

## Provenance and access caveat

The manifests record `source_version: Not verified` and `access_rights: Not verified`. The S01 copyright page does not allow reproduction without written permission, so the raw PDFs are not committed. The prepared chunks contain text from the PDFs. The lecturer replied by message (21 Nov 2026) that committing them is okay because they are used for a course project assignment and not for reselling or other purposes, so the prepared `.jsonl` files are committed. This is the lecturer's informal reply, not written permission from UTM. A checksum only shows that a file has not changed. It does not prove permission to share or data quality.

## Stage contracts

| Stage | Contract / evidence | Failure response |
|---|---|---|
| Ingest | The chosen files exist under `data/raw/` and are permitted (Cell 3, `select_source` / `resolve_source`). | Stop for missing input (`FileNotFoundError`). |
| Inspect | Each PDF is readable and has extractable text (Step A3, `inspect_documents`). | Stop and flag: "No extractable text found. Scanned PDFs may require OCR." |
| Prepare | The chosen method produces non-empty chunks (Cell 5A2, `prepare_documents`). | Stop and report: "No text chunks produced". |
| Validate | Each output exists and is non-empty, every chunk has text, the exported count matches, and SHA-256 fingerprints are stored. | Stop for a missing or empty output. |
| Record lineage | Each source links to its exact prepared output with checksums (Cell 6A / 6B). | Stop and report a missing mapping or a checksum mismatch. |

Run evidence: S01 prepared SHA-256 `189fea50182ee87d...` (165,241 bytes), pipeline output `0b76758a29266df6...`. S02 prepared SHA-256 `f8d99493790d4aa2...` (41,794 bytes), pipeline output `88b8b9095440097a...`. Cell 6B and Cell 9 passed for both sources.

## Traceability update (adds Week 3 evidence to the Week 2 matrix)

| Objective | Requirement | Architecture element | Week 3 evidence |
|---|---|---|---|
| O2 | FR1, FR4 / NFR2 | Official UTM Documents → Knowledge Access | `data/pipeline_v1/documents_S01_regulations/prepared_text.jsonl` (213 chunks) |
| O2 | FR1, FR2 | Official UTM Documents → Knowledge Access | `data/pipeline_v1/documents_S02_programme/prepared_text.jsonl` (57 chunks) |
| O2 | FR3 / RAI1 | Source reference | `source` and `source_sha256` in every record; `data/week3_manifest.json` |
| O3 | RAI3 / NFR4 | Source version, repeatable update | Manifest `source_version: Not verified`; Cell 9 repeatability check |
