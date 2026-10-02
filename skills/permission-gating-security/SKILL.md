---
name: permission-gating-security
description: Use when intercepting potentially dangerous shell commands or sensitive file operations with security approval gates.
---

# Permission Gating & Security

## Overview
Enforces security boundaries and human confirmation gates over terminal actions, protecting the developer's system from accidental or destructive executions.

## When to Use
- When executing commands that alter system state (installing packages, modifying configs, deleting files).
- When running git operations that alter remote histories (`git push --force`, rebase).
- When invoking network egress commands that transmit data outside the local network.

## Core Capabilities
1. **Command Risk Classification**: Categorizes commands into safe (read-only), moderate (local edits), and high-risk (system, delete, force).
2. **Terminal Confirmation Prompt**: Halts execution until the developer interactively presses enter or approves via keybinding.
3. **Auto-Approval Rules**: Allows trusted commands (e.g. `pytest`, `git status`, `ls`) to be pre-authorized via user configuration.
