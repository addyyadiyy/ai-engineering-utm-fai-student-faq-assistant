

Individual Data Quality Audit: UTM FAI Student FAQ Assistant
Student Name: Nur Adina binti Zahrulkamar
Student ID: A24AI0073
A. Project context
## Item Detail
Project title UTM FAI Student FAQ Assistant
Project purpose A document-grounded AI assistant that answers FAI
undergraduates’ questions about academic regulations
and programme information, citing the official source.
Application requirements R1 (FR1, FR4): answer regulation questions, including
ones that need an article plus its appendix.
R2 (FR3, RAI1, RAI3): cite document, page and version
for every document-based answer.
Data sources and types S01: UTM Academic Regulations for Full-Time
Undergraduate Studies, 2026 (PDF, 81 pages, 213
chunks).
S02: FAI Bachelor in AI with Honours programme page
saved as PDF (14 pages, 57 chunks).
Unstructured text, split into 500-character chunks
(JSONL). Without S01, R1 fails; without S02, programme
questions (duration, fees, admission) fail.
Sample inspected 16 chunks read by hand (8 per source; listed in the
appendix), mixing body text, cross-references, tables,
covers and very short chunks. I then checked them
against the original PDFs, whose SHA 256 values match
the project manifest. Two scripted rules ran on all 270
chunks. Sample findings are marked as such; counts
over all chunks are stated as counts.

B. Data quality assessment
Dimension Finding and evidence Impact
Completeness Pages 1, 2, 3, 11, 13, 15 and 81 of S01
gave no text. Checked against the PDF:
they are the front and back covers
(images), a graphic title page and blank
pages, so no rule text is missing. The
title, subtitle and "16th Edition" on pp.
2 and 4 were never extracted, and
chunks #1–2 hold glyph codes (/gid...)
in place of the editors' names. page is
empty in all 270 chunks.
Edition text is invisible to
the pipeline, and answers
cannot cite a page (R2).
Consistency The PDF contradicts itself: the cover
and title page say "16th Edition" but the
copyright page (p. 5, chunk #3) says
The assistant could quote
a wrong edition, credit
total or duration (R1, R2).

"First Edition 2026". Both manifests say
source_version: Not verified. S02 mixes
2026 FAI text with 2024 admission
pages (pp. 12–13, "Copyright © 2024");
p. 4 says 46 credit hours and CGPA 3.0
while p. 14 says a norm of 121 credits
and p. 13 lists a "6 Semester" AI
programme against "3 Years" on p. 14.
Accuracy Sample vs PDF: Table 1 (p. 26)
numbers match the page image (14, 1
and 2 weeks; total 42). Faithfulness to
layout is weaker: the table is split
across chunks #46 (caption, notes),
#47 (rows) and #48 (short semester,
42-week total). 175 of 213 S01 chunks
(82.2%) start mid-sentence. S02 p. 11
has a source typo, "USD 12,3000".
A retrieved chunk may lack
its heading or its total, so a
question like "how many
weeks per session?" needs
two or three chunks (R1).
Timeliness S01 is a 2026 edition. S02's admission
table (Malay, pp. 7–8) is for 2024/2025
and its fee pages (10–13) carry no date.
Admission and fee
answers may be outdated
## (RAI3).
Uniqueness No duplicate chunks (0 of 270). The
repetition is boilerplate: the S02 menu
and copyright block appears in 7 of 57
chunks.
Boilerplate competes with
real content in retrieval.

C. Two validation rules
Both rules were run with a short Python script over all 270 chunks.
Rule 1: readable text Rule 2: page reference present
Problem detected Extraction damage: font-
glyph codes instead of
words.
Chunks that cannot be cited to a
page (R2, FR3).
Condition checked Text contains /gid followed by
five digits.
Page is not empty.
Actual result Fail: 2 of 213 S01 chunks (#1,
#2; 94.6% of their characters
are glyph codes). 0 of 57 S02
chunks.
Fail: 270 of 270 chunks have an
empty page.
Action Send #1–2 to a person for
review. Rebuild them from
PDF p. 4 by hand, or exclude
them.
Re-extract page by page. Every
chunk text was found in the PDF
text, so each can be given a page
(60 S01 and 18 S02 chunks cross
a page break and need a start
page and end page).

A failed rule needs human review, not automatic deletion, because a failure is not always
a defect. The seven empty S01 pages a blank-page rule would flag are covers and blank
sheets,  harmless  to  drop.  The  same  flag  on  a  content  page  would  silently  delete
regulations. Chunks #1–2 are also the only record of the editors' credit, so a person must
decide whether to keep them.

D. Coverage and potential bias
Coverage findings Category 1, regulation questions (S01): sufficient on topic
presence, weak on multi-part answers.
Keyword counts show the main topics are present (registration 64
chunks, examination 67, GPA/CGPA 31, withdrawal 14, appeals
13, deferment 11), though a count shows a topic is mentioned,
not that it is answered. A rule and its appendix sit far apart: the
registration rule in chunk #56 (pp. 29–30) points to Appendix 3,
which starts in chunk #165 (p. 66).
Category 2, programme facts (S02): insufficient.
Duration, credits and fees conflict inside the file (see B). The
international fee table (p. 11) and the local table (p. 13, a 2024
page) give very different totals for the same degree, and neither is
dated. Admission rules are in Malay for 2024/2025. Specific
session dates are not in either source (the Registration Kit is not
used). These are findings from the sample and page checks, not a
full review of all 270 chunks.
Potential bias
and its effect
Pipeline-induced and representation bias (a risk, not
confirmed).
Prose rules in S01 survive chunking well, but table rows are
flattened and split and S02 is partly Malay and partly out of date.
The assistant could therefore answer prose regulation questions
better than fee, admission, credit or table based questions, and
answer the weaker ones confidently and wrongly to new
students. To investigate: build the planned 50-question
evaluation set with a fixed number of questions per category and
source, then compare the correct-answer rate by category and
flag any gap above 10 percentage points

