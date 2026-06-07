# Filesystem Overview

- Date: 2026/06/07
- Summary: A research meta-repository ("A collection of the newest Claude Code open source") that studies Anthropic's Claude Code CLI from multiple angles for academic/educational purposes. It bundles two large decompiled TypeScript snapshots of the official Claude Code source (`original-source-code`, `claude-code-source-code`), several clean-room Python reimplementations of increasing completeness (`claw-code`, `clawspring` / "Nano Claude Code"), standalone Python subsystem packages (`memory`, `multi_agent`, `skill`), and a shared `docs` analysis collection.

## Structure

```
chauncygu/
├── .gitignore                              — Git ignore rules (Python build artifacts, debug payloads)
├── LICENSE                                 — Repository license (MIT-style text)
├── README-CN.md                            — Chinese top-level README / project overview
├── README.MD                               — Main English README: news feed, content index, subproject comparisons
├── .git/                                   — Git version-control metadata (dot-folder leaf, not expanded)
├── claude-code-source-code/                — Decompiled Claude Code v2.1.88 source (TypeScript) packaged for research builds (expanded)
│   ├── .gitignore                          — Ignore node_modules / build-src / dist
│   ├── package.json                        — npm manifest: build scripts, esbuild + typescript devDeps
│   ├── package-lock.json                   — Locked npm dependency tree
│   ├── QUICKSTART.md                       — Quick build/run instructions for the decompiled source
│   ├── README.md                           — Detailed English analysis of the decompiled source
│   ├── README_CN.md                        — Chinese analysis of the decompiled source
│   ├── tsconfig.json                       — TypeScript compiler configuration
│   ├── docs/                               — Bilingual deep-dive write-ups (telemetry, hidden features, roadmap)
│   │   ├── en/                             — English analysis docs (telemetry, codenames, undercover mode, remote control, roadmap)
│   │   └── zh/                             — Chinese counterparts of the analysis docs
│   ├── scripts/                            — Build/transform pipeline (prepare-src, transform, stub-modules, build via esbuild)
│   ├── src/                                — Decompiled Claude Code application source (~1940 files across 37 subsystems)
│   │   ├── (root files)                    — Entry/core modules: QueryEngine.ts, Task.ts, Tool.ts, query.ts, tools.ts, context.ts, main.tsx, commands.ts, etc.
│   │   ├── assistant/                      — Assistant-message/response handling
│   │   ├── bootstrap/                      — Session-global state and startup singletons
│   │   ├── bridge/                         — Remote control / bridge mode (session, auth, transport)
│   │   ├── buddy/                          — "Buddy" assistant feature
│   │   ├── cli/                            — CLI argument/command wiring
│   │   ├── commands/                       — Slash-command implementations (~207 files)
│   │   ├── components/                     — Ink/React terminal UI components (~389 files)
│   │   ├── constants/                      — Shared constants (tool whitelists, etc.)
│   │   ├── context/                        — Context/system-prompt assembly
│   │   ├── coordinator/                    — Multi-worker coordinator mode
│   │   ├── entrypoints/                    — CLI entrypoints (cli.tsx, init)
│   │   ├── hooks/                          — Lifecycle/event hooks (~104 files)
│   │   ├── ink/                            — Ink TUI framework internals (~96 files)
│   │   ├── keybindings/                    — Keyboard binding definitions
│   │   ├── memdir/                         — Memory-directory persistence
│   │   ├── migrations/                     — Config/state migrations
│   │   ├── moreright/                      — Misc feature module
│   │   ├── native-ts/                      — Native/TS bridge helpers
│   │   ├── outputStyles/                   — Output style definitions
│   │   ├── plugins/                        — Plugin system
│   │   ├── query/                          — Query loop helpers
│   │   ├── remote/                         — Remote session support
│   │   ├── schemas/                        — Data schemas
│   │   ├── screens/                        — REPL/screen components
│   │   ├── server/                         — Local server mode
│   │   ├── services/                       — API clients and services (~130 files)
│   │   ├── skills/                         — Skill system
│   │   ├── state/                          — App state management
│   │   ├── tasks/                          — Task/agent task system
│   │   ├── tools/                          — Built-in tool implementations (~184 files)
│   │   ├── types/                          — Shared type definitions
│   │   ├── upstreamproxy/                  — Upstream proxy/relay
│   │   ├── utils/                          — Utility functions (~564 files)
│   │   ├── vim/                            — Vim-mode editing support
│   │   └── voice/                          — Voice-mode input
│   ├── stubs/                              — Build-time stubs (bun:bundle, macros, global.d.ts)
│   ├── tools/                              — Extra standalone tool prompts/constants (Workflow, Tungsten, TerminalCapture, etc.)
│   ├── types/                              — Extra top-level type modules (connectorText)
│   ├── utils/                              — Extra top-level util modules (attribution hooks, theme watcher, UDS client)
│   └── vendor/                             — Vendored native module sources (audio-capture, image-processor, modifiers, url-handler)
├── claw-code/                              — Earlier Python clean-room reimplementation mirroring Claude Code's subsystem layout (expanded)
│   ├── .gitignore                          — Ignore __pycache__ / archive
│   ├── README.md                           — claw-code project description
│   ├── .github/                            — GitHub config (dot-folder leaf, not expanded)
│   ├── assets/                             — Marketing/screenshot images (hero, tweets, reviews, star history)
│   │   └── omx/                            — Additional review screenshots
│   ├── src/                                — Python source mirroring the TS subsystem names (mostly __init__.py package stubs + core modules)
│   │   ├── (root modules)                  — QueryEngine.py, Tool.py, main.py, query.py, tools.py, commands.py, runtime.py, permissions.py, etc.
│   │   ├── assistant/                      — Assistant package stub
│   │   ├── bootstrap/                      — Bootstrap package stub
│   │   ├── bridge/                         — Bridge package stub
│   │   ├── buddy/                          — Buddy package stub
│   │   ├── cli/                            — CLI package stub
│   │   ├── components/                     — Components package stub
│   │   ├── constants/                      — Constants package stub
│   │   ├── coordinator/                    — Coordinator package stub
│   │   ├── entrypoints/                    — Entrypoints package stub
│   │   ├── hooks/                          — Hooks package stub
│   │   ├── keybindings/                    — Keybindings package stub
│   │   ├── memdir/                         — Memory-dir package stub
│   │   ├── migrations/                     — Migrations package stub
│   │   ├── moreright/                      — Misc package stub
│   │   ├── native_ts/                      — Native-TS package stub
│   │   ├── outputStyles/                   — Output styles package stub
│   │   ├── plugins/                        — Plugins package stub
│   │   ├── reference_data/                 — JSON snapshots of upstream subsystems/commands/tools used for parity auditing
│   │   │   └── subsystems/                 — Per-subsystem JSON reference snapshots (assistant, bridge, tools, utils, …)
│   │   ├── remote/                         — Remote package stub
│   │   ├── schemas/                        — Schemas package stub
│   │   ├── screens/                        — Screens package stub
│   │   ├── server/                         — Server package stub
│   │   ├── services/                       — Services package stub
│   │   ├── skills/                         — Skills package stub
│   │   ├── state/                          — State package stub
│   │   ├── types/                          — Types package stub
│   │   ├── upstreamproxy/                  — Upstream proxy package stub
│   │   ├── utils/                          — Utils package stub
│   │   ├── vim/                            — Vim package stub
│   │   └── voice/                          — Voice package stub
│   └── tests/                              — Pytest suite (porting workspace test)
├── clawspring/                             — "Nano Claude Code" v3.x: a fast, hackable single-file-core Python AI coding assistant (expanded)
│   ├── .gitignore                          — Ignore Python build artifacts / brainstorm outputs
│   ├── LICENSE                             — MIT license
│   ├── README.md                           — Extensive clawspring/Nano Claude Code documentation
│   ├── agent.py                            — Core agent loop
│   ├── clawspring.py                       — Main entrypoint / CLI (large monolithic core, ~145KB)
│   ├── cloudsave.py                        — Cloud session save/sync
│   ├── compaction.py                       — Conversation history compaction
│   ├── config.py                           — Configuration loading
│   ├── context.py                          — System/user context assembly
│   ├── demo.py                             — Demo runner
│   ├── memory.py                           — Memory module shim re-exporting the memory package
│   ├── providers.py                        — Model provider abstraction (open & closed source models)
│   ├── pyproject.toml                      — Python package manifest (deps: anthropic, openai, httpx, rich)
│   ├── requirements.txt                    — Pinned runtime + optional voice/STT dependencies
│   ├── skills.py                           — Skill module shim
│   ├── subagent.py                         — Subagent module shim
│   ├── tool_registry.py                    — Tool registration/dispatch
│   ├── tools.py                            — Built-in tool implementations (~42KB)
│   ├── demos/                              — Scripts that render the README demo GIFs (brainstorm, proactive, ssj, telegram)
│   ├── docs/                               — Multilingual READMEs, architecture, comparison docs, demo GIFs/images
│   │   ├── PR/                             — Feature PR write-ups (resume feature)
│   │   └── superpowers/                    — Design specs and enhancement plans
│   │       ├── plans/                      — Enhancement plan docs
│   │       └── specs/                      — Design spec docs
│   ├── mcp/                                — MCP client subsystem (client, config, tools, types)
│   ├── memory/                             — AI memory package (store, scan, consolidator, context, tools, types)
│   ├── multi_agent/                        — Multi-agent / subagent orchestration package
│   ├── plugin/                             — Plugin system (loader, recommend, store, types)
│   ├── skill/                             — Skill system (loader, executor, builtin skills, tools)
│   ├── task/                               — Task management package (store, tools, types)
│   ├── tests/                              — Pytest suite (compaction, mcp, memory, plugin, skills, subagent, task, voice, …)
│   └── voice/                              — Voice input package (recorder, STT, key-terms)
├── docs/                                   — Shared analysis docs, multilingual READMEs, and demo media for the meta-repo (expanded)
│   ├── README.CN.MD                        — Chinese README variant
│   ├── README.DE.MD                        — German README variant
│   ├── README.ES.MD                        — Spanish README variant
│   ├── README.FR.MD                        — French README variant
│   ├── README.JP.MD                        — Japanese README variant
│   ├── README.KO.MD                        — Korean README variant
│   ├── architecture.md                     — Architecture analysis write-up
│   ├── brainstorm_demo.gif                 — Demo GIF: brainstorm mode
│   ├── comparison_claude_code_vs_nano_v3.03_cn.md — Chinese feature comparison (Claude Code vs Nano)
│   ├── comparison_claude_code_vs_nano_v3.03_en.md — English feature comparison (Claude Code vs Nano)
│   ├── contributor_guide.md                — Contributor guidelines
│   ├── demo.gif                            — Main demo GIF
│   ├── logo-2.png                          — Project logo (variant 2)
│   ├── logo-v1.png                         — Project logo (variant 1)
│   ├── proactive_demo.gif                  — Demo GIF: proactive mode
│   ├── readme.md                           — clawspring demo notes
│   ├── screenshot.png                      — UI screenshot
│   ├── ssj_demo.gif                        — Demo GIF: ssj feature
│   ├── telegram_demo.gif                   — Demo GIF: Telegram integration
│   ├── update_readme_v3.0.md               — README changelog for v3.0
│   ├── update_readme_v3.02.md              — README changelog for v3.02
│   ├── PR/                                 — Feature PR write-ups
│   │   └── resume_Feature.md               — Resume-session feature description
│   └── superpowers/                        — Design specs and enhancement plans
│       ├── plans/                          — Enhancement plan docs
│       └── specs/                          — Design spec docs
├── memory/                                 — Standalone AI-memory Python package (extracted for study/reuse) (expanded)
│   ├── readme.md                           — Placeholder readme (TODO)
│   ├── __init__.py                         — Package init / exports
│   ├── consolidator.py                     — Memory consolidation logic
│   ├── context.py                          — Memory-injected context builder
│   ├── scan.py                             — Memory scanning/extraction
│   ├── store.py                            — Memory persistence store
│   ├── tools.py                            — Memory-related tool definitions
│   └── types.py                            — Memory data types
├── multi_agent/                            — Standalone multi-agent/subagent Python package (expanded)
│   ├── readme.md                           — Placeholder readme (TODO)
│   ├── __init__.py                         — Package init / exports
│   ├── subagent.py                         — Subagent execution model
│   └── tools.py                            — Multi-agent tool definitions
├── original-source-code/                   — Raw original Claude Code source snapshot (TypeScript) plus zipped archive (expanded)
│   ├── readme.md                           — One-line note: "Original Claude Code source code"
│   ├── src.zip                             — Zipped copy of the original source (~9.9MB)
│   └── src/                                — Unzipped original Claude Code source (~1904 files, same 37-subsystem layout as claude-code-source-code/src)
├── skill/                                  — Standalone skill-system Python package (expanded)
│   ├── readme.md                           — Placeholder readme (TODO)
│   ├── __init__.py                         — Package init / exports
│   ├── builtin.py                          — Built-in skill definitions
│   ├── executor.py                         — Skill execution engine
│   ├── loader.py                           — Skill discovery/loading
│   └── tools.py                            — Skill-related tool definitions
```

Note: `original-source-code/src/` and `claude-code-source-code/src/` are large mirror copies of the decompiled Claude Code application (~1900 files each, identical 37-subsystem layout). They are summarized at the subsystem-folder level above (under `claude-code-source-code/src/`) rather than dumping every file. `claw-code/src/` package subdirectories contain only `__init__.py` stubs alongside the listed core modules.
