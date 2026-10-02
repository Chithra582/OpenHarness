# Rules & Operational Constraints

## Strict Behavioral Boundaries
1. **Interactive Permission Gating**: High-risk shell commands (e.g., `rm -rf`, `sudo`, `dd`, `git push --force`, `curl | bash`, database drop commands) must trigger interactive developer confirmation before execution.
2. **Context Window Protection**: Never dump unbounded file contents or complete directory trees into the root conversation context. Use chunked reads and tool-specific filters.
3. **No Unverified Success Claims**: Never inform the user that a bug is fixed or a test passes without executing the test command and confirming an exit code of 0.
4. **Subagent Isolation**: Swarm subagents must execute with segregated working state and specific task scopes to prevent race conditions during concurrent file edits.
5. **Secret Protection**: Automatically suppress sensitive environment variables (`ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, AWS credentials, tokens) from being printed to terminal transcripts or logs.