E. Responsible data practices
- Source and traceability. Both sources are public UTM and FAI documents listed in
the  README. data/week3_manifest.json links  each PDF to its  prepared  output by
SHA-256, and I confirmed both PDFs match. The raw PDFs are not in the repository,
and source_version is Not verified.
- Personal  or  sensitive  data. No  student  records.  A  pattern  scan  found  no  email
addresses.  Six  S02  chunks  hold  faculty  and  emergency  phone  numbers

(organisational contacts), and S01 names the editors. The risk is low, but the scan was
pattern-based only.
- Basis  for  use. The  S01  copyright  page  (p.  5)  forbids  reproduction  without  UTM's
written  permission,  yet  the  prepared  chunks  hold  its  text.  The  lecturer  agreed  by
message  on  21  Nov  2026  that  committing  them  is  fine  for  coursework,  which  is
informal  and  not  UTM's  permission.  Both  manifests  record  access_rights:  Not
verified, and the README still says documents are "linked, not stored".
- Protecting the data and keeping it reliable. Keep raw PDFs out of the repository, re-
run the repeatability check after every fix, and make the two rules in C a pipeline stage
that stops on failure.
Data governance statement. This project uses public UTM documents only and stores no
personal data. Before the repository goes public or the assistant is released, the team
will obtain UTM's written permission or keep the chunks private, correct the README, and
record the confirmed edition and access basis in the manifests.

F. Engineering decision
Decision: GO WITH CONDITIONS.
The regulations data (S01) is good enough to build retrieval on now. The programme data
(S02) and the citation fields must be fixed before the assistant is evaluated or released.
Evidence-based justification.
- Not HOLD: S01 text matches the PDF where I checked it (Table 1), the empty pages
are covers and blanks, no chunk is empty or duplicated, and 211 of 213 S01 chunks
pass Rule 1. Every chunk can be traced to a page.
- Not GO: Rule 2 fails on all 270 chunks, the edition is unconfirmed and contradicted
inside the PDF, and S02 contains conflicting duration, credit and fee figures, 2024
pages and untranslated Malay.
- Limit: findings come from a 16-chunk sample plus scripted counts. I did not read
every chunk against every PDF page.
Two priority improvements.
- Rebuild extraction page by page so each chunk stores its page, add source_sha256
and  record_id  to  the pipeline_v1  output,  strip  glyph  codes  and  S02  menu  and
copyright lines, and split S01 at Article and Section headings and keep each table with
its caption. Re-run both rules to zero failures.
- Resolve the provenance and S02 conflicts: confirm the edition ("16th" vs "First Edition
2026") with UTM and record it, obtain written permission or keep the chunks private,
and ask the faculty which duration, credit and fee figures are current, dropping or
dating the 2024 pages.

## Appendix
Sample   inspected   (16   chunks).   Chunk   numbers   count   from   1   in
data/processed/prepared_text_S01.jsonl and _S02.jsonl. PDF pages were located by
searching each chunk's text in the original PDF.
Source Chunk PDF page What it shows
S01 #1 4 www.penerbit.utm.my, 2026, then glyph codes
S01 #2 4 Glyph codes only (editors' credit block)
S01 #3 5 "First Edition 2026" and author credit line
S01 #4 5 Malay copyright notice: no reproduction without
UTM's written permission
S01 #28 20 Definitions; "Minimum Credits" = 12; starts mid-
sentence
S01 #45 25 Short  semester  rule  pointing  to  "Section  10,
## Appendix"
S01 #56 29-30 Registration  rule  pointing  to  Section  10,
## Appendix 3
S01 #165 66 Start of Appendix 3: Course Registration Details
S02 #1 1 Copyright line, page menu, programme overview

S02 #6 Unclear One  word,  "Contact"  (text  repeats  on  many
pages)
S02 #8 2 End  of  objectives  list,  then  contact  phone
numbers
S02 #23 7 Malay   admission   table   header,   session
## 2024/2025
S02 #30 7-8 Malay sentence fragment (31 characters)
S02 #43 10 Heading only: International Undergraduate Fees
S02 #44 10 International  fee  table,  "rates  are  subject  to
change"
S02 #52 12-13 Footer links to other faculties, 2024 admission
pages

Checks against the original PDFs.
(a) SHA-256 of both PDFs equals the manifest.
(b) S01 pp. 1, 2, 3, 11, 13, 15 and 81 are covers, a graphic title page and blank sheets.
(c) S01 p. 4 shows the title and "16th Edition" as graphics; p. 5 states "First Edition
## 2026".
(d) S01 Table 1 (p. 26): 14, 1 and 2 weeks per semester, total 42 weeks, same as the
chunks.
(e) S02 p. 11 reads "USD 12,3000"; p. 4 says 46 credit hours and p. 14 says 121; p. 13
lists the AI programme as 6 semesters.




Validation output (all chunks).
## Check S01 (213) S02 (57)
Glyph-code chunks (Rule 1) 2 (#1, #2) 0
page empty (Rule 2) 213 57
Empty or duplicate chunks 0 0
Chunks under 50 characters 0 4 (#6, 30, 42, 43)
Chunks with copyright or menu text 2 (#1, #4) 7 (#1, 7, 10, 40,
## 49, 52, 56)
Chunks starting in lowercase 175 (82.2%) 20 (35.1%)
Chunk text found in PDF text 213 of 213 57 of 57
Chunks crossing a page break 60 18
processed vs pipeline_v1 text identical Yes Yes
