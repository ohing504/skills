# Multi-Agent Data Sources

The data collection paths in SKILL.md are Claude Code-specific. When running Phase 1 in other agents, use these alternative data sources.

---

## Quick Reference

| Agent | Session History | Memory/Rules File | Format |
|-------|----------------|-------------------|--------|
| Claude Code | `~/.claude/projects/{path}/` | `~/.claude/projects/{path}/memory/MEMORY.md` | JSONL |
| Cursor | `~/.cursor/projects/{path}/` | `.cursor/rules/*.mdc`, `.cursorrules` | SQLite (`.vscdb`) |
| Windsurf | `~/Library/Application Support/Windsurf/User/workspaceStorage/` | `~/.codeium/windsurf/memories/global_rules.md`, `.windsurf/rules/*.md` | Protobuf (limited access) |
| Cline | `~/Library/Application Support/Code/User/globalStorage/saoudrizwan.claude-dev/tasks/` | `.clinerules` | JSON |
| Continue.dev | `~/.continue/sessions/` | `.continue/rules/`, `~/.continue/config.yaml` | JSON |
| Aider | `.aider.chat.history.md` (project root) | `CONVENTIONS.md`, `.aider.conf.yml` | Markdown |
| GitHub Copilot | `~/Library/Application Support/Code/User/workspaceStorage/{hash}/chatSessions/` | `.github/copilot-instructions.md` | JSON |

Paths shown are macOS. For other platforms:
- **Windows**: Replace `~/Library/Application Support/` with `%APPDATA%\`
- **Linux**: Replace `~/Library/Application Support/` with `~/.config/`

---

## Per-Agent Details

### Cursor

Cursor's project structure mirrors Claude Code — directory names encode project paths.

**Session data**:
- `~/Library/Application Support/Cursor/User/workspaceStorage/{hash}/state.vscdb` — SQLite database with chat history
- `~/.cursor/projects/{project-path}/` — project-level data (newer versions)

The SQLite database contains a `cursorDiskKV` table. Useful keys:
- `composer.composerData` — primary chat list
- `workbench.panel.aichat.view.aichat.chatdata` — legacy chat storage

**Memory/rules** (most useful for understanding the user's working style):
- `.cursor/rules/*.mdc` — project-level rules (current format, Markdown with metadata)
- `.cursorrules` — project root, legacy single-file format

**Practical note**: The rules files (`.cursorrules`, `.cursor/rules/`) are the easiest to analyze and often reveal the user's priorities, coding standards, and workflow preferences. Start there before attempting to parse SQLite history.

### Windsurf (Codeium)

**Session data**: Stored in protobuf format without a public schema — **not reliably readable**. May be partially cloud-stored.

**Memory files** (the primary useful source):
- `~/.codeium/windsurf/memories/global_rules.md` — global rules across all projects
- `~/.codeium/windsurf/memories/` — auto-generated workspace memories (Markdown)
- `.windsurf/rules/*.md` — project-level rules

**Practical note**: Focus on the memories directory and rules files. Windsurf's auto-generated memories are particularly useful — they capture patterns the agent observed over time, similar to Claude Code's auto memory.

### Cline (formerly Claude Dev)

**Session data**:
```
~/Library/Application Support/Code/User/globalStorage/saoudrizwan.claude-dev/
├── tasks/
│   └── {task-uuid}/
│       ├── api_conversation_history.json   # Full conversation (richest source)
│       ├── ui_messages.json                # UI-rendered messages
│       └── task_metadata.json              # Model, cost, duration
└── state/
    └── taskHistory.json                    # Index of all tasks
```

The `api_conversation_history.json` is the richest source — full conversation turns including tool calls and model reasoning, in standard API message format.

**Memory/rules**: `.clinerules` in project root (if present).

### Aider

**Session data**: Written to the project root (git repo root) by default:
- `.aider.chat.history.md` — **complete conversation in human-readable Markdown** (best format for analysis)
- `.aider.input.history` — raw user commands only
- `.aider.llm.history` — raw API log (opt-in via `--llm-history-file`)

Note: Aider auto-adds these to `.gitignore`, so they won't appear in git history.

**Configuration**: `.aider.conf.yml` (project), `~/.aider.conf.yml` (global).

**Practical note**: Aider's `.aider.chat.history.md` is the most analysis-friendly format across all agents — plain Markdown with full conversations including code edits and commit messages.

### Continue.dev

**Session data**: `~/.continue/sessions/*.json` — UUID-named session files with full conversation turns.

**Configuration**: `.continue/rules/` (project), `~/.continue/config.yaml` (global).

### GitHub Copilot

**Session data**: `~/Library/Application Support/Code/User/workspaceStorage/{hash}/chatSessions/*.json` — per-workspace chat sessions. Can also export via VS Code command palette (`Chat: Export Session...`).

**Configuration**: `.github/copilot-instructions.md` (optional, project-level).

**Practical note**: Copilot sessions tend to be shorter and more code-completion focused, providing less narrative context than other agents. Most useful for identifying coding patterns and problem-solving approaches.

---

## Analysis Strategy for Non-Claude-Code Agents

When running in an agent without rich session history (or with opaque formats like Windsurf):

1. **Start with memory/rules files** — these are always readable and reveal user preferences, standards, and workflow patterns
2. **Use git history** — available everywhere, agent-independent, and reveals the same commit patterns, code quality signals, and decision-making evidence
3. **Check for readable session files** — Aider (Markdown), Cline (JSON), Continue (JSON) have accessible formats
4. **Fall back to current-session observation** — if no historical data is available, the reference will be based on the current interaction only (declare this in the confidence section)

The core analysis dimensions (work style, thinking process, communication, philosophy) apply regardless of agent. The data sources change, but the questions you're asking about the user remain the same.
