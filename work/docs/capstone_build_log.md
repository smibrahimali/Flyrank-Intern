Capstone MVP Build Log & Execution Guide (Checkpoint 1)
1. Build Overview & Platform Setup
Target Agent: DS Study & Research Coach (scoped from FL-06).
Platform: Claude Project integrated with local workspace file uploads and connected via MCP (Filesystem/Shell) for live code inspection.
Core Objective: Execute a complete study-coaching loop—ingesting user coursework, evaluating data science code for leakage or structural flaws, and providing source-grounded answers without mid-run human edits.
2. Build Log: Iteration, Breakages, & Scope Cuts
Entry 1: Initial Ingestion & System Prompt Binding
What I did: Created a new Claude Project workspace. Uploaded the core course markdown notes, baseline metric definitions, and feature engineering notebooks into the project knowledge base. Injected the system instructions defined in FL-06.
What broke: The model initially hallucinated external Python machine learning libraries (like xgboost automatic hyperparameter tuning wrappers) not present in the course materials.
What I changed: Tightened system prompt rule #1 to explicitly state: "If a methodology or library is not explicitly present in the uploaded knowledge base or user repository, state 'Not covered in uploaded course materials' rather than guessing."
Entry 2: Live Tool Connection (MCP Integration)
What I did: Configured local MCP filesystem access so the agent could read files directly from smibrahimali/Flyrank-Intern rather than relying solely on static uploads.
What broke: Initial path permission errors occurred when the MCP server attempted to read outside the designated workspace root directory.
What I changed: Restricted the MCP server root explicitly to the absolute path of the local Flyrank-Intern git repository. Tool calls can now successfully query work/notebooks/w04_baseline_score.ipynb and work/outputs/w05_metrics.json in real time.
Entry 3: Scope Cuts vs. FL-06 Spec
What I cut: I originally specced an automated background script that would run nightly evaluations of new CSV files written to work/outputs/.
Why I cut it: For Checkpoint 1 (MVP), an asynchronous background cron agent introduces unnecessary infrastructure complexity that risks breaking the 10-hour build window. I pivoted to an interactive request-response loop where the user triggers the evaluation explicitly, preserving the core analytical job while eliminating brittle scheduling overhead.
3. End-to-End Test Execution (The MVP Run)
To verify the MVP meets the evaluation criteria, execute the following test run for your video capture:
Prompt: "Review our Customer Churn feature vector notebook (work/notebooks/w03_feature_leakage_check.ipynb) via MCP, explain how temporal leakage is prevented, and quiz me with a follow-up Socratic question."
Agent Action (Tool Use): The agent invokes the MCP file-reading tool to fetch the exact code contents of the notebook.
Result (Output):
The agent accurately references the publish_age_days and impressions_30d extraction logic.
It confirms that future windows (like future_impressions_90d) were explicitly excluded in Section 4.
It ends with a Socratic question testing your understanding of train/test split stratification.
4. Instructions for Your Video Capture (2-Minute Screen Recording)
Open your screen recorder (OBS, QuickTime, or Loom). Ensure your microphone or system sound is ready if you want to narrate, though unedited silent captures are fully accepted.
Open your Claude Project interface with the MCP tools active.
Type the test prompt above live on camera.
Let the screen record the tool call executing (showing the file read) and the final generated response streaming in.
Stop recording without editing. Save the raw video file (approx. 1.5 to 2 minutes) and upload it directly along with this build log to your assignment submission card.
