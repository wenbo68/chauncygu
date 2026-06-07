# FILESYSTEM-OVERVIEW.md

## Summary

**chauncygu** (also published under the name *collection-claude-code-source-code*) is a research and educational collection that studies Anthropic's Claude Code CLI from multiple angles. It brings together four distinct subprojects:

| Subproject | Language | Nature |
|---|---|---|
| `original-source-code` | TypeScript | Raw leaked source archive (1,884 files, Mar 2026) |
| `claude-code-source-code` | TypeScript | Decompiled source archive v2.1.88 + bilingual analysis docs |
| `claw-code` | Python | Clean-room architectural rewrite (109 files) |
| `clawspring` | Python | Nano Claude Code v3 — fully runnable multi-agent coding assistant (~5,000 lines) |

The root of the repository also contains the shared Python packages (`memory/`, `multi_agent/`, `skill/`) that `clawspring` re-exports for backward compatibility.

---

## Annotated Directory Tree

```
chauncygu/
│
├── .gitignore                     # Python build artifacts ignored (__pycache__, *.pyc, dist/)
├── LICENSE                        # Apache-2.0
├── README.MD                      # Primary English README — full project overview and comparison table
├── README-CN.md                   # Chinese translation of the top-level README
│
├── .claude/                       # Claude Code agent configuration (hidden, not expanded)
│
├── docs/                          # Shared documentation and media assets
│   ├── readme.md
│   ├── architecture.md            # High-level architecture writeup
│   ├── contributor_guide.md
│   ├── comparison_claude_code_vs_nano_v3.03_cn.md
│   ├── comparison_claude_code_vs_nano_v3.03_en.md
│   ├── update_readme_v3.0.md
│   ├── update_readme_v3.02.md
│   ├── README.CN.MD               # Full Chinese README (85 KB)
│   ├── README.DE.MD               # German translation
│   ├── README.ES.MD               # Spanish translation
│   ├── README.FR.MD               # French translation
│   ├── README.JP.MD               # Japanese translation
│   ├── README.KO.MD               # Korean translation
│   ├── logo-2.png                 # Project logo v2
│   ├── logo-v1.png                # Project logo v1
│   ├── screenshot.png
│   ├── demo.gif
│   ├── brainstorm_demo.gif
│   ├── proactive_demo.gif
│   ├── ssj_demo.gif
│   ├── telegram_demo.gif
│   ├── PR/                        # Pull-request related docs
│   └── superpowers/               # Superpowers feature docs
│
├── original-source-code/          # Subproject 1 — raw leaked TypeScript source
│   ├── readme.md
│   ├── src.zip                    # Compressed archive of 1,884 TS/TSX files (~9.5 MB)
│   └── src/                       # Extracted full TypeScript source tree (same layout as claude-code-source-code/src)
│
├── claude-code-source-code/       # Subproject 2 — decompiled v2.1.88 source + analysis
│   ├── .gitignore
│   ├── package.json               # name: @anthropic-ai/claude-code-source, version: 2.1.88
│   ├── package-lock.json
│   ├── tsconfig.json
│   ├── QUICKSTART.md
│   ├── README.md                  # Deep analysis doc (51 KB, English)
│   ├── README_CN.md               # Chinese analysis doc
│   ├── src/                       # Full TypeScript source (~163 K lines)
│   │   ├── main.tsx               # CLI entry & REPL bootstrap (4,683 lines)
│   │   ├── query.ts               # Core agent loop (785 KB, largest file)
│   │   ├── QueryEngine.ts         # SDK/Headless query lifecycle
│   │   ├── Tool.ts                # Tool interface + buildTool factory
│   │   ├── commands.ts            # ~87 slash commands (~25 K lines)
│   │   ├── tools.ts               # Tool registration
│   │   ├── context.ts             # User input context handling
│   │   ├── history.ts             # Session history
│   │   ├── cost-tracker.ts        # API cost tracking
│   │   ├── setup.ts               # First-run initialization
│   │   ├── cli/                   # CLI infrastructure (stdio, structured transports)
│   │   ├── commands/              # ~87 slash command implementations
│   │   ├── components/            # React/Ink terminal UI (33 subdirectories)
│   │   ├── tools/                 # 40+ tool implementations (44 subdirectories)
│   │   ├── services/              # Business logic (22 subdirectories)
│   │   ├── utils/                 # Utility functions
│   │   ├── state/                 # App state management
│   │   ├── types/                 # TypeScript type definitions
│   │   ├── hooks/                 # React Hooks
│   │   ├── bridge/                # Claude Desktop remote bridge
│   │   ├── remote/                # Remote mode
│   │   ├── coordinator/           # Multi-agent coordination
│   │   ├── tasks/                 # Task management
│   │   ├── assistant/             # KAIROS assistant mode
│   │   ├── memdir/                # Long-term memory management
│   │   ├── plugins/               # Plugin system
│   │   ├── voice/                 # Voice mode
│   │   └── vim/                   # Vim mode
│   ├── docs/                      # In-depth bilingual analysis docs (en/ + zh/)
│   ├── scripts/                   # Build scripts (prepare-src.mjs, build.mjs)
│   ├── stubs/                     # Module stubs
│   ├── tools/                     # Top-level tool helpers
│   ├── types/                     # Global type declarations
│   ├── utils/                     # Top-level utility functions
│   └── vendor/                    # Third-party vendored dependencies
│
├── claw-code/                     # Subproject 3 — clean-room Python architectural rewrite
│   ├── .gitignore
│   ├── .github/                   # GitHub Actions workflows
│   ├── README.md
│   ├── assets/                    # Images and media
│   ├── src/                       # Python source (109 files)
│   │   ├── __init__.py
│   │   ├── main.py                # CLI entry (~200 lines)
│   │   ├── query_engine.py        # Core query engine
│   │   ├── runtime.py             # Runtime session management
│   │   ├── models.py              # Shared data classes
│   │   ├── commands.py            # Command metadata + execution
│   │   ├── tools.py               # Tool metadata + execution
│   │   ├── permissions.py         # Permission context
│   │   ├── context.py             # Context layer
│   │   ├── setup.py               # Workspace initialization
│   │   ├── session_store.py       # Session persistence
│   │   ├── transcript.py          # Session transcript storage
│   │   ├── parity_audit.py        # Parity audit vs TypeScript source
│   │   ├── reference_data/        # JSON snapshots (commands_snapshot.json, tools_snapshot.json)
│   │   ├── commands/              # Command implementations
│   │   ├── tools/                 # Tool implementations
│   │   ├── services/              # Business logic
│   │   ├── components/            # Terminal UI (Python)
│   │   ├── state/                 # State management
│   │   ├── types/                 # Type definitions
│   │   ├── utils/                 # Utilities
│   │   ├── remote/                # Remote mode
│   │   ├── bridge/                # Bridge modules
│   │   ├── hooks/                 # Hook system
│   │   ├── memdir/                # Memory management
│   │   ├── vim/                   # Vim mode
│   │   ├── voice/                 # Voice mode
│   │   └── plugins/               # Plugin system
│   └── tests/                     # Validation tests
│
├── clawspring/                    # Subproject 4 — Nano Claude Code v3 (ClawSpring)
│   ├── .gitignore
│   ├── LICENSE
│   ├── README.md                  # Full feature documentation (104 KB)
│   ├── pyproject.toml             # Package manifest (name: clawspring, version: 3.05.5)
│   ├── requirements.txt           # Core deps: anthropic, openai, httpx, rich
│   ├── clawspring.py              # Main entry point — REPL + slash commands (145 KB)
│   ├── agent.py                   # Agent loop: message format + tool dispatch
│   ├── cloudsave.py               # Cloud save support
│   ├── compaction.py              # Context compression (auto-compact)
│   ├── config.py                  # Config load/save/defaults
│   ├── context.py                 # System prompt builder (CLAUDE.md + git + memory)
│   ├── demo.py                    # Demo script
│   ├── memory.py                  # Backward-compat shim → memory/
│   ├── providers.py               # Multi-provider adapters + message conversion
│   ├── skills.py                  # Backward-compat shim → skill/
│   ├── subagent.py                # Backward-compat shim → multi_agent/
│   ├── tool_registry.py           # Central tool registry + plugin entry point
│   ├── tools.py                   # Tool dispatch + auto-registration (41 KB)
│   ├── demos/                     # Demo scripts and examples
│   ├── docs/                      # Additional documentation
│   ├── mcp/                       # MCP (Model Context Protocol) support
│   ├── memory/                    # Persistent memory package
│   ├── multi_agent/               # Multi-agent orchestration package
│   ├── plugin/                    # Plugin system
│   ├── skill/                     # Skill system package
│   ├── task/                      # Task management
│   ├── tests/                     # Test suite
│   └── voice/                     # Voice mode
│
├── memory/                        # Shared Python package — persistent memory (root-level copy)
│   ├── __init__.py
│   ├── readme.md
│   ├── consolidator.py            # Memory consolidation logic
│   ├── context.py                 # System-prompt injection + AI-ranked search
│   ├── scan.py                    # Index scanning, age/freshness helpers
│   ├── store.py                   # Save/load/delete/search memory entries
│   ├── tools.py                   # MemorySave, MemoryDelete, MemorySearch, MemoryList tools
│   └── types.py                   # MEMORY_TYPES definitions
│
├── multi_agent/                   # Shared Python package — multi-agent orchestration (root-level copy)
│   ├── __init__.py
│   ├── readme.md
│   ├── subagent.py                # AgentDefinition, SubAgentTask, SubAgentManager, worktree helpers
│   └── tools.py                   # Agent, SendMessage, CheckAgentResult, ListAgentTasks, ListAgentTypes
│
└── skill/                         # Shared Python package — skill system (root-level copy)
    ├── __init__.py
    ├── readme.md
    ├── builtin.py                 # Built-in skills: /commit, /review
    ├── executor.py                # Inline + fork execution modes
    ├── loader.py                  # SkillDef, file parsing, argument substitution
    └── tools.py                   # Skill, SkillList tools
```

---

## Key Entry Points

| Subproject | Entry point | How to run |
|---|---|---|
| `claude-code-source-code` | `src/main.tsx` (TypeScript) | `npm run build && node dist/cli.js` |
| `claw-code` | `src/main.py` | `python3 -m src.main [COMMAND]` |
| `clawspring` (Nano v3) | `clawspring/clawspring.py` | `pip install -e clawspring/ && clawspring` |

## Notes

- The root-level `memory/`, `multi_agent/`, and `skill/` directories are the same packages as `clawspring/memory/`, `clawspring/multi_agent/`, and `clawspring/skill/` — they exist at the root for direct import by users running `nano_claude.py` or scripts without installing the package.
- `original-source-code/src/` and `claude-code-source-code/src/` share the same git tree SHA (`7640f58e`), confirming they are identical copies of the decompiled TypeScript source.
- Hidden folder `.claude/` contains Claude Code agent/command/skill definitions and is not expanded per convention.
