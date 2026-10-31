# Week 2 Requirements Specification: UTM FAI Student FAQ Assistant

Version: 1.0

## Section 0: Project Framing & Application Scope Checkpoint

| Element | Details |
|---|---|
| **Problem Evidence** | **(1)** The official UTM undergraduate academic regulations are one long document (10 sections, 42 articles, 11 appendices) whose rules often point to other articles, tables or appendices. For example, course registration in Article 4 refers to Appendix 3, and credit transfer in Articles 15 and 16 refers to Appendix 4. One student question can therefore need several places read together. Source: Universiti Teknologi Malaysia (2026), *Academic Regulations for Full-Time Undergraduate Studies Programmes*, https://ugs.utm.my/wp-content/uploads/sites/8/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf **(2)** The FAI Undergraduate Registration Kit gives new students about ten separate items as individual Google Drive links (academic rules, handbook, calendar, grading system, important dates and others). Students have to open and search them one by one. Source: https://fai.utm.my/undergraduate-registration-kit/ |
| **Existing Approaches** | **(1) Static FAQ pages and document hubs** (UTM SPS FAQ, FAI Registration Kit): official and authoritative, but static, split across files, and without search or question answering. The SPS FAQ is for postgraduate students and dated 2017. **(2) Academic advisors** (Article 2 of the regulations): advisors guide students on academic matters and verify course registration, so they give personal, contextual advice. **(3) General-purpose AI assistants** (for example ChatGPT or Claude): answer in plain language on almost any topic, but are not tied to UTM's official documents. **(4) Keyword search inside a PDF** (Ctrl+F): finds exact words quickly, but does not combine related articles or explain the result. |
| **Project / Engineering Gap** | No tool gives FAI undergraduates quick answers that are grounded in official UTM documents and cite the exact article or page. Students must search a long regulations document and several separate files themselves, or ask a person. The practical limitations that remain are weak grounding, no source traceability and no scope control in general-purpose assistants. |
| **Objective O1** | To design a document-grounded AI FAQ assistant based on official, public UTM documents. |
| **Objective O2** | To develop the assistant with retrieval and appropriate guardrails (scope check and citation requirement). |
| **Objective O3** | To evaluate the assistant in terms of answer accuracy, groundedness (correct citations), latency and safety. |
| **Intended Users** | UTM FAI undergraduate students (Bachelor in Artificial Intelligence with Honours), especially new and first-year students. |
| **Supported Tasks** | Answer questions about (a) academic regulations: course registration, credit load, course withdrawal, grading, GPA and CGPA, academic standing, study deferment, credit transfer, examination rules, grade appeals and graduation requirements; (b) programme information from the FAI programme page: duration, total credits, structure and admission criteria; (c) pointing the student to the relevant official document and article. |
| **Information / Knowledge Domain** | Official, public UTM and FAI documents for full-time undergraduate students: the Academic Regulations (2026), the FAI Bachelor in Artificial Intelligence with Honours programme page, and the documents linked from the FAI Undergraduate Registration Kit that the team chooses to include. |
| **Operating Context** | A web chat interface used by students on a laptop or phone, typically while planning course registration or checking a rule. Questions are in English. It supports routine information enquiries and does not replace the academic advisor. |
| **Important Constraints** | Public documents only (no student records or personal data). The regulations PDF is copyrighted, so the team must check what can be stored in the repository. Course project: limited time and a small budget for model usage. The regulations state they are accurate as of their publication date and apply to students from the 2022/2023 session onwards, so answers can become outdated. Malay-language questions are not covered in V1. |
| **Explicit Out-of-Scope Tasks** | Making individual academic decisions (for example whether a particular student will be dismissed, granted deferment or have an appeal approved). Looking up personal records such as grades or fee balances. Medical, legal, financial or mental health advice. Postgraduate regulations, other universities, and other UTM faculties' programmes. Anything not supported by the included documents. |
| **Evaluation Focus** | Does the assistant give correct, source-cited answers to routine regulation and programme questions, refuse out-of-scope or individual-decision requests, protect personal information, and respond fast enough to be useful to students? |

---

