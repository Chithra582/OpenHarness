# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **# Explainability & Decision Transparency Report** (`openharness`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** # Explainability & Decision Transparency Report (`openharness`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Autonomous CLI Coding Agent & Swarm Harness  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

# Explainability & Decision Transparency Report operates via a deterministic five-stage operational pipeline.

### 1. Decision Architecture

The runtime intake, state classification, evaluation, and execution tracking operate across a deterministic, five-stage pipeline:

```
+-----------------------------------------------------------------------------------+
|                        Deterministic OpenHarness Pipeline                         |
+-----------------------------------------------------------------------------------+
|  [Stage 1: Prompt Ingestion & Intent Resolution Gate]                             |
|     --> Ingest user instruction, parse context files, & formulate action plan     |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 2: Context Pruning & Tool Selection]                                      |
|     --> Filter relevant tools & files; resolve subagent vs. local execution        |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 3: Security Permission & Risk Assessment Gate]                            |
|     --> Evaluate bash commands against risk rubric; prompt user on high-risk ops  |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 4: Execution, Telemetry Capture & Tool Handling]                          |
|     --> Execute approved bash/file tools; stream outputs; handle exit codes       |
+-----------------------------------------+-----------------------------------------+
                                          |
                                          v
+-----------------------------------------------------------------------------------+
|  [Stage 5: Evidence-Based Verification & Turn Finalization]                       |
|     --> Execute test commands, verify exit code 0, & output final response        |
+-----------------------------------------------------------------------------------+
```

### 2. Decision Logic & Routing Formulations

Scoring
Tool selection affinity across available tools $t \in T$ is resolved by evaluating semantic relevance against task requirement $q$:

$$S_{\text{affinity}}(t) = w_1 \cdot \text{IntentMatch}(t, q) + w_2 \cdot \text{ContextRelevance}(t) + w_3 \cdot \text{ToolCostEfficiency}(t)$$

Where:
- $w_1 = 0.50$: Semantic similarity between tool capabilities and required action.
- $w_2 = 0.30$: Relevance to active workspace files currently under inspection.
- $w_3 = 0.20$: Computational cost factor favoring lightweight AST/file tools over heavy shell subshells.

Command risk evaluation $R_{\text{risk}}(c)$ for shell command $c$ determines whether interactive approval is required:

$$R_{\text{risk}}(c) = \sum_{k} v_k \cdot \mathbb{I}(c \text{ matches pattern } k)$$

Where matching destructive patterns ($c \in \{\text{rm}, \text{force}, \text{sudo}, \text{kill}\}$) yields $R_{\text{risk}} \ge 1.0$, immediately triggering mandatory user confirmation.

### 3. Thresholding & Refusal Decision Criteria

# Explainability & Decision Transparency Report enforces strict operational boundaries and deterministic refusal thresholds:
- **Refusal on ERR_PERMISSION_DENIED_BY_USER**: **Interactive User Rejection** halts execution with code `ERR_PERMISSION_DENIED_BY_USER`.
- **Refusal on ERR_SHELL_COMMAND_TIMEOUT**: **Process Execution Timeout** halts execution with code `ERR_SHELL_COMMAND_TIMEOUT`.
- **Refusal on ERR_UNVERIFIED_COMPLETION_ASSERTION**: **Premature Completion Claim** halts execution with code `ERR_UNVERIFIED_COMPLETION_ASSERTION`.
- **Refusal on ERR_CONTEXT_WINDOW_OVERFLOW**: **Context Token Limit** halts execution with code `ERR_CONTEXT_WINDOW_OVERFLOW`.
- **Refusal on ERR_SUBAGENT_RECURSION_LIMIT**: **Subagent Recursion Limit** halts execution with code `ERR_SUBAGENT_RECURSION_LIMIT`.

### 4. Fallback Decision Mechanism

Continuous operational stability is maintained through layered fault recovery:
- **Model Fallback Cascade**: High-level reasoning and synthesis default to `gemini-2.0-flash` with automatic failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

Human operators retain sovereign authority over the multi-agent execution lifecycle:
- **Consequential Action Sign-Off**: Sensitive and consequential actions require operator sign-off.
- **Offline Ledger Auditing**: Operators can verify execution records and state transitions offline.

---

## The Data It Uses

# Explainability & Decision Transparency Report operates under strict principles of data minimization, environment isolation, and privacy protection.

### 1. Ingested Input Data

The framework processes only operational data necessary to perform its functions:
- **Developer Instructions**: Natural language coding prompts, questions, and feature requests.
- **Local Source Files**: Source code, test scripts, project manifests, and diff patches.
- **Terminal Telemetry**: Process return codes, stdout/stderr streams, and execution timings.

### 2. Configuration & Reference Data

- **Configuration Schemas**: Declarative system policy files.

### 3. Base Model & Inference Lineage

- **Supported Models**: Anthropic Claude 3.5 Sonnet / Haiku / Opus, OpenAI GPT-4o / o1, DeepSeek, Local Ollama.
- **Runtime Environment**: Python 3.10+, Typer, Prompt Toolkit, Textual, Rich, websockets.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against indirect prompt injection, credential leakage, and unauthorized external API dispatch.
- **Local Environment Isolation**: Agent execution workspaces, intermediate scratchpads, and vector stores reside strictly within designated local project directories.
- **Automated Secret Scrubbing**: API keys, database credentials, and personal credentials are automatically redacted prior to embedding or logging.
- **Zero Commercial Monetization**: Prompts, intermediate reasoning trajectories, and task deliverables are never commercialized or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of # Explainability & Decision Transparency Report is essential for effective deployment.

### 1. Interactive Terminal Shell Stalling on Indefinite Prompts
- **Limitation**: Running interactive CLI programs (e.g. `vim`, `nano`, interactive shell prompts) can hang the headless execution loop.
- **Mitigation**: Disallow non-headless subshells and enforce non-interactive flags (`DEBIAN_FRONTEND=noninteractive`, `--yes`).

### 2. Context Truncation on Very Large Compiler Output Logs
- **Limitation**: Compiling large codebases can produce megabytes of verbose warning logs, threatening context limits.
- **Mitigation**: Truncate compiler outputs, preserving the first 50 and last 100 lines containing fatal errors.

### 3. Non-Idempotent Bash Commands During Automated Retries
- **Limitation**: Retrying non-idempotent shell commands (e.g. file appends) after partial failure can duplicate state.
- **Mitigation**: Require idempotent file edit tools over shell redirection for code modifications.

### 4. Rate Limiting on Upstream Frontier LLM API Providers
- **Limitation**: Heavy multi-turn coding sessions can trigger API provider token-per-minute rate limits.
- **Mitigation**: Implement automatic backoff with jitter and allow seamless switching to secondary API keys or providers.

### 5. High Memory Footprint During Massive Swarm Parallelism
- **Limitation**: Dispatching dozens of concurrent subagents can consume substantial local memory and network sockets.
- **Mitigation**: Enforce a concurrency semaphore (default 4 parallel workers) to bound system resource utilization.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & routing formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested input data & query streams | Section 1 | Verified |
| - Configuration & reference schemas | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Interactive Terminal Shell Stalling on Indefinite Prompts | Section 1 | Verified |
| - Context Truncation on Very Large Compiler Output Logs | Section 2 | Verified |
| - Non-Idempotent Bash Commands During Automated Retries | Section 3 | Verified |
| - Rate Limiting on Upstream Frontier LLM API Providers | Section 4 | Verified |
| - High Memory Footprint During Massive Swarm Parallelism | Section 5 | Verified |
