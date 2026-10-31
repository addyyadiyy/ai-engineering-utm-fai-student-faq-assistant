# Week 2 Evaluation Criteria: UTM FAI Student FAQ Assistant

Capability buckets: Domain-specific capability, Generation capability, Instruction-following capability, Cost and latency.

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
|---|---|---|---|---|---|---|
| O3 / FR1, FR2, FR4 / NFR2 | Answer Correctness | Domain-specific capability | Run a curated set of at least 50 supported questions (single-article, multi-part and programme questions) and compare each answer with an answer key built from the official documents, using a rubric (correct / partly correct / incorrect). | Correct response rate | >= 85% | Students receive reliable answers to routine regulation and programme questions. |
| O2 / FR3 / RAI1 | Source Citation and Support | Instruction-following capability | Inspect every document-based response for a citation (document + article, section or page) and check whether the cited part supports the answer. | Citation presence rate; citation-source match rate | Presence >= 95%; match >= 90% | Students can check each answer against the official UTM document. |
| O2 / FR5, FR6 / RAI2 | Out-of-Scope Refusal and Redirect | Instruction-following capability | Test with at least 20 out-of-scope or individual-decision queries (for example "Will I be dismissed with my CGPA?", medical or legal questions) and check that the system refuses and redirects. | Refusal-and-redirect success rate | >= 90% | Students are not given individual rulings or advice outside the assistant's intended scope. |
| O3 / NFR1 | Response Latency | Cost and latency | Measure end-to-end response time over a workload of at least 50 queries. | p90 latency | < 5 s | Students get answers fast enough to use while planning registration. |
| O1 / FR7 | Response Clarity | Generation capability | Rate each response in a sample of 20 using a simple rubric: 1 = unclear, 2 = understandable, 3 = clear and abbreviations explained. | Share of responses scoring 2 or 3; mean clarity score | >= 90% score 2 or 3; mean >= 2.5 | Students can understand the academic information without needing to re-read the regulations. |
| O3 / RAI3 | Source Version Display | Instruction-following capability | Check all document-based responses for the source name, date or edition, and the "rules may change" notice. | Source-version display rate | 100% | Students know how current the information is and when to confirm with the faculty. |


## Reflection

*Which evaluation criterion is most important for determining whether your AI application will be useful to its intended users? Why?*

I think answer correctness is the most important because students rely on the AI assistant to get accurate information. If it gives the wrong credit-hour limit or deadline, students might make mistakes when registering subjects or miss important dates. Even if the assistant responds quickly and provides citations, it would not be helpful if the information is incorrect. Citations are also important because they allow students to check whether the answers are reliable.
