---
name: mcp-ecosystem-bridging
description: Use when discovering, configuring, or communicating with external Model Context Protocol (MCP) servers and tools.
---

# MCP Ecosystem Bridging

## Overview
Connects OpenHarness to the expanding universe of Model Context Protocol (MCP) tools, resources, and servers.

## When to Use
- When augmenting coding workflows with external MCP tools (GitHub, PostgreSQL, Sentry, Brave Search).
- When exposing local OpenHarness commands as an MCP server for other desktop agents.
- When dynamically discovering tool schemas at runtime without code changes.

## Core Capabilities
1. **JSON-RPC Protocol Client**: Handles stdio and SSE transport protocols with automatic error recovery.
2. **Dynamic Tool Schema Ingestion**: Parses JSON schemas from `tools/list` into callable agent tools.
3. **Resource Introspection**: Reads external resources and injects them directly into conversation turns.
