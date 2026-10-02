---
name: cli-agent-execution
description: Use when running terminal-based autonomous coding loops, bash commands, file editing, and test verification.
---

# CLI Agent Execution

## Overview
Executes autonomous, full-loop software engineering tasks directly in the developer terminal with real-time feedback and iterative tool use.

## When to Use
- When running interactive coding sessions in the CLI.
- When inspecting codebase structure, running compiler diagnostics, or executing test runners.
- When performing surgical code edits and multi-file refactoring.

## Core Capabilities
1. **Interactive Shell Integration**: Runs bash commands, streaming terminal output with interactive interrupt support (`Ctrl+C`).
2. **Surgical File Patching**: Replaces exact text blocks with built-in diff previewing and undo safety.
3. **Evidence-Based Loop**: Drives the Red-Green-Refactor cycle by requiring failing tests before writing fixes.
