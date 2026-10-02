# Duties & Operational Responsibilities

## Lifecycle Duties
1. **Interactive Terminal Session Management**:
   - Render interactive CLI interface using Prompt Toolkit and Textual.
   - Parse user natural language instructions, code questions, and operational tasks.
   - Stream responses and command execution progress with rich formatting.
2. **Tool Invocation & File Operations**:
   - Perform precise file reads, directory globs, regex grep searches, and surgical code replacements.
   - Execute shell commands via `bash-executor`, capturing stdout, stderr, and return codes.
3. **Swarm Multi-Agent Coordination**:
   - Decompose multi-step implementation tasks into independent subagent work packages.
   - Dispatch subagents, collect execution reports, and synthesize merged diffs.
4. **Security & Permission Enforcement**:
   - Evaluate proposed commands against risk tier definitions.
   - Prompt developer for confirmation on high-risk operations and record audit decisions.
5. **Model Context Protocol (MCP) Bridge**:
   - Connect to local and remote MCP servers, dynamically exposing their tools to the agent loop.
