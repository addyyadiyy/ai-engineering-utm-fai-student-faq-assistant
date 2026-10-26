# **UTM FAI Student FAQ Assistant** 

An AI assistant that answers UTM Faculty of Artificial Intelligence undergraduate students' questions about academic regulations and programme information, grounded in official public UTM documents.


## **Problem Statement**

FAI undergraduates need quick, reliable answers about academic rules, programme structure and key dates, but this information is spread across a long regulations document and many separate files. A document-grounded AI assistant can answer in plain language and cite the official source, which a static FAQ page or plain keyword search does not do well.


## **Evidence of the Problem**

- The official undergraduate academic regulations have 10 sections, 42 articles and 11 appendices, with many cross-references between them (https://ugs.utm.my/wp-content/uploads/sites/8/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf).

- The FAI Undergraduate Registration Kit lists about ten separate items as individual Google Drive links (https://fai.utm.my/undergraduate-registration-kit/).

  
## **Initial Related Work / Existing Approaches**
- Static FAQ pages and document hubs (SPS FAQ, FAI Registration Kit): authoritative, but static and fragmented.

- Academic advisors (Article 2 of the regulations): contextual, but depend on advisor availability.

- General-purpose AI assistants: flexible, but may not be grounded in official UTM documents (Huyen, 2025).

  
## **Engineering Gap / Limitation**

No single tool gives FAI undergraduates quick answers grounded in official UTM documents with citations to the exact source.


## **Project Objectives**

- O1: Design a document-grounded AI FAQ assistant based on official, public UTM documents.
  
- O2: Develop the assistant with retrieval and appropriate guardrails.

- O3: Evaluate answer accuracy, groundedness, latency and safety.


## **Target Users**

UTM FAI undergraduate students (Bachelor in Artificial Intelligence with Honours), especially new and first-year students.


## **Data Sources**

- UTM Academic Regulations for Full-Time Undergraduate Studies Programmes (2026): https://ugs.utm.my/wp-content/uploads/sites/8/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf

- Bachelor in Artificial Intelligence with Honours, FAI programme page: https://fai.utm.my/?p=2248

- FAI Undergraduate Registration Kit: https://fai.utm.my/undergraduate-registration-kit/

Documents are linked, not stored in this repository.


## **Repository Structure**

docs/         Week 1 deliverables and later documentation
data/         Data notes (no copyrighted or restricted files committed)
src/          Source code
notebooks/    Colab notebooks
evaluation/   Test sets and results
prompts/      Prompt templates

## **Weekly Progress**
 <hr width="100%" size="2">
Week	   Focus	                Deliverable	       Status
 <hr width="100%" size="2">
W1	    System map + project scope	README + docs	✅
 <hr width="100%" size="2">
