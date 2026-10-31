# Week 2 Architecture Decisions and Traceability: UTM FAI Student FAQ Assistant

Diagram: `docs/week2_architecture_v1.png`

V1 is a conceptual architecture. Components are labelled by function or capability. Specific models, tools and frameworks will be chosen in later topics.

## Step C1: Simplest architecture

Student → Language Model Service → Answer.

This flow can answer general questions, but it cannot satisfy several requirements on its own:
- It has no access to the official UTM regulations or programme pages (FR1, FR2, FR4, NFR2).
- It cannot point to a source or show a document version (FR3, RAI1, RAI3).
- It cannot reliably refuse out-of-scope or individual-decision questions (FR6, RAI2) or protect personal identifiers (RAI4).
- It has no way to record results for evaluation (NFR1, NFR2, NFR3).

## Step C2: Architecture needs from the requirements

| Requirement ID | What does the requirement need? | Architecture Response |
|---|---|---|
| FR1, FR2, FR4, NFR2 | Answers based on the official regulations and programme pages, including rules spread over several articles and appendices. | Add a **Knowledge Access** component and **Official UTM Documents** as the knowledge source. |
| FR3, RAI1, RAI3 | A source reference (document and article, section or page) and the document version or date with each answer. | The knowledge source keeps document name, article or page and version, and the application adds them, plus a "rules may change" notice, to the **Answer**. |
| FR5, FR6, RAI2 | Refuse out-of-scope and individual-decision queries, redirect to the academic advisor, and say when evidence is missing. | Add a **Scope / Safety Control** before the answer is generated. |
| RAI4 | Personal identifiers typed by users must not be stored or logged. | The **Scope / Safety Control** masks identifiers, and the **Evaluation Point** logs only masked content. |
| NFR1, NFR2, NFR3, RAI1 | Evidence about correctness, citations, latency and cost. | Add an **Evaluation Point** that records query, answer, sources and timing. |
| FR7 | Clear language and explained abbreviations. | Handled by the **Language Model Service** instructions, checked at the Evaluation Point with the clarity rubric. No extra component needed. |
| NFR4 | Update a source document without rebuilding unrelated parts. | Keep **Official UTM Documents** separate from the application and the Language Model Service. |

## Step C4: Architecture Decision Record (ADR)

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| Include knowledge access over official documents | Answers must be grounded in official UTM documents and cite them (FR1, FR3, FR4, NFR2, RAI1). | (a) Base model only; (b) keyword search inside the PDF (Ctrl+F style); (c) dynamic knowledge access that finds relevant passages for each question | Generic knowledge-access component | More system complexity and a source index to maintain, but better grounding and traceability. |
| Include explicit scope control | Out-of-scope and individual-decision queries must be refused and redirected (FR6, RAI2). | (a) Instructions in the model prompt only; (b) explicit scope-control component | Explicit scope / safety control | Some valid questions may be refused by mistake, and extra processing adds latency (NFR1). |
| Include an evaluation point that stores only masked content | Accuracy, citation, latency and cost must be measurable (NFR1, NFR2, NFR3, RAI1), while personal identifiers must not be retained (RAI4). | (a) Manual testing only, with no records; (b) log everything; (c) log masked queries, answers, sources and timing | Evaluation point with masked logging | Less detail in the logs and some extra processing, but evidence for evaluation without keeping personal data. |
| Show source version and a disclaimer with every answer | Documents can be outdated and answers can be mistaken for official rulings (RAI2, RAI3). | (a) Disclaimer only on the site; (b) show version and disclaimer in every answer | Show in every answer | Longer answers, but students see how current the information is each time. |

## Initial Requirements Traceability Matrix

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
|---|---|---|---|---|
| O1 | FR7 (clear language, abbreviations explained) | Response Clarity | Language Model Service instructions; Answer format | Clarity rubric scores on a sample of 20 responses |
| O2 | FR3 / RAI1 (source reference for document-based answers) | Source Citation and Support | Knowledge Access; Official UTM Documents; Answer with source reference | Citation presence rate and citation-source match rate on the evaluation set |
| O2 | FR6 / RAI2 (refuse out-of-scope and individual-decision queries) | Out-of-Scope Refusal and Redirect | Scope / Safety Control | Refusal-and-redirect success rate on at least 20 test queries |
| O3 | FR1, FR2, FR4 / NFR2 (correct answers from official documents) | Answer Correctness | Knowledge Access; Language Model Service; Evaluation Point | Correct response rate on the curated 50-question set |
| O3 | NFR1 (latency) | Response Latency | Whole pipeline; Evaluation Point timing | p90 latency over at least 50 queries |


## Reflection

*Which architecture decision involved the most important trade-off? Which requirement drove the decision, and what trade-off did you accept?*

I think the most important decision was adding a scope control to make sure the AI only answers questions related to its purpose. This was important because students might ask personal questions, such as whether they will be dismissed from university. The downside is that the AI might reject some questions that it could actually answer, and it may take a little longer to respond. However, I think this is necessary to make sure the answers stay relevant and appropriate.
