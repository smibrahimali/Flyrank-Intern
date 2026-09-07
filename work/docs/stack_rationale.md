Automated Research & Drafting Pipeline Walkthrough

Pipeline Type: Draft, Critique, Revise (Applied to Technical Data Science Case Studies)

Author: Syed Muhammad Ibrahim Ali

1. Step Diagram & Pipeline Architecture

This workflow chains three distinct steps together to transform rough experimental code and metrics into a polished technical case study without manual drafting overhead.

[Step 1: Gather & Extract] 
       │  (Input: Raw Jupyter Notebook / Script Code & Metrics)
       ▼
[Step 2: Synthesize & Draft] 
       │  (Claude Project + Strict System Instructions: Generates structured markdown)
       ▼
[Step 3: Critique & Revise] 
       │  (Secondary prompt pass checking for temporal leakage, tone, and rigor)
       ▼
[Final Output] (Production-Ready Case Study Markdown)


2. Prompts & Configuration Used

Step 1 Configuration (Gather)

Tool: VS Code / Local Environment & Python Notebooks.

Action: Extract raw metrics and evaluation stats (e.g., Precision@K, grain size, train/test split timestamps) directly from w04_baseline_score.ipynb.

Step 2 Configuration (Synthesize & Draft)

Tool: Claude Project (Loaded with your Identity Kit, Content Map, and Style Notes).

System Prompt / Instructions:

"You are a technical writing assistant for a Data Science portfolio. Using the provided raw notebook code and metrics, draft a rigorous case study section. Maintain a direct, objective tone. Never use generic AI buzzwords or filler adjectives. Focus strictly on the problem, the leak-free data constraint, and the numerical evaluation."

Step 3 Configuration (Critique & Revise)

Tool: Secondary Prompt Pass in Claude.

Prompt:

"Review the drafted case study against these three failure criteria: 1) Does it inadvertently imply future data leakage? 2) Does it sound like a generic marketing template rather than a developer's log? 3) Are the numbers clearly attributed as observed or directional? Rewrite any offending sections while preserving the author's authentic voice."

3. Five Real Runs Documented

Run 1 (Customer Churn ML Pipeline):

Input: w02_ml_task_framing.ipynb data contract metrics.

Output Summary: Clean case study section detailing temporal train/test split isolation and Precision@50 evaluation.

Status: Success. Zero leakage detected.

Run 2 (Search Intelligence Data Contract):

Input: w03_data_contract.ipynb grain and feature buckets.

Output Summary: Structured breakdown of context vs feature fields with explicit exclusion logic.

Status: Success.

Run 3 (Baseline Action Score - Lane 2):

Input: w04_baseline_score.ipynb staleness and CTR rules.

Output Summary: Clear explanation of the STALE_HIGH_VOL_LOW_CTR reason code and heuristic score formulation.

Status: Success.

Run 4 (Padel Backend Schema):

Input: MongoDB document schema notes and Spring Boot validation code.

Output Summary: Technical overview of race-condition prevention during concurrent ELO updates.

Status: Success.

Run 5 (Portfolio Identity & Design Rationales):

Input: Hex codes (#FAFAFA, #1A1A1A) and typography rules (JetBrains Mono / Inter).

Output Summary: Formatted visual identity and layout specifications.

Status: Success.

4. Time-Saved Estimate

Manual Process: Writing, formatting, critique-checking, and refining a single technical case study from raw code takes roughly 90 minutes of deep focus. For 5 runs, manual execution totals ~450 minutes (7.5 hours).

Automated Pipeline Process:

Setup / Prompt Engineering Cost: 45 minutes (one-time).

Per-Run Execution & Human Verification: 10 minutes per run (50 minutes total).

Total Automated Time: 95 minutes.

Net Time Saved: ~5.5 hours across the initial batch, with compounding time savings for future documentation updates.

5. Known Failure Points & Required Human Review

While the pipeline drastically accelerates drafting, an automated system cannot catch everything. A human reviewer must actively check for:

Phantom Metrics: Ensuring the LLM does not hallucinate decimal accuracy or performance scores not present in the underlying Jupyter Notebook outputs.

Tone Drift: Catching overly dramatic phrasing (e.g., "revolutionary algorithm") and replacing it with sober, engineering-focused language.

Contextual Blind Spots: Verifying that data constraints (like missing GSC history) are accurately stated rather than glossed over.