## Section 1: Functional Requirements

| # | Requirement: The system SHALL | Scope Boundary: SHALL NOT (where relevant) |
|---|---|---|
| FR1 | Answer questions about academic regulations (for example registration, credit load, withdrawal deadlines, grading, academic standing, deferment, credit transfer and examination rules) using the current UTM academic regulations. | Invent rules, numbers, deadlines or fees that are not supported by the source documents. |
| FR2 | Answer questions about the Bachelor in Artificial Intelligence with Honours programme (for example duration, total credits, structure and admission criteria) using the FAI and UTM programme pages. | Answer about other programmes, faculties or universities as if the sources covered them. |
| FR3 | Provide a source reference for every answer based on official documents, naming the document and the article, section or page, or the page URL. | Present unsupported information as if it came from an official source. |
| FR4 | Answer questions that need more than one part of a document (for example an article plus the appendix it refers to) and reference each part used. | Merge conflicting rules into one answer without saying that the sources differ. |
| FR5 | State clearly when the included documents do not contain enough information to answer, and redirect the student to the academic advisor or the faculty office. | Guess or fill the gap with general knowledge. |
| FR6 | Decline questions outside the supported scope and provide an appropriate redirect. | Decide an individual student's case (dismissal, deferment, appeal outcome, credit transfer approval), or give medical, legal, financial or mental health advice. |
| FR7 | Present answers in clear language suited to undergraduate students and explain UTM abbreviations used in the answer (for example KB, KS, KG, CGPA). | - |

---

## Section 2: Non-Functional Requirements


| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| NFR1 | Performance | The system shall keep interactive responses within the agreed latency target. | Measure end-to-end response time on a defined test workload of at least 50 queries. | p90 latency | < 5,000 ms |
| NFR2 | Correctness | Answers shall be factually consistent with the approved source documents. | Score a curated evaluation set of at least 50 questions (single-article, multi-part and programme questions) against an answer key built from the documents, using a defined rubric. | Correct response rate (%) | >= 85% |
| NFR3 | Cost | Inference cost shall remain within the project budget. | Record model/API usage over the evaluation workload. | Cost per 100 queries | < US$0.50 per 100 queries (illustrative) |
| NFR4 | Maintainability | Approved source updates (for example a new regulations edition) shall be incorporated without rebuilding unrelated components. | Run a controlled document-update test with a changed source document. | Update time | < 15 min |

---

## Section 3: Responsible AI Requirements

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| RAI1 | Hallucination / Transparency | The system shall give a verifiable source reference for document-grounded answers and indicate when sufficient supporting evidence cannot be found. | Evaluate the responses to the curated set for citation presence and check whether the cited source supports the answer. | Citation presence rate; citation-source match rate; unsupported-claim rate | Presence >= 95%; match >= 90%; unsupported claims <= 5% |
| RAI2 | Safety / Scope and over-reliance | The system shall refuse out-of-scope queries and individual-decision requests, redirect users appropriately, and remind users to confirm important decisions with their academic advisor. | Run a defined scope test set containing at least 20 out-of-scope or individual-decision queries. | Refusal-and-redirect success rate | >= 90% |
| RAI3 | Outdated information | The system shall show the name and date or edition of the source document with each document-based answer and warn that rules may change. | Check all document-based responses in the evaluation set for source name, date or edition, and the notice. | Source-version display rate | 100% |
| RAI4 | Privacy | The system shall not request, store or log personal identifiers (such as matric number or IC number) typed by users. | Run privacy test cases containing synthetic identifiers and inspect the stored logs and any outgoing model/API payloads. | Number of detected identifier retentions or transmissions | 0 |

---

## Reflection

*Which Responsible AI requirement in your project will be the most difficult to measure or enforce technically? Why?*

I think RAI1 would be the most difficult requirement to measure and enforce. Although it is easy to check whether an answer includes a citation, it is harder to make sure the source actually supports the information given. Someone may need to review the answers manually to confirm their accuracy. Also, some rules may have exceptions, such as credit-hour limits for final-semester students, so it can be difficult for the system to decide whether an answer is truly incorrect or just missing some details.

