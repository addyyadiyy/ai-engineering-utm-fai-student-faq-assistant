Week 1 Project Scope: UTM FAI Student FAQ Assistant

Project Name:	UTM FAI Student FAQ Assistant
Domain:	Higher Education / Student Information (academic regulations and programme information)

Problem Statement
Undergraduate students in the Faculty of Artificial Intelligence (FAI), UTM, often need quick answers about academic rules, programme structure and key dates, but this information is spread across a long regulations document and many separate files. A document-grounded AI assistant can answer these questions in plain language and point to the official source. AI engineering is a good fit because answering one question often means combining information from several places in a document, which a simple keyword search or a static FAQ page does not do well.

Evidence of the Problem
Evidence 1: The official undergraduate academic regulations are a single long document with 10 sections, 42 articles and 11 appendices. Many rules point to other articles, tables or appendices (for example, course registration in Article 4 refers to Appendix 3, and credit transfer in Articles 15 and 16 refers to Appendix 4), so one question can require reading several places. Source / citation: Universiti Teknologi Malaysia. (2026). Academic Regulations for Full-Time Undergraduate Studies Programmes. Penerbit UTM Press. https://ugs.utm.my/wp-content/uploads/sites/8/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf (The file name says "16th edition"; the copyright page of the PDF says "First Edition 2026". Cite by URL.)
Evidence 2: The FAI Undergraduate Registration Kit page gives new students about ten separate items as individual Google Drive links (Academic Rules, Academic Handbook, Academic Calendar, Grading System, Important Dates, Work Schedule, Program Coordinators, a briefing session recording, an "Others" folder, and a handbook for a different programme). Students have to open and search these one by one. Source / citation: Faculty of Artificial Intelligence, UTM. Undergraduate Registration Kit. https://fai.utm.my/undergraduate-registration-kit/

Initial Related Work / Existing Approaches
Study / solution 1: Static university FAQ pages and document hubs Source / citation: UTM School of Graduate Studies (SPS). (2017). Frequently Asked Questions (FAQ). https://sps.utm.my/wp-content/uploads/2017/08/FAQ.pdf ; FAI Undergraduate Registration Kit. https://fai.utm.my/undergraduate-registration-kit/ Key limitation: Authoritative, but static and fragmented across files. The SPS FAQ is aimed at postgraduate students and dated 2017, and the Registration Kit is a list of separate files with no search or Q&A.
Study / solution 2: Academic advisors Source / citation: Universiti Teknologi Malaysia. (2026). Academic Regulations for Full-Time Undergraduate Studies Programmes, Article 2 (Academic Advisory). https://ugs.utm.my/wp-content/uploads/sites/8/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf Key limitation: The regulations say advisors guide students and verify course registration. Advice depends on advisor time and availability (my assumption, not stated in the document).
Study / solution 3: General-purpose AI assistants such as ChatGPT or Claude Source / citation: Huyen, C. (2025). AI engineering: Building applications with foundation models. O'Reilly Media. (Chapter 1: foundation models and how they are adapted using prompting, RAG or fine-tuning.) Key limitation: Flexible, but may not be grounded in official UTM documents and can produce wrong or invented answers.

Engineering Gap / Limitation
There is no single tool that gives FAI undergraduates quick answers grounded in official UTM documents, with citations to the exact source, so students must search a long regulations document and several separate files themselves.

Initial Project Objectives
O1: To design a document-grounded AI FAQ assistant based on official, public UTM documents.
O2: To develop the assistant with retrieval and appropriate guardrails (scope check and citation requirement).
O3: To evaluate the assistant in terms of answer accuracy, groundedness (correct citations), latency and safety.

Target Users
UTM FAI undergraduate students (Bachelor in Artificial Intelligence with Honours), especially new and first-year students.

Data Sources
Source 1: UTM Academic Regulations for Full-Time Undergraduate Studies Programmes (Penerbit UTM, 2026): https://ugs.utm.my/wp-content/uploads/sites/8/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf
Source 2: Bachelor in Artificial Intelligence with Honours, FAI programme page: https://fai.utm.my/?p=2248
Source 3: FAI Undergraduate Registration Kit (links to the Academic Handbook, Academic Calendar and Grading System): https://fai.utm.my/undergraduate-registration-kit/

Similar to EduAdvisor
Both are retrieval-based assistants that answer student questions from official, public university documents and are meant to cite their sources.

Different from EduAdvisor
- Covers a single faculty (FAI) and one programme, instead of university-wide course and prerequisite advising.
- Focuses on academic regulations, programme information and key dates, not course-by-course prerequisite checking.
- Mixes a long PDF with web pages and linked files as data sources.

Responsible AI Risks
Risk 1: Hallucination. The model invents a rule or number (for example a credit limit or a deadline) that is not in the documents, so a student acts on wrong information.
Risk 2: Outdated information. The regulations state they are accurate as of their publication date, and the kit files and web pages can be updated, so answers may not match current rules and students may follow an old policy.
Risk 3: Over-reliance on an unofficial source. A student treats the answer as an official ruling (for example on academic standing, appeals or credit transfer) instead of confirming with their academic advisor or the faculty office. Mitigation: show a disclaimer and cite the source document every time.
