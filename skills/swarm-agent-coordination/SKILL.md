---
name: swarm-agent-coordination
description: Use when delegating sub-tasks across concurrent or sequential specialized worker agents to conserve context.
---

# Swarm Agent Coordination

## Overview
Coordinates swarms of specialized subagents, allowing the main harness agent to maintain a clean context while workers handle isolated sub-tasks.

## When to Use
- When executing complex plans with independent, parallelizable tasks.
- When performing codebase-wide research that would otherwise flood root session tokens.
- When pairing an implementer agent with an independent code reviewer agent.

## Core Capabilities
1. **Isolated Working State**: Subagents receive scoped instructions and return concise structured status reports.
2. **Dynamic Role Dispatch**: Spawns specialized agents (Researcher, Implementer, Reviewer, Tester) with custom model selections.
3. **Context Compaction Protection**: Prevents parent session degradation by keeping intermediate search and trial-and-error out of root history.
