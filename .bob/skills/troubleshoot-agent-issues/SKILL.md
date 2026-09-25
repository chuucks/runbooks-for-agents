---
name: troubleshoot-agent-issues
description: >-
  Diagnoses and resolves agent issues on any platform or framework (LangChain,
  CrewAI, AutoGen, OpenAI Assistants, IBM watsonx Orchestrate, Vertex AI Agent
  Builder, Azure AI Foundry, custom pipelines, and more). Guides the agent
  through gathering the issue description, locating trace logs, identifying
  root causes, and proposing or applying fixes. Use whenever the user asks to
  "troubleshoot my agent", "debug agent behavior", "why is my agent looping",
  "agent not calling the right tool", "tool call failing", "context lost
  between turns", "agent not routing", "unexpected agent behavior", or any
  similar phrasing. Always use this skill instead of improvising an ad hoc
  diagnosis — the structured steps are tuned for reliability across platforms.
  When running unattended, skip all clarifying questions, infer the platform
  from available files, and proceed directly to log analysis with reasonable
  defaults.
---

<!-- ============================================================
  CONFIGURATION — edit these defaults to match your project
  ============================================================
  log_dir: logs/          # Directory to scan for trace/log files
  default_framework: ""   # Pre-set platform if always the same
                          # (e.g. "LangChain", "CrewAI"). Leave blank
                          # to detect automatically from logs or user input.
  unattended: false       # Set to true when triggered by a schedule;
                          # skips all interactive clarifying questions.
  ============================================================ -->

## Usage Examples

- **Slash command with details:**
  `/troubleshoot-agent Agent fails to call the right tool after the second turn.`
- **Slash command without details:**
  `/troubleshoot-agent`
- **Natural language:**
  `Can you troubleshoot why my LangGraph agent is looping? Check logs/.`
  `Why is my CrewAI agent ignoring the tool output?`
  `My OpenAI Assistant is not calling the function I defined.`

# Agent Troubleshooting Skill

Guide the agent through gathering the issue description, identifying the platform and framework, locating and inspecting relevant trace logs, and diagnosing issues. This skill is platform-agnostic and applies to any agent framework or hosted platform.

## Step 1: Ensure Issue Description and Platform Are Known

> **Unattended mode:** If running via an automated schedule or the `unattended` config key is `true`, skip all interactive questions. Infer the platform from log file contents or filenames and proceed immediately to Step 2 using whatever context is available.

1. Check if the user has provided:
   - A clear issue description (error message, unexpected behavior, symptoms, or context).
   - The platform or framework in use (e.g., LangChain, CrewAI, AutoGen, OpenAI Assistants, IBM watsonx Orchestrate, Vertex AI Agent Builder, Azure AI Foundry, custom pipeline).
2. If either is missing **and running interactively**, ask for both before proceeding:
   - What happened, what was expected, and any error message or timestamp observed.
   - Which platform or framework is involved.
   - Wait for the user's response before continuing.

## Step 2: Check for Trace Files

1. List files in the `logs/` directory using `list_files` or `glob` on `logs/**`.
2. **If no files are present in `logs/`:**
   - Prompt the user: `"No trace or log files found in the 'logs/' folder. Please add the relevant trace/log files into the 'logs/' directory so we can proceed with the analysis."`
   - Ask if logs are available in another location and adjust accordingly.
   - Halt further diagnosis until files are provided.
3. **If files exist in `logs/`:**
   - Identify the most relevant log file(s) based on the issue description, error keywords, and file modification times.

## Step 3: Analyze Traces and Logs

1. Read the relevant log files using `read_file` or `grep` to locate errors, exceptions, stack traces, warnings, or failed tool/API calls.
2. Correlate findings with the issue description:
   - Identify failure points, error codes, failed tool invocations, or unexpected state transitions.
   - Pinpoint root causes such as misconfiguration, missing parameters, auth failures, schema mismatches, timeouts, context loss between turns, or upstream service errors.
3. Apply platform-specific pattern recognition:

   | Platform / Framework | Common Failure Patterns |
   |---|---|
   | **LangChain / LangGraph** | Tool schema mismatch, agent loop exit, missing memory binding, wrong chain order |
   | **CrewAI** | Task delegation failure, tool not registered to correct agent, crew output parsing error |
   | **AutoGen** | Termination condition triggered early, function map not populated, speaker selection error |
   | **OpenAI Assistants** | Function not called (tool_choice), run stuck in `requires_action`, file_id not attached |
   | **IBM watsonx Orchestrate** | Supervisor routing failure, collaborator `thread_id` mismatch, `@tool` decorator missing, context variable not propagated |
   | **Vertex AI Agent Builder** | Webhook timeout, session entity mismatch, intent not matched, fulfillment error |
   | **Azure AI Foundry** | Deployment not found, connection string invalid, API version mismatch, safety filter triggered |
   | **Custom pipelines** | HTTP error, serialization failure, env variable missing, dependency version conflict |

## Step 4: Present Findings and Recommendations

1. **Summary of the Issue:** Concise explanation of what failed and why.
2. **Evidence from Traces:** Relevant snippet(s) or log entries illustrating the root cause.
3. **Root Cause Analysis:** Clear breakdown of the underlying reason, contextualized to the platform or framework in use.
4. **Actionable Remediation Steps:** Concrete, step-by-step instructions or code/config fixes to resolve the issue.

> **Note on Project Context:**
> If a project-level architecture or conventions file is present (e.g., `AGENTS.md`, `README.md`, framework config files), read it to understand repository structure, naming conventions, and framework-specific guidelines when scoping the fix.

## Step 5: Offer to Implement Changes

1. **Ask for Confirmation:**
   - Ask the user whether they want you to implement the proposed remediation fixes directly.
   - Outline the specific files, configurations, or code components that will be modified.
2. **Execute Approved Modifications:**
   - Upon confirmation, make targeted edits to the affected files following the project's conventions and the platform's best practices.
   - Keep changes minimal and focused strictly on fixing the diagnosed root cause.
3. **Validate and Verify:**
   - Run relevant validation checks (tests, linting, syntax verification) if applicable to confirm the fix resolves the issue without regressions.
4. **Report Outcome:**
   - Summarize the applied changes and advise the user on next steps or verification in their environment.
