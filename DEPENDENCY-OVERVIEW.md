# DEPENDENCY-OVERVIEW.md

This repository contains multiple subprojects, each with its own dependency manifest. There is no single top-level manifest — dependencies are declared per subproject.

---

## 1. `claude-code-source-code/` — TypeScript (Node.js)

**Manifest:** `claude-code-source-code/package.json`

```json
{
  "name": "@anthropic-ai/claude-code-source",
  "version": "2.1.88",
  "engines": { "node": ">=18.0.0" }
}
```

### devDependencies

| Package | Version spec | Purpose |
|---|---|---|
| `esbuild` | `^0.27.4` | JavaScript bundler — used by `scripts/build.mjs` to produce `dist/cli.js` |
| `typescript` | `^6.0.2` | TypeScript compiler — type checking via `tsc --noEmit` |

**Note:** This package has no runtime dependencies in its `package.json`. The original `@anthropic-ai/claude-code` npm package bundles all runtime dependencies (Anthropic SDK, React/Ink, Commander.js, Zod, etc.) into the compiled output. The decompiled source archive only ships the build tooling needed to re-type-check and re-bundle the source.

---

## 2. `clawspring/` — Python (Nano Claude Code v3 / ClawSpring)

**Manifest:** `clawspring/pyproject.toml` (also `clawspring/requirements.txt`)

```toml
[project]
name = "clawspring"
version = "3.05.5"
requires-python = ">=3.10"
```

### Core dependencies

| Package | Version spec | Purpose |
|---|---|---|
| `anthropic` | `>=0.40.0` | Official Anthropic Python SDK — Claude API calls, streaming, tool use |
| `openai` | `>=1.30.0` | OpenAI Python SDK — used for OpenAI-compatible providers (OpenAI, Ollama, vLLM, LM Studio, DeepSeek, etc.) |
| `httpx` | `>=0.27.0` | Async HTTP client — used for WebFetch tool and custom API endpoints |
| `rich` | `>=13.0.0` | Terminal rendering — colored output, markdown rendering, diff display, progress bars |

### Optional dependencies

| Extra | Package | Purpose |
|---|---|---|
| `voice` | `sounddevice` | Cross-platform microphone capture for voice input mode |
| `vision` | `Pillow` | Image processing for screenshot/vision features |

### Voice mode additional (manual install, not declared in pyproject)

| Package | Purpose |
|---|---|
| `faster-whisper` | Local Whisper STT backend (recommended) |
| `openai-whisper` | Alternative local Whisper STT backend |
| `numpy` | Required by local Whisper backends and arecord RMS detector |

---

## 3. `claw-code/` — Python (Clean-room architectural rewrite)

**Manifest:** Not found (no `requirements.txt` or `pyproject.toml` in `claw-code/`).

The `claw-code` subproject is a structural/metadata-driven rewrite focused on architectural mapping. It does not appear to have its own declared Python dependencies — it is primarily a research artifact with JSON snapshot-driven metadata.

---

## 4. `original-source-code/` — TypeScript archive

**Manifest:** None. This directory contains only the raw source archive (`src.zip`) and the extracted `src/` tree. No `package.json` is present; it is an unmodified reference snapshot without build tooling.

---

## 5. Root-level Python packages (`memory/`, `multi_agent/`, `skill/`)

These packages have no independent manifests. They are re-exports / symlinked copies of the corresponding packages inside `clawspring/` and share its dependencies (`anthropic`, `openai`, `httpx`, `rich`).

---

## Summary Table

| Subproject | Language | Runtime | Key Dependencies |
|---|---|---|---|
| `claude-code-source-code` | TypeScript | Node.js >=18 | `esbuild ^0.27.4`, `typescript ^6.0.2` (dev only) |
| `clawspring` | Python | Python >=3.10 | `anthropic >=0.40.0`, `openai >=1.30.0`, `httpx >=0.27.0`, `rich >=13.0.0` |
| `claw-code` | Python | Python 3.x | No manifest declared |
| `original-source-code` | TypeScript | N/A (archive) | No manifest |
