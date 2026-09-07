Explainer: Workflows, Agents, and the Model Context Protocol (MCP)
1. Workflows vs. Agents: The Core Distinction
In software development, terminology is often stretched for marketing purposes, and "agent" is currently the most overused word in the industry. To understand what an agent actually is, we must first look at what it is not: a workflow.
Workflows are structured, predictable paths where orchestration is hardcoded. The execution steps—whether sequential, parallel, or routed via conditional logic—are predefined by a human developer. The LLM acts as a reasoning or text-generation engine inside a rigid container. The control loop is external to the model; the code decides what happens next, and human intervention is usually required if an unexpected state is reached.
Agents, conversely, are systems where the LLM dynamically drives its own execution loop. Instead of following a hardcoded path, the model acts as a reasoning engine that perceives its environment, selects tools, evaluates outputs, and autonomously decides its next steps (including whether to loop, retry, or terminate) based on intermediate results.
Classification of the FL-04 Pipeline
The research and drafting pipeline built in FL-04 is strictly a workflow, not an agent. It follows a hardcoded, linear sequence: step 1 (Gather in NotebookLM) feeds into step 2 (Synthesize), which feeds into step 3 (Draft via Claude Project instructions), ending at step 4 (Review). At no point does the system dynamically decide to rewrite its own instructions, scrape external sources autonomously based on a failing test, or alter its structural routing. The control flow is entirely static.
2. Model Context Protocol (MCP) and its Three Primitives
Large Language Models have traditionally been locked inside isolated sandbox environments. They can reason over text provided in the prompt window, but they cannot natively interact with local filesystems, databases, or live external services without custom API wrapper code written for every single integration.
The Model Context Protocol (MCP) solves this by acting as a universal standard—often described as a "USB-C port for AI applications"—that connects LLM clients to secure data sources and tool providers. MCP establishes a client-server architecture built on three core primitives:
Tools: Executable functions exposed by an MCP server that the LLM can invoke to take action or modify state (e.g., executing a local terminal command, writing to a database, or modifying a file). The model decides when to call a tool based on the user's prompt.
Resources: Read-only data sources exposed by the server (e.g., local file contents, API schemas, or system logs) that can be loaded into the LLM’s context window as background information.
Prompts: Template workflows or predefined interaction patterns provided by the server that guide the user or model on how to interact with specific tools and data.
3. Evidence of MCP Integration: Three Tasks Chat Alone Could Not Do
By connecting a local filesystem and terminal MCP server to a Claude client, the system gains capabilities that pure LLM text generation cannot replicate.
Task 1: Inspecting Local File System State
Prompt to Client: "Check the exact file size and lines of code in work/outputs/baseline_action_score.csv."
Tool Execution: The client invoked the MCP filesystem tool (read_file / get_file_info) directly against the local repository.
Why chat alone fails: Chat models have no persistent memory of local machine state or un-uploaded local files. MCP allowed real-time inspection of the local disk.
Task 2: Executing Code and Reading Terminal Output
Prompt to Client: "Run the validation check script in our repository and report back any errors."
Tool Execution: The client invoked the MCP shell execution tool to run python scripts/validate_data.py.
Why chat alone fails: Chat models can write Python code, but they cannot execute it on a local machine, view compiler errors, or inspect runtime output without an external execution interface.
Task 3: Reading Protected Internal Configuration Files
Prompt to Client: "Read our root .gitignore file and verify if baseline_action_score.csv is properly excluded."
Tool Execution: The client used the MCP file-reading resource primitive to fetch the raw text of the hidden local configuration file.
Why chat alone fails: Without explicit file-system MCP binding, models cannot access dotfiles or local configuration directories outside the immediate chat context upload window.
(Note: Attach your screenshots showing the tool-use json/terminal blocks for these three tasks to your repository submission).
4. What FL-04 Needs to Become an Agent
To transition the FL-04 research pipeline from a static workflow into a true agentic system, it requires the introduction of a dynamic control loop with self-correction.
Specifically, the pipeline would need an evaluation agent node equipped with MCP tools. Instead of blindly drafting study notes and handing them to a human, the agent would:
Generate the initial draft via the existing synthesis steps.
Utilize an MCP database or file-writing tool to commit the draft to a staging environment.
Execute a validation script (via an MCP shell tool) that checks the markdown output against strict formatting rules, anti-fluff constraints, and citation linkage.
The Agentic Loop: If the validation script returns errors or detects ungrounded claims, the system loops back, feeds the error logs into its context, autonomously rewrites the draft to fix the violation, and re-runs the validation script without human intervention—stopping only when tests pass successfully.
