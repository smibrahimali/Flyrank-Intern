Automated Research & Drafting Pipeline: Walkthrough Document
1. Step Diagram & Pipeline Architecture
This pipeline automates the generation of source-grounded technical study notes and research syntheses for data science coursework.
[ Step 1: Gather (Source Ingestion) ] 
               │
               ▼ (Raw unstructured PDF/Web sources)
[ Step 2: Synthesize (NotebookLM Querying & Extraction) ]
               │
               ▼ (Structured semantic chunks & citations)
[ Step 3: Draft (Claude Project Instruction Guardrails) ]
               │
               ▼ (Raw Markdown draft matching Identity Kit tone)
[ Step 4: Review & Format (Skeptic Critique & Final Polish) ]
               │
               ▼ (Production-Ready Markdown Output)
2. Configuration & Prompts Used
Tool Stack:
Gather & Synthesize: Google NotebookLM (Source-grounded chunk extraction).
Draft & Review: Claude Project (Custom instructions enforcing a rigorous, technical, fluff-free tone).
Claude Project System Instructions:
"You are a technical writing assistant for an undergraduate Computer Science and Data Science student. Your job is to take synthesized source material and draft rigorous study notes or case study sections. Tone must be direct, technical, and grounded in numbers. Strictly avoid AI fluff, generic introductions, or buzzwords. Always reference specific data or principles provided in the source context."

3. Five Real Runs Documented
Run 1: Database Normalization & NoSQL Trade-offs
Input Source: University database systems lecture notes (PDF).
Output Summary: Clean breakdown of ACID compliance vs. BASE properties with MongoDB document structuring rules.
Status: Success. Zero hallucinated terms; fully grounded in source text.
Run 2: Temporal Leakage in Machine Learning Pipelines
Input Source: Feature engineering technical whitepaper (PDF).
Output Summary: Step-by-step definition of target leakage, train/test split isolation rules, and evaluation constraints.
Status: Success. Aligned with Lane 2 methodological guidelines.
Run 3: Concurrent State Management in Java (Spring Boot)
Input Source: Backend architecture documentation and API design specs.
Output Summary: Explanation of race conditions during ELO score recalculation and document locking strategies.
Status: Success. Code snippet syntax verified.
Run 4: Information Retrieval Metrics (Precision@K & MAP)
Input Source: Search engine ranking evaluation notes.
Output Summary: Mathematical definition of Precision@50 and its practical application to search relevance scoring.
Status: Success. Formulas and metric boundaries accurately rendered.
Run 5: Customer Churn Feature Engineering Heuristics
Input Source: Historical customer interaction logs and tabular datasets.
Output Summary: Identification of high-value behavioral indicators (recency, frequency, monetary drop-off) without future window contamination.
Status: Success. Validated against baseline scoring constraints.
4. Time-Saved Estimate (Honest Accounting)
Manual Process (per study note/case summary):
Reading & extracting source text: 45 minutes
Structuring notes and drafting framework: 40 minutes
Formatting and editing for clarity: 25 minutes
Total per document: ~110 minutes (5.5 hours for 5 documents).
Automated Pipeline Process:
Setup & configuration cost (one-time): 45 minutes
Execution time per document (NotebookLM + Claude prompt): 8 minutes
Human review & verification time: 10 minutes per document
Total for 5 documents: 45 mins setup + (18 mins × 5) = 2 hours total.
Net Time Saved: ~3.5 hours on the first batch, with marginal time dropping to under 15 minutes per subsequent document.
5. Known Failure Points & Required Human Review
Context Window Hallucinations: If a source document contains ambiguous terminology (e.g., poorly defined variables), NotebookLM may synthesize generalized definitions rather than context-specific ones. Human check required: Verify technical definitions against primary course materials.
Over-Compression: Claude's strict anti-fluff instructions can occasionally make explanations too terse, skipping necessary transitional logic. Human check required: Read through the draft to ensure conceptual continuity between steps.
Citation Drift: While NotebookLM anchors chunks well, page numbers or source section pointers occasionally mismatch if source files are updated. Human check required: Manually spot-check numeric claims against original PDFs.
