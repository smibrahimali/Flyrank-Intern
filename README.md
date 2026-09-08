# FlyRank Content Refresh Opportunity Model

An automated, offline analytical pipeline designed to predict content decay in high-volume publishing environments using historical search telemetry. Built for content editors and SEO analysts to proactively triage publishing resources before traffic loss occurs.

## 1. What It Does & Who It Is For
* **For Editors & Content Teams:** Translates 30,000 anonymized search performance rows into a prioritized action queue (`outputs/refresh_queue.csv`) with specific recommendations (`refresh`, `monitor`, `expand_and_refresh`, etc.).
* **For Engineers & Reviewers:** Demonstrates rigorous chronological validation splits (`client_holdout`), feature engineering on impression velocity, and explicit performance evaluation without relying on data leakage.

## 2. Setup & Installation
Follow these steps to reproduce the environment and inspect the pipeline:

\`\`\`bash
# Clone the repository
git clone https://github.com/smibrahimali/Flyrank-Intern.git
cd Flyrank-Intern

# Install required dependencies
pip install -r requirements.txt

# Run the pipeline/notebook analysis
jupyter notebook work/notebooks/capstone.ipynb
\`\`\`

## 3. Architecture & Data Flow
The pipeline operates as an offline analytical decision-support system:
1. **Ingestion:** Consumes anonymized search performance telemetry (`data/raw/content_refresh_anonymized.csv`).
2. **Feature Engineering:** Computes rolling velocity metrics, `log_impressions_90d`, `avg_position`, and `content_age_days`.
3. **Model Evaluation:** Trains multiple classifiers using a `client_holdout` validation strategy, optimizing for top-tier precision.
4. **Artifact Generation:** Exports structured JSON metrics (`outputs/model_results.json`, `outputs/summary.json`) and visualization assets (`outputs/charts/`).

## 4. v2 Evaluation Results
The Random Forest model was selected based on its superior `Precision@50` performance on the held-out validation split (30,000 scored rows, 54.2% declining-label rate).

| Model | ROC-AUC | Avg Precision | Precision@50 | Recall | F1 Score |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Random Forest (Selected)** | **0.750** | **0.618** | **0.740** | **0.744** | **0.640** |
| **Decision Tree** | 0.742 | 0.575 | 0.540 | 0.716 | 0.634 |
| **Logistic Regression** | 0.700 | 0.522 | 0.400 | 0.567 | 0.566 |
| **Baseline Rules** | 0.627 | 0.468 | 0.240 | -- | -- |

## 5. Limitations
* **Statistical vs. Causal:** The model calculates the statistical probability of content decay based on historical telemetry; it cannot predict unobserved external shifts such as sudden search engine core algorithm updates or macroeconomic changes in user search intent.
* **Reviewer Aid:** The output queue serves as a prioritization guide and requires manual editorial verification before executing content updates.

## 6. Transparency & AI Usage Statement
*This project was developed by Syed Muhammad Ibrahim Ali with AI assistance (Claude) utilized for code structuring, documentation scaffolding, and drafting editorial prose. All evaluation metrics, split strategies, validation reports, and architectural decisions were manually verified against local telemetry execution outputs.*
\`\``

---

**Assignment 8.2: 500–800 Word Retrospective**

Save this text as `work/retrospective.md` or include it in your submission package.

```markdown
# Internship Retrospective: From Raw Telemetry to Production-Ready Signals

When I started this internship in Week 1, my primary goal was straightforward: build a machine learning model that could predict content decay and stitch together a portfolio that proved I could handle messy, real-world data without falling into the trap of data leakage. Looking back across the entire track, the journey changed not just what I built, but how I approach software design and data validation.

### What I Set Out to Do vs. What Actually Happened
Initially, I expected the core challenge to be tuning complex neural networks or squeezing marginal gains out of hyperparameter grids. I quickly learned that real data science is 90% structural hygiene and validation design. Moving from naive random splits to a strict `client_holdout` validation strategy completely shifted my perspective. I realized that an inflated accuracy score built on a leaky pipeline is worse than useless—it is actively misleading. The hardest part wasn't writing the model training loop; it was engineering rolling-window velocity features, managing feature importances (`days_with_impressions` and `log_impressions_90d`), and ensuring zero temporal contamination between training sets and evaluation splits.

### What I Would Build Next
If I were to take this architecture to the next production tier, I would transition the current offline analytical pipeline into an active streaming microservice. Specifically, I would build an automated event-driven ingestion layer using Java and Spring Boot—drawing on the backend patterns I explored in my Padel ELO analytics engine—coupled with a live REST API endpoint that ingests daily search console webhooks, updates MongoDB document states, and automatically dispatches high-confidence decay alerts directly to editorial dashboards.

### The Three Most Transferable Things I Learned
1. **Defensive Data Validation Over Raw Complexity:** A simpler model (like our optimized Random Forest achieving 0.75 ROC-AUC and 0.74 Precision@50) backed by bulletproof chronological validation beats a black-box model built on contaminated data every single time. 
2. **Artifact-Driven Transparency:** Building inspectable outputs (`model_report.json`, structured CSV queues, and SVG charts) rather than hiding behind high-level summaries builds immediate trust with reviewers and engineering teams.
3. **AI as an Architectural Partner:** Using AI tools effectively requires treating them as rigorous sounding boards rather than shortcuts. By demanding rigorous pushback on leakage assumptions and split logic, I learned how to use AI to harden my code rather than just generate syntax.

This track bridged the gap between academic coursework in Computer Science and Data Science at Dawood University and actual production discipline. I am walking away with a live research paper, inspectable code artifacts, and a framework for building software that stands up to scrutiny.
